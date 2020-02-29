# @RequestMapping使用须知

https://blog.csdn.net/zalan01408980/article/details/82904126



（1）映射单个URL

```
@RequestMapping("") 或 @RequestMapping(value="")
```

2）映射多个URL

```
@RequestMapping({"",""}) 或 @RequestMapping(value={"",""})
```

##### 路径开头是否加斜杠/均可，建议加上，如：@RequestMapping("/hello")



### @RequestMapping 一共有五种映射方式：



1、标准URL 映射

```
@RequestMapping("/hello")
或
@RequestMapping({"/hello","/world"})
```

2、Ant 风格的 URL 映射

| 通配符 | 说明                          |
| ------ | ----------------------------- |
| ?      | 匹配任何单字符                |
| *      | 匹配任意数量的字符（含 0 个） |
| **     | 匹配任意数量的目录（含 0 个） |

```
例如：
（1）@RequestMapping("/?/hello/")
（2）@RequestMapping("/*/hello")
（3）@RequestMapping("/**/hello")
```

3、占位符URL 映射

```
URL 中可以通过一个或多个 {} 占位符映射
例如：@RequestMapping("/user/{userId}/show")
可以通过@PathVariable("") 注解将占位符中的值绑定到方法参数上。

/**
* 如果 URL 中的 userId 是纯数字，那么使用 @PathVariable
* 做绑定时，可以根据自己的需求将方法参数类型设置为 Long、
* Integer、String
*/

@RequestMapping("/user/{userId}/show")

public ModelAndView show(@PathVariable("userId") Long userId) {
	// 创建 ModelAndView 对象，并设置视图名称
	ModelAndView mv = new ModelAndView("show");
	// 添加模型数据
	mv.addObject("msg", "User ID：" + userId);

return mv;

}
```

![img](https://img-blog.csdn.net/20180228234248012)



4、限制请求方法的URL 映射

```
在HTTP 请求中最常用的请求方法是 GET、POST，还有其他的

一些方法，如：DELET、PUT、HEAD 等
限制请求方法，例如：
@RequestMapping(value="/hello", method=RequestMethod.POST)
如需限制多个请求方法，以大括号包围，逗号隔开即可，例如：
method={RequestMethod.GET,RequestMethod.POST}
```

5、限制请求参数的URL 映射

```
限制请求参数来映射URL，例如：
@RequestMapping(value="/user/show", params="userId")
即请求中必须带有userId 参数
参数的限制规则如下：

（1）params="userId" 请求参数中必须包含 userId

（2）params="!userId" 请求参数中不能包含 userId

（3）params="userId!=1" 请求参数中必须包含 userId，但不能为 1

（4）params={"userId","userName"} 必须包含 userId 和 userName 参数

可以通过@RequestParam("") 注解将请求参数绑定到方法参数上
```

```
@RequestMapping(value="/user/show",params="userId")
public ModelAndView show(@RequestParam("userId") Long userId) {
        // 创建 ModelAndView 对象，并设置视图名称
        ModelAndView mv = new ModelAndView("show");
        // 添加模型数据
        mv.addObject("msg", "User ID：" + userId);
        return mv;
}
```

### Spring mvc中@RequestMapping 6个基本用法小结

```
小结下spring mvc中的@RequestMapping的用法。

1）最基本的，方法级别上应用，例如：
   
Java代码  收藏代码
@RequestMapping(value="/departments")  
public String simplePattern(){  
  
  System.out.println("simplePattern method was called");  
  return "someResult";  
  
}  

   则访问http://localhost/xxxx/departments的时候，会调用 simplePattern方法了

2） 参数绑定
  
Java代码  收藏代码
@RequestMapping(value="/departments")  
public String findDepatment(  
  @RequestParam("departmentId") String departmentId){  
    
    System.out.println("Find department with ID: " + departmentId);  
    return "someResult";  
  
}  

  
  形如这样的访问形式：

   /departments?departmentId=23就可以触发访问findDepatment方法了

3 REST风格的参数
  
Java代码  收藏代码
@RequestMapping(value="/departments/{departmentId}")  
public String findDepatment(@PathVariable String departmentId){  
  
  System.out.println("Find department with ID: " + departmentId);  
  return "someResult";  
  
}  

 
  形如REST风格的地址访问，比如：
/departments/23，其中用(@PathVariable接收rest风格的参数

4 REST风格的参数绑定形式之2
   先看例子，这个有点象之前的：
Java代码  收藏代码
@RequestMapping(value="/departments/{departmentId}")  
public String findDepatmentAlternative(  
  @PathVariable("departmentId") String someDepartmentId){  
  
    System.out.println("Find department with ID: " + someDepartmentId);  
    return "someResult";  
  
}  


   这个有点不同，就是接收形如/departments/23的URL访问，把23作为传入的departmetnId,，但是在实际的方法findDepatmentAlternative中，使用
@PathVariable("departmentId") String someDepartmentId，将其绑定为
someDepartmentId,所以这里someDepartmentId为23

5 url中同时绑定多个id
 
Java代码  收藏代码
@RequestMapping(value="/departments/{departmentId}/employees/{employeeId}")  
public String findEmployee(  
  @PathVariable String departmentId,  
  @PathVariable String employeeId){  
  
    System.out.println("Find employee with ID: " + employeeId +   
      " from department: " + departmentId);  
    return "someResult";  
  
}  


   这个其实也比较好理解了。

6 支持正则表达式
  
Java代码  收藏代码
@RequestMapping(value="/{textualPart:[a-z-]+}.{numericPart:[\\d]+}")  
public String regularExpression(  
  @PathVariable String textualPart,  
  @PathVariable String numericPart){  
  
    System.out.println("Textual part: " + textualPart +   
      ", numeric part: " + numericPart);  
    return "someResult";  
}  


   比如如下的URL：/sometext.123，则输出：
Textual part: sometext, numeric part: 123.
```

