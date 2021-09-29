---
title: C# 属性（Property）
toc: false
date: 2020-12-14 21:38:22
tags: [C#]
categories:
    - 编程语言
    - C Sharp
---

C#在字段的基础上延伸出了属性的概念。属性定义包含`get`和`set`两个成员用于检索该属性的值以及对其赋值。可以在属性声明的大括号之后通过等号对其进行初始化，适用于不想将属性赋值为系统默认值时的情况。

<!-- more -->

```C#
public class Person
{
    public string FirstName { get; set; } = string.Empty;

    // remaining implementation removed from listing
}
```

属性可以作为`class` `structure` `interface`的命名成员，
