1：注释掉

```xml
<bean id="primaryAuthenticationHandler" class="org.jasig.cas.authentication.AcceptUsersAuthenticationHandler"> 
	<property name="users"> 
		<map> 
			<entry key="casuser" value="Mellon" />
        	<entry key="maimai" value="maimai" /> 
        </map> 
     </property> 
</bean>
```



2：添加

```xml

	<!-- 配置认证类 -->
	<bean id="dbAuthHandler"
		class="org.jasig.cas.adaptors.jdbc.QueryDatabaseAuthenticationHandler">
		<!--dataSource指向上面配置的dataSource bean -->
		<property name="dataSource" ref="dataSource"></property>
		<property name="sql"
			value="select password from cas_t_user where user_name=?"></property>
		<!--passwordEncoder 指向上面配置的 passwordEncoder bean -->
		<property name="passwordEncoder" ref="MD5PasswordEncoder"></property>
	</bean>


	<!-- 添加以下bean 数据库用户名密码按自己的配置 -->
	<!-- 添加数据源 -->
	<bean id="dataSource"
		class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName"
			value="com.mysql.jdbc.Driver" />
		<property name="url"
			value="jdbc:mysql://localhost/test?useUnicode=true&amp;characterEncoding=utf8&amp;serverTimezone=GMT" />
		<property name="username" value="root" />
		<property name="password" value="123456" />
	</bean>


	<!-- 配置passwordEncoder -->
	<bean id="MD5PasswordEncoder"
		class="org.jasig.cas.authentication.handler.DefaultPasswordEncoder">
		<constructor-arg index="0" value="MD5" />
	</bean>
```



3：修改

```
<bean id="authenticationManager"
		class="org.jasig.cas.authentication.PolicyBasedAuthenticationManager">
		<constructor-arg>
			<map>
				<!-- | IMPORTANT | Every handler requires a unique name. | If more than 
					one instance of the same handler class is configured, you must explicitly 
					| set its name to something other than its default name (typically the simple 
					class name). -->
				<entry key-ref="proxyAuthenticationHandler"
					value-ref="proxyPrincipalResolver" />
                 注释掉这行
				<!-- <entry key-ref="primaryAuthenticationHandler" value-ref="primaryPrincipalResolver" 
					/> -->
					添加下面这行
				<entry key-ref="dbAuthHandler"
					value-ref="primaryPrincipalResolver" />

			</map>
		</constructor-arg>

		<!-- Uncomment the metadata populator to allow clearpass to capture and 
			cache the password This switch effectively will turn on clearpass. <property 
			name="authenticationMetaDataPopulators"> <util:list> <bean class="org.jasig.cas.extension.clearpass.CacheCredentialsMetaDataPopulator" 
			c:credentialCache-ref="encryptedMap" /> </util:list> </property> -->

		<!-- | Defines the security policy around authentication. Some alternative 
			policies that ship with CAS: | | * NotPreventedAuthenticationPolicy - all 
			credential must either pass or fail authentication | * AllAuthenticationPolicy 
			- all presented credential must be authenticated successfully | * RequiredHandlerAuthenticationPolicy 
			- specifies a handler that must authenticate its credential to pass -->
		<property name="authenticationPolicy">
			<bean
				class="org.jasig.cas.authentication.AnyAuthenticationPolicy" />
		</property>
	</bean>
```

