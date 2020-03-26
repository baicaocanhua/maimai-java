

# [【SSO单点系列】（4）：CAS4.0 SERVER登录后用户信息的返回](https://www.cnblogs.com/vhua/p/cas_4.html)

## CAS获取用户更多信息

https://my.oschina.net/xiaokaceng/blog/182547?p=1



# [CAS实战の获取多用户信息](https://www.cnblogs.com/tomcatx/p/4585040.html)

# cas 4.2.5 返回更多的用户信息  ****

https://blog.csdn.net/Mr_yangzc/article/details/71438828

# 1.找到cas server 中的 deployerConfigContext.xml

## 注释掉   

```xml
<!-- <bean id="attributeRepository" class="org.jasig.services.persondir.support.NamedStubPersonAttributeDao"
        p:backingMap-ref="attrRepoBackingMap" /> -->



    <!-- <util:map id="attrRepoBackingMap">

       <entry key="uid" value="uid" />
       <entry key="eduPersonAffiliation" value="eduPersonAffiliation" /> 

       <entry key="groupMembership" value="groupMembership" />

       <entry>

        <key>

                   <value>memberOf</value>

         </key>

         <list>

              <value>faculty</value>

                <value>staff</value>

               <value>org</value>
        </list>

    </entry>

        </util:map> -->
--------------------- 
作者：Mr丶YangZCH 
来源：CSDN 
原文：https://blog.csdn.net/Mr_yangzc/article/details/71438828 
版权声明：本文为博主原创文章，转载请附上博文链接！
```

## 添加代码:

```xml
<bean id="attributeRepository"
        class="org.jasig.services.persondir.support.jdbc.SingleRowJdbcPersonAttributeDao">
        <constructor-arg index="0" ref="dataSource" />
        <constructor-arg index="1"
            value="SELECT username,password,email,userId,sex,telPhone,mobileNumber, contents,sysStatus,sysUpDated,sysBack1,sysBack2,sysBack3 FROMDC_US_USERINFO WHERE {0}" />
        <property name="queryAttributeMapping">
            <map>
                <entry key="username" value="username" />
            </map>
        </property>
        <property name="resultAttributeMapping">
            <map>
                <!--key为对应的数据库字段名称，value为提供给客户端获取的属性名字，系统会自动填充值 -->
                <entry key="username" value="username"></entry>
                    <entry key="password" value="password"></entry>
                <entry key="email" value="email"></entry>
                <entry key="userId" value="userId"></entry>
                <entry key="sex" value="sex"></entry>
                <entry key="telPhone" value="telPhone"></entry>
                <entry key="mobileNumber" value="mobileNumber"></entry>
                
                <entry key="contents" value="contents"></entry>
                <entry key="sysStatus" value="sysStatus"></entry>
                <entry key="sysUpDated" value="sysUpDated"></entry>
                <entry key="sysBack1" value="sysBack1"></entry>
                <entry key="sysBack2" value="sysBack2"></entry>
                <entry key="sysBack3" value="sysBack3"></entry>
            
            </map>
        </property>
    </bean>
--------------------- 
作者：Mr丶YangZCH 
来源：CSDN 
原文：https://blog.csdn.net/Mr_yangzc/article/details/71438828 
版权声明：本文为博主原创文章，转载请附上博文链接！
```

# 2.cas-server修改casServiceValidationSuccess.jsp文件

只在文件添加下面的，其他不变

```xml
此文件是将用户登录成功后，将信息生成XML传递给客户端，原文件是只包含name信息，所以需要修改，新增获取其他属性的代码如下:

  <!-- 在server验证成功后，这个页面负责生成与客户端交互的xml信息，在默认的casServiceValidationSuccess.jsp中，只包括用户名，并不提供其他的属性信息，因此需要对页面进行扩展 -->
        <c:if test="${fn:length(assertion.chainedAuthentications[fn:length(assertion.chainedAuthentications)-1].principal.attributes) > 0}">   
            <cas:attributes>   
                <c:forEach var="attr" items="${assertion.chainedAuthentications[fn:length(assertion.chainedAuthentications)-1].principal.attributes}">                             
                    <cas:${fn:escapeXml(attr.key)}>${fn:escapeXml(attr.value)}</cas:${fn:escapeXml(attr.key)}>                                 
                </c:forEach>     
            </cas:attributes>   
        </c:if> 
--------------------- 
作者：Mr丶YangZCH 
来源：CSDN 
原文：https://blog.csdn.net/Mr_yangzc/article/details/71438828 
版权声明：本文为博主原创文章，转载请附上博文链接！
```

