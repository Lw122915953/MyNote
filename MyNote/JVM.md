# 一：Java类主动使用情况，会导致类的初始化：
1）创建类的实例
2）访问某个类或接口的静态变量，或者对该静态变量赋值
3）调用类的静态方法
4）反射（如Class.forName(“com.bunny.Test”)）
5）初始化一个类的子类
6）Java虚拟机启动时被表明为启动类的类（JavaTest）

其他使用java类方式，都可以被看作是被动使用，都不会导致类的初始化。

  