# Zuul使用问题记录

为了屏蔽后台微服务细节，同时对分布式的微服务访问提供负载均衡，我们采用了`Spring Cloud`的`Zuul`作为API网关。

由于前期机器较为紧张，API网关，注册中心，以及其他的微服务都是部署在同一台机器上，但是部署了两套，实现了分布式。

## 问题一：

> 访问同一个服务A的接口时，会间歇性的出现`Zuul Exception`。

刚开始以为是随机出现，把怀疑的重点放在了Zuul上面，但是Zuul的服务代码极少，基本就是一堆配置文件，不应该出现这种问题。于是开始在其他方面继续排查。

在检查服务注册中心的时候发现，注册中心有两个实例，每一个上都只有一个A的实例，而且该实例的IP与注册中心的IP一致。这说明A服务的两个实例都正常启动了，只是注册的时候出了问题，很有可能是网络不通。

我在其中一台机器上用`telnet`测试另外一台机器A服务的端口，发现是不通的，这就是确定了是网络的问题。

此时我恍然大悟，忘记开放端口了，然后Zuul有负载均衡，如果把流量代理到另外一台机器上，就会出现`Zuul Exception`，而且出现的概率大概是50%。

## 问题二：

> 登录成功之后，所有需要鉴权的接口调用都是401错误。

### 问题分析：

登录接口时不需要token做鉴权的，其他接口需要鉴权，既然是未授权，那可能是token鉴权出了问题。

运行日志显示，明明是管理员账户登录，最后身份却变成了匿名用户，这个非常奇怪，但是又实在找不到突破口，只能增加调试日志。

后来在日志中发现，所有需要鉴权的接口请求Header中都没有`Authoration`字段。客户端请求的时候，是填了这个字段的。但是最后到了微服务那里就没有了。

那问题应该出在代理那里，首先可以排除nginx,因为nginx反向代理没有配置丢弃header，而且之前也测试过nginx直接反向代理过去，没有任何问题。

既然Nginx的嫌疑解除，现在就只剩下Zuul了。我查了一下相关的书和博客，基本了解到，zuul会根据配置选择性忽略一些header。

这里的配置，主要涉及到两个字段`sensitive-headers`和`ignored-headers`。

当时急于解决问题，没有仔细研究，我就天真的信了不少地方讲到的配置`sensitive-headers`就可以了。 `sensitive-headers:Cookie,Set-Cookie和Authoration` 然而配置结果是一点用都没有。

没办法，眼看就要解决问题了，现实却不遂人愿，只能退回去老老实实研究`sensitive-headers`和`ignored-headers`的意思。

`sensitive-headers` 何为敏感，其实就是指那些字段内容信息比较敏感，不希望传递给下游微服务。

`ignore-headers` 这个比较好理解，就是制定忽略的Header字段列表。

怎么这俩好像说的意思好像差不多啊，都是要忽略一部分Header，这个有点出乎我意料？我赶紧又把《Spring Cloud与Docker 微服务架构实战》关于Zuul的部分读了一遍，留意到一段注释：

> 事实上，sensitive-headers会被添加到ignored-headers中，详见代码....

这段话让我更加疑惑了，赶紧去查源代码一探究竟。一番查找之后，我找到如下代码：

`ZuulProperties.java`：

```
	/**
	 * Headers that are generally expected to be added by Spring Security, and hence often
	 * duplicated if the proxy and the backend are secured with Spring. By default they
	 * are added to the ignored headers if Spring Security is present and ignoreSecurityHeaders = true.
	 */
	public static final List<String> SECURITY_HEADERS = Arrays.asList("Pragma",
			"Cache-Control", "X-Frame-Options", "X-Content-Type-Options",
			"X-XSS-Protection", "Expires");
复制代码
	/**
	 * List of sensitive headers that are not passed to downstream requests. Defaults to a
	 * "safe" set of headers that commonly contain user credentials. It's OK to remove
	 * those from the list if the downstream service is part of the same system as the
	 * proxy, so they are sharing authentication data. If using a physical URL outside
	 * your own domain, then generally it would be a bad idea to leak user credentials.
	 */	
	private Set<String> sensitiveHeaders = new LinkedHashSet<>(
			Arrays.asList("Cookie", "Set-Cookie", "Authorization"));
			
复制代码
	public Set<String> getIgnoredHeaders() {
		Set<String> ignoredHeaders = new LinkedHashSet<>(this.ignoredHeaders);
		if (ClassUtils.isPresent(
				"org.springframework.security.config.annotation.web.WebSecurityConfigurer",
				null) && Collections.disjoint(ignoredHeaders, SECURITY_HEADERS) && ignoreSecurityHeaders) {
			// Allow Spring Security in the gateway to control these headers
			ignoredHeaders.addAll(SECURITY_HEADERS);
		}
		return ignoredHeaders;
	}    
复制代码
```

`PreDecorationFilter.java`:

```
				if (!route.isCustomSensitiveHeaders()) {
					this.proxyRequestHelper
							.addIgnoredHeaders(this.properties.getSensitiveHeaders().toArray(new String[0]));
				}
				else {
					this.proxyRequestHelper.addIgnoredHeaders(route.getSensitiveHeaders().toArray(new String[0]));
				}
复制代码
```

从以上代码可知： 1.如果配置了ignoreSecurityHeaders为true(默认为false),`Pragma, Cache-Control, X-Frame-Options, X-Content-Type-Options, X-XSS-Protection, Expires`这些安全相关的Header都会被添加到`ignored-headers`中

2.sensitiveHeaders 的全局默认配置就是`Cookie, Set-Cookie, Authorization`。 3.sensitiveHeaders也会被加入到`ignored-headers`, 所以默认情况下`Cookie, Set-Cookie, Authorization`就不会传递到下游的微服务。 4.综上所述默认情况下： `ignoredHeaders` = `ignoredHeaders` + `sensitiveHeaders` + `securityHeaders`

好了，要解决上面遇到的问题，很简单,只需要修改配置`ignored-headers`即可：

```
sensitive-headers:
复制代码
```

什么都不填，也就没有了敏感header，我们所需要的Header也就不会被过滤掉。

## 问题三：

> 所有/info/xxx接口 出现ZuulException

刚开始看到这个问题是懵的，一直以为是Zuul的配置问题，但是反反复复研究了很多遍配置，也尝试了修改，最后确定配置没有问题。 单机环境是可以的，放到Spring Cloud环境中就不行了，问题还是与Spring Cloud有关，可是就是不知道哪里有关。很长一段时间陷入了毫无头绪的境地。 后来突然眼前一亮，spring boot acuator监控的接口是/info开头的，会不会是冲突了。我马上把info换成information, 果然好了。确定是/info接口被路由到zuul的actuator的接口上，但是actuator我又没有开放