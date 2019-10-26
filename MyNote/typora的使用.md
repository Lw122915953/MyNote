# Typora使用说明

## 菜单

[TOC]



- Typora使用说明
  - [菜单](https://www.e-learn.cn/content/qita/573336#菜单)
  - [区域元素](https://www.e-learn.cn/content/qita/573336#区域元素)
  - [段落](https://www.e-learn.cn/content/qita/573336#段落)
  - [标题](https://www.e-learn.cn/content/qita/573336#标题)
  - [引注](https://www.e-learn.cn/content/qita/573336#引注)
  - [序列](https://www.e-learn.cn/content/qita/573336#序列)
  - [可选序列](https://www.e-learn.cn/content/qita/573336#可选序列)
  - [代码块](https://www.e-learn.cn/content/qita/573336#代码块)
  - [数学块](https://www.e-learn.cn/content/qita/573336#数学块)
  - [表格](https://www.e-learn.cn/content/qita/573336#表格)
  - [脚注](https://www.e-learn.cn/content/qita/573336#脚注)
  - [水平线](https://www.e-learn.cn/content/qita/573336#水平线)
  - 特征元素
    - [连接](https://www.e-learn.cn/content/qita/573336#连接)
    - [超链接](https://www.e-learn.cn/content/qita/573336#超链接)
    - [相关链](https://www.e-learn.cn/content/qita/573336#相关链)
    - [URLs](https://www.e-learn.cn/content/qita/573336#urls)
  - [图片](https://www.e-learn.cn/content/qita/573336#图片)
  - [斜体](https://www.e-learn.cn/content/qita/573336#斜体)
  - [加粗](https://www.e-learn.cn/content/qita/573336#加粗)
  - [删除线](https://www.e-learn.cn/content/qita/573336#删除线)
  - [下划线](https://www.e-learn.cn/content/qita/573336#下划线)
  - [代码](https://www.e-learn.cn/content/qita/573336#代码)
  - [数学式](https://www.e-learn.cn/content/qita/573336#数学式)
  - [下标](https://www.e-learn.cn/content/qita/573336#下标)
  - [上标](https://www.e-learn.cn/content/qita/573336#上标)
  - [插入表情](https://www.e-learn.cn/content/qita/573336#插入表情)
  - [高亮](https://www.e-learn.cn/content/qita/573336#高亮)
- [快捷键](https://www.e-learn.cn/content/qita/573336#快捷键)

输入[toc]即可产生菜单，菜单会自动更新

```中文
[toc]
```

## 区域元素

在文章的最上方输入—，按换行键产生，然后在里面输入内容即可。

## 段落

按换行键可以建立新的一行

可在行尾插入打断线，禁止向后插入，以后的内容会重新换行

```中文
打断线<br/>后面的内容将自动换行
```

## 标题

开头#的个数表示，空格加文字。标题有1~6个级别，#表示开始，按换行建结束

```中文
# 一号标题
## 二号标题
······
###### 六号标题 
```



## 引注

开头>表示，空格+文字，按换行键换行，双按换行跳出

> 我的引注1
>
> 我的引注2
>
> 还有一行，双按换行建跳出引注模式

```中文
>引注1
>···
>引注2
```

## 序列

开头*/+/- 加空格+文字，可以创建无序序列，换行键换行，删除键+shift+tab跳出

开头1.加空格+后接文字，可以创建有序序列

例：

- 第一个无序序列
- 第二个无序序列
- ······
  1. 第一个有序序列
  2. 第二个有序序列
  3. ······

## 可选序列

开头序列+空格+[ ]+空格+文字，换行键换行，删除键+shift+tab跳出

```
例如：

* [ ] 第一个可选序列
- [ ] 第二个可选序列
- [ ] ······
```

## 代码块

开头 以三个 ` +语言名，开启代码块，换行键换行，光标下移键跳出

```
java
public class Main{
    public static void main(String[] args){
        System.out.print("Java 代码块");
    }
}
```

## 数学块

开头$$+换行键，产生输入区域，输入Tex/LaTex格式的数学公式，代码和效果如下图：

```
$$
\mathbf{V}_1 \times \mathbf{V}_2 = \begin{vmatrix}
\mathbf{i} & \mathbf{j} & \mathbf{k} \
\frac{\partial X}{\partial u} & \frac{\partial Y}{\partial u} & 0 \
\frac{\partial X}{\partial v} & \frac{\partial Y}{\partial v} & 0 \
\end{vmatrix}
$$
```



**V**1×**V**2=∣∣**i****j****k** ∂*x*∂*u*∂*Y*∂*u*0 ∂*X*∂*v*∂*Y*∂*v*0 ∣∣V1×V2=|ijk ∂x∂u∂Y∂u0 ∂X∂v∂Y∂v0 |

## 表格

开头|+列名+|+列名+|+换行键，创建一个2*2表格

将鼠标放置其上，弹出编辑尺寸，个数，文字等

```
例如：
|第一列|第二列|
```

| 第一列 | 第二列 |
| :----- | :----- |
|        |        |

## 脚注

在需要添加脚注的文字后面+[+^+序列+]，注释的产生可以鼠标放置其上单击自动产生，添加信息

或人工添加+[+^+序列+]+:

```
脚注产生的方法[^footnote].
[^footnote]:这个就是"脚注"
```

脚注的产生方法[1](https://www.e-learn.cn/content/qita/573336#fn:footnote)

## 水平线

输入***/—,按换行键换行

```
***
---
```

------

上下是水平线

------

## 特征元素

### 连接

单击链接，展开后可编辑

ctrl+单击，打开链接。例如：

[https://www.baidu.com](https://www.baidu.com/)

### 超链接

用[]括住要超链接的内容，紧接着用()括住超链接源+名字，超链接源后面+超链接命名

同样ctrl+单击，打开链接。

例如：

```
这是[百度](https://www.baidu.com)官网
```

这是 [百度](https://www.baidu.com/)官网

### 相关链

使用[+超链接文字+]+[+标签+]，创建可定义链接

ctrl+单击，打开链接。

```
这是[百度][id]
[id]:https://www.baidu.com 
```

这是[百度](https://www.baidu.com/)

### URLs

用<>括住url，可手动设置url

对于标准URLs，可自动识别

```
<www.baidu.com>
```



## 图片

1. 手动添加：类似链接格式，前面需加！
2. 用鼠标拖图片进入，然后鼠标放置其上

![wd](http://img.zcool.cn/community/0142135541fe180000019ae9b8cf86.jpg@1280w_1l_2o_100sh.png)

需要把图片传到网上，然后这样才可以给别人看见。https://simimg.com/

## 斜体

以**或__括住

```
*这是斜体字体1*
_这是斜体字体2_
```

*这是斜体字体1*

*这是斜体字体2*

## 加粗

```
开头**，结尾**。
或者开头__,结尾__。

例如：
**这是加粗字体1**
__这是加粗字体2__
```

**这是加粗字体1** 
**这是加粗字体2**

## 删除线

```
开头~~，结尾~~。

例如：
~~这是错误文字~~
```

~~这是错误文字~~

## 下划线

```
使用HTML标签<u>  </u>

例如：
<u>下划线</u>
```

下划线

## 代码

```
用两个`在正常段落总表示代码
例如：
Use the `printf()`function.
```

Use the `printf()`function.

## 数学式

```
输入$，然后按ESC键，之后输入Tex命令，可预览
例如：
$\lim_{x\to\infty}\exp(-x)=0$
```

lim*x*→∞exp(−*x*)=0limx→∞exp⁡(−x)=0

## 下标

```
使用~~括住内容。需要自己在偏好设置里面打开这项功能
例如：
H~2~o
```

H~2~0

## 上标

```
使用^括住内容。需要自己在偏好设置里面打开这项功能
例如：
y^2^=4
```

y^2^=4

## 插入表情

```
以:开始，然后输入表情的英文单词
例如：
:happy
:sad
```

## 高亮

```
==内容==。需要自己在偏好设置里面打开这项功能
例如：
==高亮==
```

# 快捷键

- 无序列表：输入-之后输入空格
- 有序列表：输入数字+“.”之后输入空格
- 任务列表：-[空格]空格 文字
- 标题：ctrl+数字
- 表格：ctrl+t
- 生成目录：[TOC]按回车
- 选中一整行：ctrl+l
- 选中单词：ctrl+d
- 选中相同格式的文字：ctrl+e
- 跳转到文章开头：ctrl+home
- 跳转到文章结尾：ctrl+end
- 搜索：ctrl+f
- 替换：ctrl+h
- 引用：输入>之后输入空格
- 代码块：ctrl+alt+f
- 加粗：ctrl+b
- 倾斜：ctrl+i
- 下划线：ctrl+u
- 删除线：alt+shift+5
- 插入图片：直接拖动到指定位置即可或者ctrl+shift+i
- 插入链接：ctrl+k