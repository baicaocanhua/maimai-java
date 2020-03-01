[java两个对象属性比较](https://www.cnblogs.com/heavenTang/p/7608253.html)

两个对象进行比较相等，有两种做法：
1,情况一：当仅仅只是判断两个对象是否相等时，只需重写equals()方法即可。这里就不用说明
2.情况二：当除了情况一之外，还需知道是那个属性不同，那么就需要采用类反射，具体代码如下：



```
    public static void main(String[] args) {
    A a = new A();
	a.setUserName("a");
	a.setPassword("p");
	a.setQq("q");
	a.setWechat("w");
    A b = new A();
    b.setUserName("a");
    b.setPassword("p");
    b.setQq("q");
    b.setWechat("ww");

    //只是比较两个对象是否相等，那么直接重写equals方法
    System.out.println( a.equals(b));

    try {
        Map<String, String> maps = compare( a, b );
        System.out.println();
    } catch (Exception e) {
        e.printStackTrace();
    }
}


public static <T> Map<String, String> compare(T obj1, T Obj2)
        throws Exception {

    Map<String, String> result = new HashMap<String, String>();

    Field[] fs = obj1.getClass().getDeclaredFields();
    for (Field f : fs) {
        f.setAccessible(true);
        Object v1 = f.get(obj1);
        Object v2 = f.get(Obj2);
        if( ! equals(v1, v2) ){
            result.put(f.getName(), String.valueOf(equals(v1, v2)));

        }
    }
    return result;
}

public static boolean equals(Object obj1, Object obj2) {

    if (obj1 == obj2) {
        return true;
    }
    if (obj1 == null || obj2 == null) {
        return false;
    }
    return obj1.equals(obj2);
}
```