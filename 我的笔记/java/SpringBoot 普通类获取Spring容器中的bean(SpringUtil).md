# SpringBoot 普通类获取Spring容器中的bean(SpringUtil)



https://blog.csdn.net/tuoni123/article/details/80213160





# **普通类调用Spring Bean对象**

https://412887952-qq-com.iteye.com/blog/1479445





# Spring中的各种Utils（一）:PropertiesLoaderUtils

https://www.jianshu.com/p/52f8b7de1d76

```java
import org.springframework.beans.BeansException;
import org.springframework.beans.factory.NoSuchBeanDefinitionException;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ApplicationContextAware;
 
public class SpringContextUtil implements ApplicationContextAware {  
   private static ApplicationContext applicationContext;     //Spring应用上下文环境  
     
   public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {  
       SpringContextUtil.applicationContext = applicationContext;  
   }  

   public static ApplicationContext getApplicationContext() {  
     return applicationContext;  
   }  
    
   public static Object getBean(String name) throws BeansException {  
     return applicationContext.getBean(name);  
   } 
   
   public static <T> T getBean(Class<T> requiredType) {
       return applicationContext.getBean(requiredType);
   }
    
public static <T> T getBean(String name, Class<T> requiredType) throws BeansException {  
     return applicationContext.getBean(name, requiredType);  
   }  
    
   public static boolean containsBean(String name) {  
     return applicationContext.containsBean(name);  
   }  
    
   public static boolean isSingleton(String name) throws NoSuchBeanDefinitionException {  
     return applicationContext.isSingleton(name);  
   }  

   @SuppressWarnings("rawtypes")
public static Class getType(String name) throws NoSuchBeanDefinitionException {  
     return applicationContext.getType(name);  
   }  
    
   public static String[] getAliases(String name) throws NoSuchBeanDefinitionException {  
     return applicationContext.getAliases(name);  
   }  
 }  

```



[SpringUtil](https://www.cnblogs.com/zhao123/p/3876824.html)

```
/** SpringUtil.java

{{IS_NOTE
    Purpose:
        
    Description:
        
    History:
        Thu Jun  1 13:53:53     2006, Created by henrichen
}}IS_NOTE

Copyright (C) 2006 Potix Corporation. All Rights Reserved.

{{IS_RIGHT
}}IS_RIGHT
*/
package org.zkoss.zkplus.spring;

import javax.servlet.ServletContext;

import org.springframework.beans.factory.BeanNotOfRequiredTypeException;
import org.springframework.beans.factory.NoSuchBeanDefinitionException;
import org.springframework.context.ApplicationContext;
import org.springframework.web.context.support.WebApplicationContextUtils;
import org.zkoss.zk.ui.Execution;
import org.zkoss.zk.ui.Executions;
import org.zkoss.zk.ui.UiException;

/***
 * SpringUtil, a Spring utility.
 * <p>Applicable to Spring Framework version 2.x or later</p>
 * @author henrichen
 */
public class SpringUtil {
    /***
     * Get the spring application context.
     */
    public static ApplicationContext getApplicationContext() {
        Execution exec = Executions.getCurrent();
        if (exec == null) {
            throw new UiException("SpringUtil can be called only under ZK environment!");
        }
        
        return WebApplicationContextUtils.getRequiredWebApplicationContext(
                (ServletContext)exec.getDesktop().getWebApp().getNativeContext());
    }
    
    /***
     * Get the spring bean by the specified name.
     */        
    public static Object getBean(String name) {
        Object o = null;
        try {
            if(getApplicationContext().containsBean(name)) {
                o = getApplicationContext().getBean(name);
            }
        } catch (NoSuchBeanDefinitionException ex) {
            // ignore
        }
        return o;
    }

    /***
     * Get the spring bean by the specified name and class.
     */        
    public static Object getBean(String name, Class cls) {
        Object o = null;
        try {
            if(getApplicationContext().containsBean(name)) {
                o = getApplicationContext().getBean(name, cls);
            }
        } catch (NoSuchBeanDefinitionException ex) {
            // ignore
        } catch (BeanNotOfRequiredTypeException e) {
            // ignore
        }
        return o;
    }
}
```

