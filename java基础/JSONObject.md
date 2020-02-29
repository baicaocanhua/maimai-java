# [com.alibaba.fastjson把JSONObject转换为Map对象](https://www.cnblogs.com/fomeiherz/p/6351287.html)

JSONObject obj = new JSONObject();
{
obj.put("key1", "value1");
obj.put("key2", "value2");
obj.put("key3", "value3");
}
Map<String, String> params = JSONObject.parseObject(obj.toJSONString(), new TypeReference<Map<String, String>>(){});
System.out.println(params);

//输出：{key3=value3, key2=value2, key1=value1}



# [FastJson中JSONObject用法及常用方法总结](https://www.cnblogs.com/zjdxr-up/p/9736755.html)

https://www.cnblogs.com/zjdxr-up/p/9736755.html





# FastJson中JSONObject用法及常用方法总结

https://blog.csdn.net/qq_22078107/article/details/85705465



# [com.alibaba.fastjson.JSONObject之对象与JSON转换方法](https://www.cnblogs.com/ibigboy/p/11124524.html)