1. 手写aop

![](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1572869994244.png)

2. aop原理



![1572869974917](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1572869974917.png)

3. 总结execution的写法

   * 完成写法

     public  void com.bianyiit.service.impl.UserServiceImpl.saveUser(String)

   * 方法访问修饰符可以被省略

     void com.bianyiit.service.impl.UserServiceImpl.saveUser(String)

   * 方法返回值类型可以用 * 号代替

   ​        *   com.bianyiit.service.impl.UserServiceImpl.saveUser(String)

      * 报名和类名可以省略

        ​       *   save*(..)

   * 整个表达式中只有方法返回值类型、方法名、方法参数不能省略，其它都不能省略

     ​        *    *(..)