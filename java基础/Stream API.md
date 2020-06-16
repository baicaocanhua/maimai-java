# Java 8都出那么久了，Stream API了解下？



https://juejin.im/post/5d6d2016e51d453c135c5b25?utm_source=gold_browser_extension



### 用collect方法将List转成map

```
// 将权限列表以id为key，以权限对象为值转换成map
Map<Long, UmsPermission> permissionMap = permissionList.stream()
    .collect(Collectors.toMap(permission -> permission.getId(), permission -> permission));

```



## JDK1.8 stream api的数组limit分页



```
public class LimitTest {
 
    public static void main(String[] args) {
        // 初始化
        List<Student> students = new ArrayList<Student>() {
            {
                add(new Student(20160001, "孔明", 20, 1, "土木工程", "武汉大学"));
                add(new Student(20160002, "伯约", 21, 2, "信息安全", "武汉大学"));
                add(new Student(20160003, "玄德", 22, 3, "经济管理", "武汉大学"));
                add(new Student(20160004, "云长", 21, 2, "信息安全", "武汉大学"));
                add(new Student(20161001, "翼德", 21, 2, "机械与自动化", "华中科技大学"));
                add(new Student(20161002, "元直", 23, 4, "土木工程", "华中科技大学"));
                add(new Student(20161003, "奉孝", 23, 4, "计算机科学", "华中科技大学"));
                add(new Student(20162001, "仲谋", 22, 3, "土木工程", "浙江大学"));
                add(new Student(20162002, "鲁肃", 23, 4, "计算机科学", "浙江大学"));
                add(new Student(20163001, "丁奉", 24, 5, "土木工程", "南京大学"));
            }
        };
        for (int limit = 2, skip = 0; skip < students.size(); skip = skip + limit) {
            System.out.println(students.stream().skip(skip).limit(limit).collect(Collectors.toList()));
        }
    }
}
 
class Student {
 
    /**
     * 学号
     */
    private long id;
 
    private String name;
 
    private int age;
 
    /**
     * 年级
     */
    private int grade;
 
    /**
     * 专业
     */
    private String major;
 
    /**
     * 学校
     */
    private String school;
 
    // 省略getter和setter
}
————————————————
版权声明：本文为CSDN博主「cjbi」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/u010697681/article/details/89487327
```

