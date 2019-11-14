# 一、配置(了解阶段)

1.pom.xml引入

~~~java
<!-- Spring Security -->
<dependency>
	<groupId>org.springframework.security</groupId>
	<artifactId>spring-security-web</artifactId>
	<version>4.2.3.RELEASE</version>
</dependency>
<dependency>
	<groupId>org.springframework.security</groupId>
	<artifactId>spring-security-config</artifactId>
	<version>4.2.3.RELEASE</version>
</dependency>
~~~

2.web.xml配置

~~~java
<context-param>
  <param-name>contextConfigLocation</param-name>
  <param-value>classpath:applicationContext.xml,classpath:spring-       security.xml</param-value>
</context-param>


<filter>
  <filter-name>springSecurityFilterChain</filter-name>
  <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
</filter>
<filter-mapping>
  <filter-name>springSecurityFilterChain</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
~~~

3.springSecurity.xml配置

~~~java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:security="http://www.springframework.org/schema/security" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd http://www.springframework.org/schema/security
http://www.springframework.org/schema/security/spring-security-4.2.xsd">
    <!-- <security:http>: spring
    过 滤 器 链 配 置 ：
    1 ） 需 要 拦 截 什 么 资 源
    2 ） 什 么 资 源 什 么 角 色 权 限
    3 ） 定 义 认 证 方 式 ： HttpBasic ， FormLogin （ * ）
    4 ） 定 义 登 录 页 面 ， 定 义 登 录 请 求 地 址 ， 定 义 错 误 处 理 方 式
    -->
    <security:http>
        <!--    pattern:
            需 要 拦 截 资 源
            access:
            拦 截 方 式
            isFullyAuthenticated():
            该 资 源 需 要 认 证 才 可 以 访 问
            isAnonymous(): 只 有 匿 名 用 户 才 可 以 访 问 （ 如 果 登 录 用 户 就 无 法 访 问 ）
            permitAll(): 允 许 所 有 人 （ 匿 名 和 登 录 用 户 ） 方 法
            hasRole()：拥有指定权限的用户才可以访问
           -->
        <security:intercept-url pattern="/product/index" access="permitAll()"/>
        <security:intercept-url pattern="/userLogin" access="permitAll()"/>
        <security:intercept-url pattern="/product/add" access="hasRole('ROLE_USER')"/>
        <security:intercept-url pattern="/product/update" access="hasRole('ROLE_USER')"/>
        <security:intercept-url pattern="/product/list" access="hasRole('ROLE_ADMIN')"/>
        <security:intercept-url pattern="/product/delete" access="hasRole('ROLE_ADMIN')"/>
        <security:intercept-url pattern="/**" access="isFullyAuthenticated()"/>
        <!--login-page拦截资源调至自定义登陆页面，login-processing-url拦截/securityLogin防止反复重定向-->
        <security:form-login login-page="/userLogin" login-processing-url="/securityLogin"/>
        <!--拦截错误页面跳至/error-->
        <security:access-denied-handler error-page="/error"></security:access-denied-handler>
        <!--为true可以跨域-->
        <security:csrf disabled="true"></security:csrf>
    </security:http>
    <!--  <!-
      security:authentication-manager ： 认 证 管 理 器
      1 ） 认 证 信 息 提 供 方 式 （ 账 户 名 ， 密 码 ， 当 前 用 户 权 限 ）
      &ndash;&gt;-->
    <security:authentication-manager>
        <security:authentication-provider>
            <security:user-service>
                <security:user name="s" password="s" authorities="ROLE_USER"/>
                <security:user name="a" password="a" authorities="ROLE_ADMIN"/>
            </security:user-service>
        </security:authentication-provider>
    </security:authentication-manager>
</beans>
~~~

5.总览

![1573660325639](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573660325639.png)