```
sensitiveHeaders会过滤客户端附带的headers
例如：
sensitiveHeaders: X-ABC
如果客户端在发请求是带了X-ABC，那么X-ABC不会传递给下游服务

ignoredHeaders会过滤服务之间通信附带的headers
例如：
ignoredHeaders: X-ABC
如果客户端在发请求是带了X-ABC，那么X-ABC依然会传递给下游服务。但是如果下游服务再转发就会被过滤

还有一种情况就是客户端带了X-ABC，在ZUUL的Filter中又addZuulRequestHeader("X-ABC", "new"),
那么客户端的X-ABC将会被覆盖，此时不需要sensitiveHeaders。如果设置了sensitiveHeaders: X-ABC，那么Filter中设置的X-ABC依然不会被过滤。
```

```
所以解决该问题的思路也很简单，我们只需要通过设置sensitiveHeaders即可，设置方法分为两种：

全局设置：
zuul.sensitive-headers=
指定路由设置：
zuul.routes.<routeName>.sensitive-headers=
zuul.routes.<routeName>.custom-sensitive-headers=true
```

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


作者：挨踢的懒猫链接：https://juejin.im/post/5a9fc7f96fb9a028d4442285来源：掘金著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。





```
一、Header

1.1、敏感header的设置

一般来说，可在同一个系统中的服务之间共享Header，不过应尽量防止让一些敏感的Header外泄。

zuul:
  routes:
    provide-user: 
     sensitive-headers: Cookie,Set-Cookie
说明：敏感的header不会传播到下游去，也就是说此处的Cookie,Set-Cookie不会传播的其它的微服务中去

1.2、忽略的Header

可以使用zuul.ignored-headers属性丢弃一些Header，如：

zuul:
  routes:
    provide-user: 
     sensitive-headers: Cookie,Set-Cookie
  ignored-headers: Authorization
说明：忽略的header不会传播到下游去，也就是说此处的Authorization不会传播的其它的微服务中去，作用与上面敏感的Header差不多，事实上sensitive-headers会被添加到ignored-headers中。



注意：

 1、默认情况下zuul.ignored-headers是空的

 2、如果Spring Security在项目的classpath中，那么zuul.ignored-headers的默认值就是Pragma,Cache-Control,X-Frame-Options,X-Content-Type-Options,X-XSS-Protection,Expires，所以，当Spring Security在项目的classpath中，同时又需要使用下游微服务的Spring Security的Header时，可以将zuul.ignoreSecurity-Headers设置为false
```

 https://www.cnblogs.com/liaojie970/p/9158991.html 





# Zuul的配置-传递header

```
zuul默认对转发的request，会把header清空，为了传递原始的header信息到最终的微服务，在配置加上：(对，你没看错，就是为空，yml格式也是）
zuul.sensitive-headers=

上面是全局的，也可定义局部了，会覆盖全局的设置：
zuul.routes.xxxapi-xxx.sensitive-headers=
或者个性化：
zuul.routes.<xxxapi-xxx>.custom-sensitive-headers=true
zuul.routes.<xxxapi-xxx>.sensitive-headers=Cookie,Set-Cookie,Authorization

作者：GZ太阳雨
链接：<a href='https://www.jianshu.com/p/59b9734022be'>https://www.jianshu.com/p/59b9734022be</a>
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

# Spring Cloud实战小贴士：Zuul处理Cookie和重定向

 http://blog.didispace.com/spring-cloud-zuul-cookie-redirect/ 