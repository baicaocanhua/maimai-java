# [Spring 指南（spring-retry）](https://segmentfault.com/a/1190000019932970)

```
spring retry 重试机制完整例子
```

```
public  static Boolean vpmsRetryCoupon(final String userId) {
        // 构建重试模板实例
        RetryTemplate retryTemplate = new RetryTemplate();
        // 设置重试策略，主要设置重试次数
        SimpleRetryPolicy policy = new SimpleRetryPolicy(10, Collections.<Class<? extends Throwable>, Boolean> singletonMap(Exception.class, true));
        // 设置重试回退操作策略，主要设置重试间隔时间
        FixedBackOffPolicy fixedBackOffPolicy = new FixedBackOffPolicy();
        fixedBackOffPolicy.setBackOffPeriod(100);
        retryTemplate.setRetryPolicy(policy);
        retryTemplate.setBackOffPolicy(fixedBackOffPolicy);
        // 通过RetryCallback 重试回调实例包装正常逻辑逻辑，第一次执行和重试执行执行的都是这段逻辑
        final RetryCallback<Object, Exception> retryCallback = new RetryCallback<Object, Exception>() {
            //RetryContext 重试操作上下文约定，统一spring-try包装
            public Object doWithRetry(RetryContext context) throws Exception {
               boolean result = pushCouponByVpmsaa(userId);
               if(!result){
                   throw new RuntimeException();//这个点特别注意，重试的根源通过Exception返回
               }
               return true;
            }
        };
        // 通过RecoveryCallback 重试流程正常结束或者达到重试上限后的退出恢复操作实例
        final RecoveryCallback<Object> recoveryCallback = new RecoveryCallback<Object>() {
            public Object recover(RetryContext context) throws Exception {
//                logger.info("正在重试发券::::::::::::"+userId);
                return null;
            }
        };
        try {
            // 由retryTemplate 执行execute方法开始逻辑执行
            retryTemplate.execute(retryCallback, recoveryCallback);
        } catch (Exception e) {
//            logger.info("发券错误异常========"+e.getMessage());
            e.printStackTrace();
        }
        return true;
    }

    public static void main(String[] args) {
        vpmsRetryCoupon("43333");
    }


    public static Boolean pushCouponByVpmsaa(String userId){
        Random random = new Random();
        int a= random.nextInt(10);
        System.out.println("a是"+a);
        if(a==8){
            return  true;
        }else{
            return false;
        }
```

```
spring-retry（1.概念和基本用法）
https://www.jianshu.com/p/58e753ca0151
```

```
 private static final Logger LOGGER= LoggerFactory.getLogger(RetryTest2.class);
    public static void main(String[] args) throws IOException {
        RetryTemplate template = new RetryTemplate(); //使用超时重试策略

        TimeoutRetryPolicy policy = new TimeoutRetryPolicy(); //设置超时时间
        policy.setTimeout(200L);
        template.setRetryPolicy(policy); //设置重试执行代码


        RecoveryCallback<String> recoveryCallback = new RecoveryCallback<String>() {
            @Override
            public String recover(RetryContext retryContext) throws Exception {
                LOGGER.info("recover");
                return "recover";
            }
        };

                String result = template.execute(new RetryCallback<String, IOException>() {
            @Override
            public String doWithRetry(RetryContext context) {
                int count=context.getRetryCount();

                LOGGER.info("Execute callback .{}",count);
                //if(count!=5){
                int i = 1/0;
               // }

                return "Successfully";
            }
        },recoveryCallback);
    }
```

