Redis分布式锁解决抢购问题
https://blog.csdn.net/thc1987/article/details/80355155
【转】基于Redis Lua脚本实现的分布式锁（Java实现）
Redis+Lua脚本实现的分布式锁的正确操作
https://www.jianshu.com/p/4b29e4d6b408

实现redis分布式锁
https://blog.csdn.net/m0_37367413/article/details/84347424


redis集群下分布式锁的实现
https://blog.csdn.net/qq_36305027/article/details/80733778


Springboot分布式锁实践（redis）
https://www.cnblogs.com/carrychan/p/9431137.html

@Slf4j
@Component
public class RedisService {
    @Autowired
    private StringRedisTemplate stringRedisTemplate;


    /***
     * 加锁
     * @param key
     * @param value 当前时间+超时时间
     * @return 锁住返回true
     */
    public boolean lock(String key,String value){
        if(stringRedisTemplate.opsForValue().setIfAbsent(key,value)){//setNX 返回boolean
            return true;
        }
        //如果锁超时 ***
        String currentValue = stringRedisTemplate.opsForValue().get(key);
        if(!StringUtils.isEmpty(currentValue) && Long.parseLong(currentValue)<System.currentTimeMillis()){
            //获取上一个锁的时间
            String oldvalue  = stringRedisTemplate.opsForValue().getAndSet(key,value);
            if(!StringUtils.isEmpty(oldvalue)&&oldvalue.equals(currentValue)){
                return true;
            }
        }
        return false;
    }
    /***
     * 解锁
     * @param key
     * @param value
     * @return
     */
    public void unlock(String key,String value){
        try {
            String currentValue = stringRedisTemplate.opsForValue().get(key);
            if(!StringUtils.isEmpty(currentValue)&&currentValue.equals(value)){
                stringRedisTemplate.opsForValue().getOperations().delete(key);
            }
        } catch (Exception e) {
            log.error("解锁异常");
        }
    }
}



private static final int TIMEOUT= 10*1000;
@Transactional
public void orderProductMockDiffUser(String productId){
     long time = System.currentTimeMillions()+TIMEOUT;
   if(!redislock.lock(productId,String.valueOf(time)){
    throw new SellException(101,"换个姿势再试试")
    }
    //1.查库存
    int stockNum  = stock.get(productId);
    if(stocknum == 0){
        throw new SellException(ProductStatusEnum.STOCK_EMPTY);
        //这里抛出的异常要是运行时异常，否则无法进行数据回滚，这也是spring中比较基础的   
    }else{
        //2.下单
        orders.put(KeyUtil.genUniqueKey(),productId);//生成随机用户id模拟高并发
        sotckNum = stockNum-1;
        try{
            Thread.sleep(100);
        } catch (InterruptedExcption e){
            e.printStackTrace();
        }
        stock.put(productId,stockNum);
    }
    redisLock.unlock(productId,String.valueOf(time));
}