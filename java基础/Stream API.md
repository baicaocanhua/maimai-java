# Java 8都出那么久了，Stream API了解下？



https://juejin.im/post/5d6d2016e51d453c135c5b25?utm_source=gold_browser_extension



### 用collect方法将List转成map

```
// 将权限列表以id为key，以权限对象为值转换成map
Map<Long, UmsPermission> permissionMap = permissionList.stream()
    .collect(Collectors.toMap(permission -> permission.getId(), permission -> permission));

```

# 简洁又快速地处理集合——Java8 Stream（下）

 https://cloud.tencent.com/developer/article/1187833 

### **sorted**

```java
根据年龄大小来比较：
list = list.stream()
           .sorted((p1, p2) -> p1.getAge() - p2.getAge())
           .collect(toList());
           
list = list.stream()
           .sorted(Comparator.comparingInt(Person::getAge))
           .collect(toList());
```

# java8 stream流操作的flatMap（流的扁平化）

 https://blog.csdn.net/Mark_Chao/article/details/80810030 



```
String[] words = new String[]{"Hello","World"};
        List<String> a = Arrays.stream(words)
                .map(word -> word.split(""))
                .flatMap(Arrays::stream)
                .distinct()
                .collect(toList());
        a.forEach(System.out::print);
```



# [最全最强 Java 8 - 函数编程（lambda表达式）](https://www.cnblogs.com/pengdai/p/11697113.html)

 https://www.cnblogs.com/pengdai/p/11697113.html#_label1 