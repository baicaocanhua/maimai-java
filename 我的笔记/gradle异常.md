# org.jetbrains.plugins.gradle.tooling.util.ModuleComponentIdentifierImpl.getModuleIdentifier()



# 解决方法

gradle  降级处理 从5变成4





# gradle版本引起编译报错问题：Cause: org.jetbrains.plugins.gradle.tooling.util.ModuleComponentIdentifierImpl.getM



将同事写的JAVA项目下载，编译，发现不成功。报这样的错：

Cause: org.jetbrains.plugins.gradle.tooling.util.ModuleComponentIdentifierImpl.getModuleIdentifier()
1
究其原因，是gradle版本导致的。从它的报错也能看出，跟我们自己写的代码无关。

怎么改呢？

凡应用gradle进行代码依赖管理的项目，从/gradle/wrapper/gradle-wrapper.properties，会指明用哪个gradle版本，并自动下载。（就是说，本地不用安装gradle也可以咯？）

#Sat Mar 09 10:27:01 CST 2019
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
#对，就是这一行
distributionUrl=https\://services.gradle.org/distributions/gradle-4.7-bin.zip
1
2
3
4
5
6
7
原本上面指明是：
gradle-5.2.1-all.zip

但他的idea是2018.3版本，而我的是2018.1，可能就是这个原因，导致他那边没问题，而我的就不行。所以我将gradle改成4.7，就可以了：

distributionUrl=https\://services.gradle.org/distributions/gradle-4.7-bin.zip
--------------------- 




# Gradle 5 IntelJ java.lang.AbstractMethodError

<https://www.jianshu.com/p/f9951d5506fc>