```
if(list!=null && !list.isEmpty()){
　　　//不为空的情况
}else{
　　　//为空的情况
}
```

List<String> Names= new ArrayList<>();

当从数据库中查出的数据为NULL时，可以用CollectionUtils.isNotEmpty()来判断Names是否有值，值是否可用。

CollectionUtils.isNotEmpty() 包含null,size=0等多种情况，太好用了。
————————————————
版权声明：本文为CSDN博主「大海绵」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/zl_1987/article/details/51849486

