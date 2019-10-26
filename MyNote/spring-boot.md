#  一：spring-boot

## 1.1 入门示例

1、创建maven工程，不创建骨架

2、引入pom.xml坐标

```java
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 				  	 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.tianhecloud</groupId>
    <artifactId>W0911Spring-boot</artifactId>
    <version>1.0-SNAPSHOT</version>

   <!--父依赖-->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.9.RELEASE</version>
    </parent>
    <dependencies>
        <!--场景应用器-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>	
    <build>
        <plugins>
            <!--这个插件可以将应用打包成一个可执行的jar包-->
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

3、创建启动主程序类（控制层类需要放在启动主程序类当前包或其当前包的子包下面）

```java
package com.tianhecloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MainApplication {
    public static void main(String[] args) {
        SpringApplication.run(MainApplication.class,args);
    }
}
```

4、创建控制层测试类

```java
package com.tianhecloud.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class HelloController {
    @ResponseBody
    @RequestMapping("/hello")
    public String hello() {
        return "Hello World!";
    }
}
```

5、打包部署，点击package后生成一个jar包

![1568170110729](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1568170110729.png)

![1568170171842](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1568170171842.png)

6、运行服务，成功后可以使用浏览器访问

![1568170231034](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1568170231034.png)

## 1.2 使用向导快速创建spring-boot应用

1、创建项目

![1568171175795](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1568171175795.png)

![1568171247086](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1568171247086.png)

![1568171314651](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1568171314651.png)

![1568171331405](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1568171331405.png)

## 1.3 spring-boot配置（以yml为例）

1、修改默认配置（例如端口号）

![1568171760692](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1568171760692.png)

![1568172409472](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1568172409472.png)

```java
    	<!--导入配置文件处理器，配置文件进行绑定yml就会有提示-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>
```

2、注入示例(实现注入)

![1568181241839](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1568181241839.png)

```java
package com.tianhecloud.entity;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

import java.util.Date;
import java.util.List;
import java.util.Map;
@Component
@ConfigurationProperties(prefix="person")
public class Person {
    private String lastName;
    private Integer age;
    private Boolean boss;
    private Date birth;
    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;

    public Person() {
    }

    public Person(String lastName, Integer age, Boolean boss, Date birth, Map<String, Object> maps, List<Object> lists, Dog dog) {
        this.lastName = lastName;
        this.age = age;
        this.boss = boss;
        this.birth = birth;
        this.maps = maps;
        this.lists = lists;
        this.dog = dog;
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public Boolean getBoss() {
        return boss;
    }

    public void setBoss(Boolean boss) {
        this.boss = boss;
    }

    public Date getBirth() {
        return birth;
    }

    public void setBirth(Date birth) {
        this.birth = birth;
    }

    public Map<String, Object> getMaps() {
        return maps;
    }

    public void setMaps(Map<String, Object> maps) {
        this.maps = maps;
    }

    public List<Object> getLists() {
        return lists;
    }

    public void setLists(List<Object> lists) {
        this.lists = lists;
    }

    public Dog getDog() {
        return dog;
    }

    public void setDog(Dog dog) {
        this.dog = dog;
    }

    @Override
    public String toString() {
        return "Person{" +
                "lastName='" + lastName + '\'' +
                ", age=" + age +
                ", boss=" + boss +
                ", birth=" + birth +
                ", maps=" + maps +
                ", lists=" + lists +
                ", dog=" + dog +
                '}';
    }
}

```

```java
package com.tianhecloud.entity;

public class Dog {
    private String name;
    private Integer age;

    public Dog() {
    }

    public Dog(String name, Integer age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Dog{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}

```

```yam
server:
  port: 8081
person:
  lastName: hello
  age: 18
  boss: false
  birth: 2019/10/10
  maps: {k1: v1,k2: 12}
  lists:
    - lisi
    - zhaoliu
  dog:
    name: 小狗
    age: 12
```

3、@ImportResource方式注入

![1568184381391](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1568184381391.png)

```java
package com.tianhecloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.ImportResource;
import org.springframework.stereotype.Component;
@ImportResource(locations = {"classpath:beans.xml"})  //需要引入xml文件
@SpringBootApplication
public class W0911springBoot01Application {

    public static void main(String[] args) {
        SpringApplication.run(W0911springBoot01Application.class, args);
    }
}
```

```java
package com.tianhecloud.entity;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.context.ApplicationContext;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest
public class W0911springBoot01ApplicationTests {
    @Autowired
    Person person;

    @Autowired
    ApplicationContext ioc;

    @Test
    public void testHelloService() {
        boolean bool = ioc.containsBean("helloService");
        System.out.println(bool); //成功打印true
    }
    
    @Test
    public void contextLoads() {
    }
}
```

3、用类配置

```java
package com.tianhecloud.controller;

import com.tianhecloud.entity.Person;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class HelloController {
    @Autowired
    private Person person;

    @ResponseBody
    @RequestMapping("/hello")
    public String hello01() {
        System.out.println(person.toString());
        return "Hello World!";
    }
}
```

```java
package com.tianhecloud.entity;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.context.ApplicationContext;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest
public class W0911springBoot01ApplicationTests {
    @Autowired
    Person person;

    @Autowired
    ApplicationContext ioc;

    @Test
    public void testHelloService() {
        boolean bool = ioc.containsBean("helloService");
        System.out.println(bool);  // 打印true
     }

    @Test
    public void contextLoads() {
    }
}
```

​	

## 1.4 配置文件加载顺序和位置  

spring boot 启动会扫描以下位置的application.properties或者application.yml文件作为spring boot的默认配置文件

-- file:./config/

-- file:./

-- classpath:/config/

-- classpath:/

-- 以上是按照优先级从高到低的顺序，所有位置的文件都会被加载，高优先级配置内容会覆盖低优先级配置内容

-- 我们也可以通过配置spring.config.location来改变默认配置

## 1.5 分页工具的使用

~~~java
package com.tianhecloud.service.impl;

import com.tianhecloud.dataobject.ProductInfo;
import com.tianhecloud.service.ProductService;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest
public class PageHelpTest {
    @Autowired
    ProductService productService;

    @Test
    public void findAll() {
        PageRequest request = new PageRequest(1, 2);//第一个参数当前页，第二个参数当前页面数量
        Page<ProductInfo> productInfos = productService.findAll(request);
        for (ProductInfo productInfo : productInfos) {
            System.out.println(productInfo);
        }
        System.out.println(productInfos.getTotalElements());
    }
}
~~~



