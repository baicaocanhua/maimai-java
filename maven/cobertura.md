mvn cobertura:cobertura -Dcobertura.aggregate=true -Dcobertura.report.format=xml

-Dcobertura.outputDirectory=./target/tmpCobertura



-Dcobertura.aggregate=true -Dcobertura.outputDirectory=./target

mvn clean cobertura:cobertura -Dcobertura.skip=true


mvn clean cobertura:cobertura -Dcobertura.report.format=xml

mvn test -Dtest=Random*Test,AccountCaptchaServiceTest  
mvn test -Dtest -DfailIfNoTests=false  





Cobertura在Maven多模块项目上

https://stackoverflow.com/questions/1420724/cobertura-on-maven-multi-module-project

Cobertura 统计多模块maven项目测试覆盖率
https://blog.csdn.net/shymi1991/article/details/52849947

Jenkins集成Cobertura显示代码测试覆盖率报告
https://www.jianshu.com/p/184c43ee3dbc

jenkins集成cobertura，调用显示cobertura的report
https://blog.csdn.net/yaominhua/article/details/40684647


Jenkins集成Maven代码覆盖率插件Cobertura
https://blog.csdn.net/boonya/article/details/77448680

cobertura-maven-plugin遇到java lamda表达式
https://blog.csdn.net/gaofuliang/article/details/80097495



# [学习Maven之Cobertura Maven Plugin](https://www.cnblogs.com/qyf404/p/5040593.html)

D