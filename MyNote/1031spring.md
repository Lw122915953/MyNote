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