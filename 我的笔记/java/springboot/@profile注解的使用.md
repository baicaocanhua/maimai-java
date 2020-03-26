



# spring @profile注解的使用

https://blog.csdn.net/u012129558/article/details/78258951

声明：

```java
package com.xueyou.demo;  
  
public interface MoveFactor {  
    void speak();  
} 
```

使用：

```java
package com.xueyou.demo;  
  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.stereotype.Component;  
  
@Component  
public class Person {  
  
    @Autowired  
    private MoveFactor moveFactor;  
  
    public void speak(){  
        moveFactor.speak();  
    }  
}  
```





实现：

```java
package com.xueyou.demo;  
  
import org.springframework.context.annotation.Configuration;  
import org.springframework.context.annotation.Profile;  
import org.springframework.stereotype.Component;  
  
  
@Configuration  
@Profile(value = "dev")  
@Component  
public class Chinese implements MoveFactor {  
    @Override  
    public void speak() {  
        System.out.println("我是中国人");  
    }  
}  
```



```java
package com.xueyou.demo;  
  
import org.springframework.context.annotation.Profile;  
import org.springframework.stereotype.Component;  
  
@Component  
@Profile("qa")  
public class English implements MoveFactor{  
    @Override  
    public void speak() {  
        System.out.println("i am an English");  
    }  
}  
```

```java
package com.xueyou.demo;  
  
import org.springframework.context.annotation.Profile;  
import org.springframework.stereotype.Component;  
  
  
@Component  
@Profile("prod")  
public class German implements MoveFactor{  
    @Override  
    public void speak() {  
        System.out.println("i am a German");  
    }  
} 
```

```java
package com.xueyou.demo;  
  
import org.junit.Test;  
import org.junit.runner.RunWith;  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.test.context.ActiveProfiles;  
import org.springframework.test.context.ContextConfiguration;  
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;  
  
  
@RunWith(SpringJUnit4ClassRunner.class)  
@ContextConfiguration(classes = App.class)  
@ActiveProfiles("dev")  
public class SpringTest {  
  
    @Autowired  
    Person p;  
  
    @Test  
    public void testProfile(){  
        p.speak();  
    }  
  
}
```

