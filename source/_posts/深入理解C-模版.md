---
title: 深入理解C++模版
toc: true
date: 2020-11-19 23:24:37
tags: [C++]
categories:
    - 编程语言
    - C++
---

## C++模版的形式

### 函数模版

针对参数类型不同的函数

### 类模版

针对数据成员和成员函数不同的类

### 模板的形参

类型形参、非类型形参和模板形参

#### 类型形参

类型形参由关见字class或typename后接说明符构成，可以定义多个类型形参。

#### 模板形参（Template Template Parameters）

就是将一个模板作为另一个模板的参数。一个template parameter本身也是一个class template。

#### 无类型模版参数

- 模板的非类型形参也就是内置类型的形参，例如下列程序中`int a`就是非类型的模板形参

```cpp
template<class T, int a> class B {
    ...
}
```
- 非类型模板的形参只能是整型，指针和引用
- 调用非类型模板形参的实参必须是一个常量表达式
- 全局变量的地址或引用，全局对象的地址或引用const类型变量是常量表达式，可以用作非类型模板形参的实参

## 参考资料
> - [C++ 模板详解](https://www.runoob.com/w3cnote/c-templates-detail.html)
> - [C++ 函数模板&类模板详解](https://blog.csdn.net/wcc27857285/article/details/84711585)
> - [C++模板](https://www.cnblogs.com/gw811/archive/2012/10/25/2738929.html)
