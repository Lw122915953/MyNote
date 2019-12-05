# 一、IOC和DI

* 控制反转：将创建对象交给容器管理
* 依赖注入：将属性值注入给属性，将属性注入给了bean，将bean注入给了ioc容器(三种注入方式)
  1. set注入
  2. 构造器注入
  3. p命名空间注入

* 配置注入方式
  1. <bean id="" class=""  autowired="" /> byName和byType
  2. 注意list、map、引用类型的注入
* Bean标注注解（先配置扫描包）
  1. @Component：适用于所有层
  2. @Repository：适用于dao层
  3. @Service：适用于Service层
  4. @Controller ：常用于对Controller实现类进行标注
* Bean属性注入注解
  1. @Value ：注入普通类型属性
  2. @Resource ：注入对象类型
  3. @Autowired ：注入对象类型，默认按照类型注入。结合@Qualifier注解完成按名称的注入

# 二、手写简单工厂

~~~java
package com.bianyiit.practice01.g;

public class Test01 {
    public static void main(String[] args) {
        FactoryBean factoryBean = new FactoryBean();
        Fruit fruit = factoryBean.getInstance("com.bianyiit.practice01.g.Orange");
        fruit.eat();
    }
}

class FactoryBean {
    public Fruit getInstance(String string) {
        Fruit fruit = null;
        try {
            fruit = (Fruit) Class.forName(string).newInstance();
            return fruit;
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }
}

interface Fruit {
    public void eat();
}

class Apple implements Fruit {

    @Override
    public void eat() {
        System.out.println("吃Apple");
    }
}

class Orange implements Fruit {
    @Override
    public void eat() {
        System.out.println("吃Orange");
    }
}
~~~





# 三、Spring创建Bean的三种方式的使用和区别

在学习Spring的时候，发现Spring的IOC（控制反转）为我们提供的三种创建Bean的方式。

## 1.Spring创建Bean的三种方式

这里采用XML配置，分别演示三种创建Bean的方式和代码。

先创建一个Bean   User类  三种方式都是为了得到这个User的对象

```java
/** * User对象 * @author:LiChong * @date:2018/7/28 */public class User {   // 这里只是一个空对象}
```

### 1.1 采用默认的无参构造创建实例

  XML配置：

```html
<!-- 默认的无参构建 -->    <bean id="user" class="ioc.pojo.User"></bean>
```

  测试：

```java
@Testpublic void testUser(){	ApplicationContext context = new ClassPathXmlApplicationContext("classpath:applicationContext.xml");	// 默认的无参构造创建	User user = (User) context.getBean("user");	System.out.println("默认的无参构造创建:" + user);}
```

 控制台输出：  **默认的无参构造创建:ioc.pojo.User@3b088d51**

### 1.2 采用静态工厂创建实例

 配置工厂类：

```java
/** * User对象的工厂类 * @author:LiChong * @date:2018/7/28 */public class UserFactory {		// 静态方法	public static User getUser1() {		return new User();	} }
```

 XML配置：

```html
  <!-- 使用静态工厂创建user -->  <bean id="user1" class="ioc.service.UserFactory" factory-method="getUser1"></bean>
```

 class 指的是该工厂类的包路径，factory-method 指的是该工厂类创建Bean的静态方法。注意：这里一定要静态方法

 测试：

```java
@Testpublic void testUser1(){	ApplicationContext context = new ClassPathXmlApplicationContext("classpath:applicationContext.xml");		// 静态工厂创建	User user1 = (User) context.getBean("user1");	System.out.println("静态工厂创建:" + user1);}
```

控制台输出结果：**静态工厂创建:ioc.pojo.User@3b088d51**

### 1.3 采用实例工厂创建实例

 配置工厂类：

```java
/** * User对象的工厂类 * @author:LiChong * @date:2018/7/28 */public class UserFactory {		//普通方法	public User getUser2() {		return new User();	}}
```

XML配置：

```html
<!-- 使用实例工厂创建 user -->    <bean id="userFactory" class="ioc.service.UserFactory"></bean>    <bean id="user2" factory-bean="userFactory" factory-method="getUser2"></bean>
```

测试：

```java
	@Test	public void testUser2(){		ApplicationContext context = new ClassPathXmlApplicationContext("classpath:applicationContext.xml");		// 实例工厂创建		User user2 = (User) context.getBean("user2");		System.out.println("实例工厂创建:" + user2);	}
```

控制台输出结果：**实例工厂创建:ioc.pojo.User@3b088d51**

**好了，实现了Spring三种Bean，感觉很顺利。**

\-----------------------------------------------------------------------------------------------------------------------------------------------------------------

**那么问题来了，为什么Spring要提供三种创建Bean的方式呢？**

**这三种创建Bean的方式又有什么区别呢？接下来开始做实验。**

**实验一： 三种方式创建的Bean是否有联系？**

测试：

```java
        @Test	public void test1(){		ApplicationContext context = new ClassPathXmlApplicationContext("classpath:applicationContext.xml");		// 默认的无参构造创建		User user = (User) context.getBean("user");		System.out.println("默认的无参构造创建:" + user);		// 静态工厂创建		User user1 = (User) context.getBean("user1");		System.out.println("静态工厂创建:" + user1);		// 实例工厂创建		User user2 = (User) context.getBean("user2");		System.out.println("实例工厂创建:" + user2);	}
```

控制台输出结果：

**默认的无参构造创建:ioc.pojo.User@3b088d51静态工厂创建:ioc.pojo.User@1786dec2实例工厂创建:ioc.pojo.User@74650e52**

结论：三种方式都是创建一个新的实例对象。实例对象都是独立的，没有联系！

**实验二： 三种方式创建的Bean的时机是否不同？**

这里采用 bean的配置 init-method 初始化方法来查看Bean的实例是什么时候被加载的

是在加载配置文件的时候？还是在调用getBean()方法的时候？

修改User类，添加init()方法

```java
/** * User对象 * @author:LiChong * @date:2018/7/28 */public class User { 	public void init(){		System.out.println("user被初始化啦");	}}
```

XML配置：

```html
    <!-- 默认的无参构建 -->    <bean id="user" class="ioc.pojo.User" init-method="init"></bean>     <!-- 使用静态工厂创建user -->    <bean id="user1" class="ioc.service.UserFactory" factory-method="getUser1" init-method="init"></bean>     <!-- 使用实例工厂创建 user -->    <bean id="userFactory" class="ioc.service.UserFactory"></bean>    <bean id="user2" factory-bean="userFactory" factory-method="getUser2" init-method="init"></bean>
```

测试：

```java
	@Test	public void test2(){		ApplicationContext context = new ClassPathXmlApplicationContext("classpath:applicationContext.xml");		System.out.println("=====================================");		// 默认的无参构造创建		User user = (User) context.getBean("user");		System.out.println("默认的无参构造创建:" + user);		// 静态工厂创建		User user1 = (User) context.getBean("user1");		System.out.println("静态工厂创建:" + user1);		// 实例工厂创建		User user2 = (User) context.getBean("user2");		System.out.println("实例工厂创建:" + user2);	}
```

控制台输出结果：

**user被初始化啦user被初始化啦user被初始化啦=====================================默认的无参构造创建:ioc.pojo.User@3b088d51静态工厂创建:ioc.pojo.User@1786dec2实例工厂创建:ioc.pojo.User@74650e52**

结论：从初始化方法可以看出，Spring这三种创建实例的方式都是一样的，在加载配置文件的时候就创建了实例，证明这三种方式实例加载的时机是一样的。

========================================================================================

**很明显，这三种方式最根本的区别还是创建方式的不同。**

**第一种，通过默认的无参构造方式创建，其本质就是把类交给Spring自带的工厂（BeanFactory）管理、由Spring自带的工厂模式帮我们维护和创建这个类。如果是有参的构造方法，也可以通过XML配置传入相应的初始化参数，这种也是开发中用的最多的。**

**第二种，通过静态工厂创建，其本质就是把类交给我们自己的静态工厂管理，Spring只是帮我们调用了静态工厂创建实例的方法，而创建实例的这个过程是由我们自己的静态工厂实现的，在实际开发的过程中，很多时候我们需要使用到第三方jar包提供给我们的类,而这个类没有构造方法，而是通过第三方包提供的静态工厂创建的，这是时候，如果我们想把第三方jar里面的这个类交由spring来管理的话，就可以使用Spring提供的静态工厂创建实例的配置。**

**第三种，通过实例工厂创建，其本质就是把创建实例的工厂类交由Spring管理，同时把调用工厂类的方法创建实例的这个过程也交由Spring管理，看创建实例的这个过程也是有我们自己配置的实例工厂内部实现的。在实际开发的过程中，如Spring整合Hibernate就是通过这种方式实现的。但对于没有与Spring整合过的工厂类，我们一般都是自己用代码来管理的。**

 