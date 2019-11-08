# 一、回顾

![1572358262475](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1572358262475.png)

# 二、自定义日期转换

1. 配置类

   ~~~java
   package com.bianyiit.date;
   
   import org.springframework.core.convert.converter.Converter;
   
   import java.text.ParseException;
   import java.text.SimpleDateFormat;
   import java.util.Date;
   
   public class DateConverter implements Converter<String, Date> {
       @Override
       public Date convert(String s) {
           if (s != null) {
               try {
                   SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");
                   return dateFormat.parse(s);
               } catch (ParseException e) {
                   e.printStackTrace();
               }
           }
           return null;
       }
   }
   ~~~

   

2. 在springmvc.xml中配置

~~~xml

    <context:component-scan base-package="com.bianyiit"></context:component-scan>

    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/"></property>
        <property name="suffix" value=".jsp"></property>
    </bean>
    <!--配置自己写好的转换器-->
    <bean id="dateConverter" class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
        <property name="converters">
            <list>
                <bean class="com.bianyiit.date.DateConverter"/>
            </list>
        </property>
    </bean>
    <!--配置注解驱动，让转换器生效-->
    <mvc:annotation-driven conversion-service="dateConverter"/>
~~~

3. 总结

   ![1572361524093](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1572361524093.png)

# 三、自定义异常



1. 自定义异常跳转页面

~~~java
package com.bianyiit.exception;

import org.springframework.web.servlet.HandlerExceptionResolver;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class GlobalExceptionResolver implements HandlerExceptionResolver {
    @Override
    public ModelAndView resolveException(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) {
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("msg", e.getMessage());
        modelAndView.setViewName("error");
        return modelAndView;
    }
}
~~~

2. 配置springmvc.xml

~~~xml
<bean class="com.bianyiit.exception.GlobalExceptionResolver"></bean>
~~~

# 四、自定义拦截器

1. 自定义拦截类

~~~java
package com.bianyiit.intecepter;


import org.springframework.lang.Nullable;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class CustomIntecepter1 implements HandlerInterceptor {

    /**
     * 请求到达目标资源之前先执行
     * @param request
     * @param response
     * @param handler
     * @return
     * @throws Exception
     */
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
            throws Exception {
        System.out.println("CustomIntecepter1   preHandle....");


//        request.getRequestDispatcher("/WEB-INF/views/error.jsp").forward(request,response);

//        response.sendRedirect("/WEB-INF/views/error.jsp");
        /*
            如果返回true放行请求
            如果返回false 拦截请求（不放行请求）
         */
        return false;
    }

    /**
     * 目标资源执行完毕之后执行
     * @param request
     * @param response
     * @param handler
     * @param modelAndView
     * @throws Exception
     */
   public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
                    @Nullable ModelAndView modelAndView) throws Exception {




       System.out.println("CustomIntecepter1  postHandle .....");

//       modelAndView.setViewName("error");
    }

    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler,
                         @Nullable Exception ex) throws Exception {

        System.out.println("CustomIntecepter1   afterCompletion........");
    }
}

~~~

2. springmvc.xml配置

~~~xml
 <!--springmvc拦截器配置-->
    <mvc:interceptors>
        <!--拦截器谁先配置谁先执行-->
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <bean class="com.bianyiit.intecepter.CustomIntecepter2"></bean>
        </mvc:interceptor>
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <bean class="com.bianyiit.intecepter.CustomIntecepter1"></bean>
        </mvc:interceptor>
    </mvc:interceptors>

~~~



3. 总结

![1572362772412](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1572362772412.png)

# 五、restful

![1572363081544](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1572363081544.png)

# 六、静态资源过滤

~~~java
<!-- 配置静态资源 -->
<mvc:resources location="/static/" mapping="/static/**"/>

 

说明：
location元素：表示webapp目录下（即服务器根目录）的static包下的所有文件；
mapping元素：表示以/static开头的所有请求路径，如/static/a 或者/static/a/b；
 
该配置的作用是：DispatcherServlet不会拦截以/static开头的所有请求路径，并当作静态资源交由Servlet处理；
 
实例：
    当我们在项目中需要引入js,css,json等资源文件时，而你在web.xml中刚好这样配置了拦截规则
    
    
    <servlet>
    <servlet-name>controller</servlet-name>
    <servlet-class>
        org.springframework.web.servlet.DispatcherServlet
    </servlet-class>
     <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>
            classpath:spring-mvc-controller.xml
        </param-value>
    </init-param>
</servlet>
<servlet-mapping>
    <servlet-name>controller</servlet-name>
    <url-pattern>/</url-pattern>  //配置成'/'即出了jsp文件外其他都拦截，'/*' 即拦截所有
</servlet-mapping>
~~~

然后 当在页面上引入js,css等文件时，springMvc会拦截这些文件 。然后 你的页面就好丑了。。
 项目目录：

　 　　![img](https://images2015.cnblogs.com/blog/740292/201704/740292-20170421212914259-400135196.png)

在页面上这样应用就可以了

~~~java
 <link rel="stylesheet" type="text/css" href="static/easyui/themes/default/easyui.css"/>
    <link rel="stylesheet" type="text/css" href="static/easyui/themes/icon.css"/>

　<script src="static/js/jquery.min.js" type="text/javascript" charset="utf-8"></script>
~~~





