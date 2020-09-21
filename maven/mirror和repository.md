# maven的setting配置文件中mirror和repository的区别

https://www.jianshu.com/p/274c363ffd7c



```xml
  <mirrors>
        <!--国内阿里云提供的镜像，非常不错-->
    <mirror>
        <!--This sends everything else to /public -->
        <id>aliyun_nexus</id>
        <!--对所有仓库使用该镜像,除了一个名为maven_nexus_201的仓库除外-->
        <!--这个名为maven_nexus_201的仓库可以在javamaven项目中配置一个repository-->
        <mirrorOf>*,!maven_nexus_201</mirrorOf> 
        <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
    </mirror>
  </mirrors>
```



```xml
<repositories>
        <!-- 192.168.0.201远程仓库 -->
        <repository>
            <id>maven_nexus_201</id>
            <name>maven_nexus_201</name>
            <layout>default</layout>
            <url>http://192.168.0.201:8081/nexus/content/groups/public/</url>
            <snapshots>  
                <enabled>true</enabled>  
              </snapshots>
        </repository>
</repositories>
```