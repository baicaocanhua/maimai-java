 最近使用的spring boot项目中需要集成redis集群，连接redis时需要设置密码，但是设置密码之后发现boot集成的redis不支持设置密码（redis单节点也是一样），一旦设置密码后就会报错：Jedis does not support password protected Redis Cluster configurations

      我使用的boot版本是1.4.x

<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.4.7.RELEASE</version>
    <relativePath /> <!-- lookup parent from repository -->
</parent>
     我看了一下源码，源码如图：

   

     源码中一旦发现你设置了密码的话，直接抛出异常，醉了。。。。。。   
    
    然后我看了默认集成的Redis相关版本，其中jedis版本是2.8.x，spring-data-redis的版本是1.7.x，对应的版本中JedisCluster的构造函数，没有一个包含密码参数。
    
    解决方案一：替换jedis和spring-data-redis的版本
    
    修改前maven依赖如下

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
    修改后maven依赖如下

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
    <exclusions>
        <exclusion>
	    <groupId>redis.clients</groupId>
	    <artifactId>jedis</artifactId>
	</exclusion>
	<exclusion>
	    <groupId>org.springframework.data</groupId>
            <artifactId>spring-data-redis</artifactId>
	</exclusion>
    </exclusions>
</dependency>

<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.9.0</version>
</dependency>

<dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-redis</artifactId>
    <version>1.8.0.RELEASE</version>
</dependency>
     解决方案二：升级boot版本到1.5或者以上

     升级前maven依赖如下

<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.4.7.RELEASE</version>
    <relativePath /> <!-- lookup parent from repository -->
</parent>
     升级后maven依赖如下

<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.2.RELEASE</version>
    <relativePath /> <!-- lookup parent from repository -->
</parent>
     boot 1.5.x版本中jedis版本默认是2.9.x，spring-data-redis的版本默认是1.8.x，所以可以正常使用redis密码进行验证

     application.properties 文件中 redis配置示例

#spring.redis.host = 140.143.23.94
spring.redis.password = 123456
#spring.redis.port = 6379
# 连接超时时间 单位 ms（毫秒）
spring.redis.timeout = 6000
spring.redis.cluster.nodes = 12.2.3.14:7001,12.2.3.14:7002,12.2.3.14:7003,12.2.3.14:7004
# 连接池中的最大空闲连接，默认值也是8
spring.redis.pool.max-idle = 8
# 连接池中的最小空闲连接，默认值也是0
spring.redis.pool.min-idle = 0
# 如果赋值为-1，则表示不限制；如果pool已经分配了maxActive个jedis实例，则此时pool的状态为exhausted(耗尽)。
spring.redis.pool.max-active = 8 
# 等待可用连接的最大时间，单位毫秒，默认值为-1，表示永不超时。如果超过等待时间，则直接抛出
spring.redis.pool.max-wait = -1
       java代码示例

package com.ldy.redisdemo;

import java.util.concurrent.TimeUnit;

import org.apache.log4j.Logger;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.data.redis.core.ValueOperations;
import org.springframework.stereotype.Service;

@Service
public class RedisService {

	private static Logger log = Logger.getLogger(RedisService.class);
	 
	@Autowired
	private StringRedisTemplate redisTemplate;
	 
	/**
	 * 写入redis缓存（不设置expire存活时间）
	 * @param key
	 * @param value
	 * @return
	 */
	public boolean set(final String key, String value) {
		boolean result = false;
		try {
			ValueOperations<String, String> operations = redisTemplate.opsForValue();
			operations.set(key, value);
			result = true;
		} catch (Exception e) {
			log.error("写入redis缓存失败！错误信息为：" + e.getMessage());
		}
		return result;
	}
	 
	/**
	 * 写入redis缓存（设置expire存活时间）
	 * @param key
	 * @param value
	 * @param expire
	 * @return
	 */
	public boolean set(final String key, String value, Long expire) {
		boolean result = false;
		try {
			ValueOperations<String, String> operations = redisTemplate.opsForValue();
			operations.set(key, value);
			redisTemplate.expire(key, expire, TimeUnit.SECONDS);
			result = true;
		} catch (Exception e) {
			log.error("写入redis缓存（设置expire存活时间）失败！错误信息为：" + e.getMessage());
		}
		return result;
	}
	 
	/**
	 * 读取redis缓存
	 * 
	 * @param key
	 * @return
	 */
	public Object get(final String key) {
		Object result = null;
		try {
			ValueOperations<String, String> operations = redisTemplate.opsForValue();
			result = operations.get(key);
		} catch (Exception e) {
			log.error("读取redis缓存失败！错误信息为：" + e.getMessage());
		}
		return result;
	}
	 
	/**
	 * 判断redis缓存中是否有对应的key
	 * @param key
	 * @return
	 */
	public boolean exists(final String key) {
		boolean result = false;
		try {
			result = redisTemplate.hasKey(key);
		} catch (Exception e) {
			log.error("判断redis缓存中是否有对应的key失败！错误信息为：" + e.getMessage());
		}
		return result;
	}
	 
	/**
	 * redis根据key删除对应的value
	 * @param key
	 * @return
	 */
	public boolean remove(final String key) {
		boolean result = false;
		try {
			if (exists(key)) {
				redisTemplate.delete(key);
			}
			result = true;
		} catch (Exception e) {
			log.error("redis根据key删除对应的value失败！错误信息为：" + e.getMessage());
		}
		return result;
	}
	 
	/**
	 * redis根据keys批量删除对应的value
	 * 
	 * @param keys
	 * @return
	 */
	public void remove(final String... keys) {
		for (String key : keys) {
			remove(key);
		}
	}

}
      经过测试，两种解决方案都能够正常通过密码验证方式连接到redis集群并进行相应操作！！！
--------------------- 
版权声明：本文为CSDN博主「伊宇紫」的原创文章，遵循CC 4.0 by-sa版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/LDY1016/article/details/80998687





# [redis集群报Jedis does not support password protected Redis Cluster configurations异常解决办法](https://www.cnblogs.com/linjiqin/p/7463012.html)

解决spring-data-redis操作redis集群报“Jedis does not support password protected Redis Cluster configurations”的异常

原因：使用spring-data-redis操作redis集群时由于redis集群设置了密码。

解决方案：升级spring-data-redis版本即可解决，最后相关jar包版本是：
jedis-2.9.0.jar
spring-data-redis-1.8.0.M1.jar
spring-session-1.2.2.RELEASE.jar
spring-data-commons-1.12.5.RELEASE.jar
commons-pool2-2.4.2.jar