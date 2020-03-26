# spring boot 下对JSON返回值去除null和空字段

在开发过程中，我们需要统一返回前端json格式的数据，但有些接口的返回值存在 null或者""这种没有意义的字段。

不仅影响理解，还浪费带宽，这时我们可以统一做一下处理，不返回空字段，或者把NULL转成“”，spring 内置的json处理框架是Jackson。我们可以对它配置一下达到目的







```
/**
 * 〈返回json空值去掉null和""〉 〈功能详细描述〉
 * 
 * @author gogym
 * @version 2017年10月13日
 * @see JacksonConfig
 * @since
 */
@Configuration
public class JacksonConfig
{
    @Bean
    @Primary
    @ConditionalOnMissingBean(ObjectMapper.class)
    public ObjectMapper jacksonObjectMapper(Jackson2ObjectMapperBuilder builder)
    {
        ObjectMapper objectMapper = builder.createXmlMapper(false).build();
 
        // 通过该方法对mapper对象进行设置，所有序列化的对象都将按改规则进行系列化
        // Include.Include.ALWAYS 默认
        // Include.NON_DEFAULT 属性为默认值不序列化
        // Include.NON_EMPTY 属性为 空（""） 或者为 NULL 都不序列化，则返回的json是没有这个字段的。这样对移动端会更省流量
        // Include.NON_NULL 属性为NULL 不序列化,就是为null的字段不参加序列化
        //objectMapper.setSerializationInclusion(Include.NON_EMPTY);
 
        // 字段保留，将null值转为""
        objectMapper.getSerializerProvider().setNullValueSerializer(new JsonSerializer<Object>()
    {
        @Override
        public void serialize(Object o, JsonGenerator jsonGenerator,
                              SerializerProvider serializerProvider)
                throws IOException, JsonProcessingException
        {
            jsonGenerator.writeString("");
        }
    });
        return objectMapper;
    }
}

--------------------- 
作者：Gogym 
来源：CSDN 
原文：https://blog.csdn.net/kokjuis/article/details/78830314 
版权声明：本文为博主原创文章，转载请附上博文链接！
```

