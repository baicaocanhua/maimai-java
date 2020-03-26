```


        ObjectMapper mapper = new ObjectMapper();
        Dog dog = new Dog();
        //实体转化为json字符串
        String json_dog = mapper.writeValueAsString(dog);
        //json字符串 转化为实体
        Dog user = mapper.readValue("json_dog", Dog.class);

        JsonNode jsonNode=mapper.readTree(json_dog);
        //JsonNode 转化为实体
        Dog suer = mapper.treeToValue(jsonNode, Dog.class);


```

JsonObject对象转换成JavaBean

Student student = object.toJavaObject(Student.class);



# 阿里巴巴的JSONObject对象转换

/Javabean对象转换成String类型的JSON字符串
JSONObject.toJSONString(Javabean对象)

//String类型的JSON字符串转换成Javabean对象
JSONObject.toJavaObject(JSON字符串,Javabean.class)

//Json字符串转换成JSONObject对象
JSONObject.parseObject(JSON字符串)

//JSON字符串转换成Javabean对象
JSONObject.parseObject(JSON字符串,Javabean.class)

例如
Refund r = new Refund();
String jsonStr = JSONObject.toJSONString(r);


String jsonStr = "{\"msg\":\"ZhangSan\"}";
Refund r = JSONObject.toJavaObject(jsonStr,Refund.class);


JSONObject jsonObject = JSONObject.parseObject(jsonStr);


Refund r = JSONObject.parseObject(jsonStr,Refund.class);
--------------------- 
作者：唯爱沁源 
来源：CSDN 
原文：https://blog.csdn.net/a990914093/article/details/81217581 
版权声明：本文为博主原创文章，转载请附上博文链接！





https://blog.csdn.net/qq_40925004/article/details/86556794

# 阿里的FastJSON使用





# 阿里fastjson框架基础

https://blog.csdn.net/zixiao217/article/details/82850686





# JsonNode、JsonObject常用方法

https://blog.csdn.net/mst1010/article/details/78589059





# 将JSON对象转化为实体对象

https://blog.csdn.net/xuwei198995/article/details/52411102