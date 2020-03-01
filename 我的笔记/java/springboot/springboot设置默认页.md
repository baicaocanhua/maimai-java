# springboot设置默认页面

<https://blog.csdn.net/weixin_42660884/article/details/85061784>

方法1

```
@Controller
public class IndexController {
    @RequestMapping({"", "login"}) //这里为空或者是login都能进入该方法
    public void login(HttpServletResponse response) {
        System.out.println(123);
        try {
            response.sendRedirect("redis/r2/");
        } catch (IOException e) {
            e.printStackTrace();
        }
        return;
    }

}
```

方法2

```
@Configuration
public class MVCConfiguration extends WebMvcConfigurerAdapter {
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {

        registry.addViewController("/").setViewName("forward:/user/mai");
        //registry.addViewController("/").setViewName("redirect:" + homePage);
        registry.setOrder(Ordered.HIGHEST_PRECEDENCE);

        super.addViewControllers(registry);

    }
}
```



# springboot默认的欢迎页面设置

<https://blog.csdn.net/qq_29477175/article/details/82117177>



# [Spring Boot 2.0 设置网站默认首页](https://www.cnblogs.com/itshare/p/8694595.html)



[SpringBoot返回页面,设置首页(默认页)跳转](https://www.cnblogs.com/cnsdhzzl/p/10894074.html)