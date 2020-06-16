```
import java.util.HashMap;
import java.util.Iterator;
import java.util.Set;

public class TestHashMap {

    public static void main(String[] args) {
        HashMap<Integer, String>map=new HashMap<Integer, String>();
        map.put(99527, "安琪拉");
        map.put(99529, "老夫子");
        map.put(99528, "虞姬");
        map.put(99531, "兰陵王");
        map.put(99530, "嬴政");
        map.put(99532, "哪吒");
        map.put(99533, "芈月");
        //重复的键，新的值覆盖以前的值
        map.put(99533, "花木兰");
        System.out.println("map元素个数"+map.size());
        System.out.println(map);
        System.out.println(map.get(99999));//找不到的元素返回null
        System.out.println(map.remove(99530));//返回删除的元素
        System.out.println(map); 
        //取出所有的键
        Set<Integer> keys = map.keySet();
        for (Iterator<Integer> it = keys.iterator();it.hasNext();) {
            Integer k = it.next();
            String v = map.get(k);
            System.out.print(k+"="+v);
        }
        System.out.println();
        System.out.println("========================================================");
        for(Integer key:map.keySet())
        {
            System.out.print(key+"="+map.get(key));
        }
        System.out.println();
        System.out.println("========================================================");
        Iterator<Integer> it=keys.iterator();
        while(it.hasNext())
        {
            Integer k=it.next();
            String v=map.get(k);
            System.out.print(k+"="+v);
        }
        System.out.println();
    }

}

```

