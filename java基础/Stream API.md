# Java 8都出那么久了，Stream API了解下？



https://juejin.im/post/5d6d2016e51d453c135c5b25?utm_source=gold_browser_extension



### 用collect方法将List转成map

```
// 将权限列表以id为key，以权限对象为值转换成map
Map<Long, UmsPermission> permissionMap = permissionList.stream()
    .collect(Collectors.toMap(permission -> permission.getId(), permission -> permission));

```

