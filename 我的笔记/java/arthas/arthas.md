# 1、下载

```shell
https://alibaba.github.io/arthas/arthas-boot.jar
```

# 2、运行

```shell
java -jar arthas-boot.jar
```

java -XX:+PrintFlagsWithComments //debug版本才有

java -XX:+PrintFlagsFinal

java -XX:+PrintFlagsInitial



java -XX:+PrintFlagsFinal |wc -l



E:\projects\jvm\src\main\java>java -Xms200M -Xmx200M -XX:+PrintGC com.maimai.jvm.example.FullGC01



jstat -gc 

jmap -histo 15156