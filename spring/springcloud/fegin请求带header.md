 https://blog.csdn.net/my_momo_csdn/article/details/80922737 

# [SpringCloud-Feign] Feign转发请求头，(防止session失效)

# Spring Cloud系列（三十二）Feign丢失Cookie和Header信息的问题（Finchley.RC2版本）

 https://blog.csdn.net/WYA1993/article/details/84304243 





 https://blog.csdn.net/crystalqy/article/details/79083857 

# feign调用session丢失解决方案



### 自定义策略

```
import com.netflix.hystrix.HystrixThreadPoolKey;
import com.netflix.hystrix.HystrixThreadPoolProperties;
import com.netflix.hystrix.strategy.HystrixPlugins;
import com.netflix.hystrix.strategy.concurrency.HystrixConcurrencyStrategy;
import com.netflix.hystrix.strategy.concurrency.HystrixRequestVariable;
import com.netflix.hystrix.strategy.concurrency.HystrixRequestVariableLifecycle;
import com.netflix.hystrix.strategy.eventnotifier.HystrixEventNotifier;
import com.netflix.hystrix.strategy.executionhook.HystrixCommandExecutionHook;
import com.netflix.hystrix.strategy.metrics.HystrixMetricsPublisher;
import com.netflix.hystrix.strategy.properties.HystrixPropertiesStrategy;
import com.netflix.hystrix.strategy.properties.HystrixProperty;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Component;
import org.springframework.web.context.request.RequestAttributes;
import org.springframework.web.context.request.RequestContextHolder;

import java.util.concurrent.BlockingQueue;
import java.util.concurrent.Callable;
import java.util.concurrent.ThreadPoolExecutor;
import java.util.concurrent.TimeUnit;

/**
 * 自定义Feign的隔离策略;
 * 在转发Feign的请求头的时候，如果开启了Hystrix，Hystrix的默认隔离策略是Thread(线程隔离策略)，因此转发拦截器内是无法获取到请求的请求头信息的，可以修改默认隔离策略为信号量模式：hystrix.command.default.execution.isolation.strategy=SEMAPHORE，这样的话转发线程和请求线程实际上是一个线程，这并不是最好的解决方法，信号量模式也不是官方最为推荐的隔离策略；另一个解决方法就是自定义Hystrix的隔离策略，思路是将现有的并发策略作为新并发策略的成员变量,在新并发策略中，返回现有并发策略的线程池、Queue；将策略加到Spring容器即可；
 *
 * @author mozping
 * @version 1.0
 * @date 2018/7/5 9:08
 * @see FeignHystrixConcurrencyStrategyIntellif
 * @since JDK1.8
 */
@Component
public class FeignHystrixConcurrencyStrategyIntellif extends HystrixConcurrencyStrategy {

    private static final Logger log = LoggerFactory.getLogger(FeignHystrixConcurrencyStrategyIntellif.class);
    private HystrixConcurrencyStrategy delegate;

    public FeignHystrixConcurrencyStrategyIntellif() {
        try {
            this.delegate = HystrixPlugins.getInstance().getConcurrencyStrategy();
            if (this.delegate instanceof FeignHystrixConcurrencyStrategyIntellif) {
                // Welcome to singleton hell...
                return;
            }
            HystrixCommandExecutionHook commandExecutionHook =
                    HystrixPlugins.getInstance().getCommandExecutionHook();
            HystrixEventNotifier eventNotifier = HystrixPlugins.getInstance().getEventNotifier();
            HystrixMetricsPublisher metricsPublisher = HystrixPlugins.getInstance().getMetricsPublisher();
            HystrixPropertiesStrategy propertiesStrategy =
                    HystrixPlugins.getInstance().getPropertiesStrategy();
            this.logCurrentStateOfHystrixPlugins(eventNotifier, metricsPublisher, propertiesStrategy);
            HystrixPlugins.reset();
            HystrixPlugins.getInstance().registerConcurrencyStrategy(this);
            HystrixPlugins.getInstance().registerCommandExecutionHook(commandExecutionHook);
            HystrixPlugins.getInstance().registerEventNotifier(eventNotifier);
            HystrixPlugins.getInstance().registerMetricsPublisher(metricsPublisher);
            HystrixPlugins.getInstance().registerPropertiesStrategy(propertiesStrategy);
        } catch (Exception e) {
            log.error("Failed to register Sleuth Hystrix Concurrency Strategy", e);
        }
    }

    private void logCurrentStateOfHystrixPlugins(HystrixEventNotifier eventNotifier,
                                                 HystrixMetricsPublisher metricsPublisher, HystrixPropertiesStrategy propertiesStrategy) {
        if (log.isDebugEnabled()) {
            log.debug("Current Hystrix plugins configuration is [" + "concurrencyStrategy ["
                    + this.delegate + "]," + "eventNotifier [" + eventNotifier + "]," + "metricPublisher ["
                    + metricsPublisher + "]," + "propertiesStrategy [" + propertiesStrategy + "]," + "]");
            log.debug("Registering Sleuth Hystrix Concurrency Strategy.");
        }
    }

    @Override
    public <T> Callable<T> wrapCallable(Callable<T> callable) {
        RequestAttributes requestAttributes = RequestContextHolder.getRequestAttributes();
        return new WrappedCallable<>(callable, requestAttributes);
    }

    @Override
    public ThreadPoolExecutor getThreadPool(HystrixThreadPoolKey threadPoolKey,
                                            HystrixProperty<Integer> corePoolSize, HystrixProperty<Integer> maximumPoolSize,
                                            HystrixProperty<Integer> keepAliveTime, TimeUnit unit, BlockingQueue<Runnable> workQueue) {
        return this.delegate.getThreadPool(threadPoolKey, corePoolSize, maximumPoolSize, keepAliveTime,
                unit, workQueue);
    }

    @Override
    public ThreadPoolExecutor getThreadPool(HystrixThreadPoolKey threadPoolKey,
                                            HystrixThreadPoolProperties threadPoolProperties) {
        return this.delegate.getThreadPool(threadPoolKey, threadPoolProperties);
    }

    @Override
    public BlockingQueue<Runnable> getBlockingQueue(int maxQueueSize) {
        return this.delegate.getBlockingQueue(maxQueueSize);
    }

    @Override
    public <T> HystrixRequestVariable<T> getRequestVariable(HystrixRequestVariableLifecycle<T> rv) {
        return this.delegate.getRequestVariable(rv);
    }

    static class WrappedCallable<T> implements Callable<T> {
        private final Callable<T> target;
        private final RequestAttributes requestAttributes;

        public WrappedCallable(Callable<T> target, RequestAttributes requestAttributes) {
            this.target = target;
            this.requestAttributes = requestAttributes;
        }

        @Override
        public T call() throws Exception {
            try {
                RequestContextHolder.setRequestAttributes(requestAttributes);
                return target.call();
            } finally {
                RequestContextHolder.resetRequestAttributes();
            }
        }
    }
}
————————————————
版权声明：本文为CSDN博主「学圆惑边」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/my_momo_csdn/article/details/80922737
```



 服务通过请求拦截器，在请求从A发送到B之后，在拦截器内将自己需要的东东加到请求头 



```
import com.intellif.log.LoggerUtilI;
import feign.RequestInterceptor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.context.request.RequestContextHolder;
import org.springframework.web.context.request.ServletRequestAttributes;

import javax.servlet.http.HttpServletRequest;
import java.util.Enumeration;

/**
 * 自定义的请求头处理类，处理服务发送时的请求头；
 * 将服务接收到的请求头中的uniqueId和token字段取出来，并设置到新的请求头里面去转发给下游服务
 * 比如A服务收到一个请求，请求头里面包含uniqueId和token字段，A处理时会使用Feign客户端调用B服务
 * 那么uniqueId和token这两个字段就会添加到请求头中一并发给B服务；
 *
 * @author mozping
 * @version 1.0
 * @date 2018/6/27 14:13
 * @see FeignHeadConfiguration
 * @since JDK1.8
 */
@Configuration
public class FeignHeadConfiguration {
    private final LoggerUtilI logger = LoggerUtilI.getLogger(this.getClass().getName());

    @Bean
    public RequestInterceptor requestInterceptor() {
        return requestTemplate -> {
            ServletRequestAttributes attrs = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
            if (attrs != null) {
                HttpServletRequest request = attrs.getRequest();
                Enumeration<String> headerNames = request.getHeaderNames();
                if (headerNames != null) {
                    while (headerNames.hasMoreElements()) {
                        String name = headerNames.nextElement();
                        String value = request.getHeader(name);
                        /**
                         * 遍历请求头里面的属性字段，将logId和token添加到新的请求头中转发到下游服务
                         * */
                        if ("uniqueId".equalsIgnoreCase(name) || "token".equalsIgnoreCase(name)) {
                            logger.debug("添加自定义请求头key:" + name + ",value:" + value);
                            requestTemplate.header(name, value);
                        } else {
                            logger.debug("FeignHeadConfiguration", "非自定义请求头key:" + name + ",value:" + value + "不需要添加!");
                        }
                    }
                } else {
                    logger.warn("FeignHeadConfiguration", "获取请求头失败！");
                }
            }
        };
    }

}
————————————————
版权声明：本文为CSDN博主「学圆惑边」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/my_momo_csdn/article/details/80922737
```

或者

```
@Configuration  
@EnableFeignClients(basePackages = "com.xxx.xxx.client")  
public class FeignClientsConfigurationCustom implements RequestInterceptor {  

  @Override  
  public void apply(RequestTemplate template) {  

    RequestAttributes requestAttributes = RequestContextHolder.getRequestAttributes();  
    if (requestAttributes == null) {  
      return;  
    }  

    HttpServletRequest request = ((ServletRequestAttributes) requestAttributes).getRequest();  
    Enumeration<String> headerNames = request.getHeaderNames();  
    if (headerNames != null) {  
      while (headerNames.hasMoreElements()) {  
        String name = headerNames.nextElement();  
        Enumeration<String> values = request.getHeaders(name);  
        while (values.hasMoreElements()) {  
          String value = values.nextElement();  
          template.header(name, value);  
        }  
      }  
    }  

  }  

} 
————————————————
版权声明：本文为CSDN博主「总有刁明想害朕」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/crystalqy/article/details/79083857
```

