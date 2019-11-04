# 一、手写动态代理

~~~java
package com.bianyiit.test2;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class MainTest {
    public static void main(String[] args) {
        LianTongDaiLi lianTongDaiLi = new LianTongDaiLi();
        LianTong proxy = ProxyFactory.getProxy(lianTongDaiLi);
        String chf = proxy.chf(10);
        System.out.println(chf);
    }
}

interface LianTong {
    public String chf(double huafei);

    public String msj(double maisouji);
}

class LianTongDaiLi implements LianTong {
    @Override
    public String chf(double huafei) {
        System.out.println("你充了" + huafei + "元话费");
        return "充了话费";
    }

    @Override
    public String msj(double maisouji) {
        System.out.println("你买了个" + maisouji + "的手机");
        return "买了手机";
    }
}

class ProxyFactory {
    public static LianTong getProxy(LianTongDaiLi lianTongDaiLi) {
        LianTong o = (LianTong) Proxy.newProxyInstance(lianTongDaiLi.getClass().getClassLoader(), lianTongDaiLi.getClass().getInterfaces(), new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                Object invoke = method.invoke(lianTongDaiLi, args);
                return invoke;
            }
        });
        return o;
    }
}

~~~

