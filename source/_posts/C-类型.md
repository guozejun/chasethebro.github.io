---
title: C# 类型
toc: true
date: 2020-11-28 03:12:23
tags: [C#]
categories:
    - 编程语言
    - C Sharp
---

## 值类型

```C#
using System;

public struct MutablePoint
{
    public int X;
    public int Y;

    public MutablePoint(int x, int y) => (X, Y) = (x, y);

    public override string ToString() => $"({X}, {Y})";
}

public class Program
{
    public static void Main()
    {
        var p1 = new MutablePoint(1, 2);
        var p2 = p1;
        p2.Y = 200;
        Console.WriteLine($"{nameof(p1)} after {nameof(p2)} is modified: {p1}");
        Console.WriteLine($"{nameof(p2)}: {p2}");

        MutateAndDisplay(p2);
        Console.WriteLine($"{nameof(p2)} after passing to a method: {p2}");
    }

    private static void MutateAndDisplay(MutablePoint p)
    {
        p.X = 100;
        Console.WriteLine($"Point mutated in a method: {p}");
    }
}
// Expected output:
// p1 after p2 is modified: (1, 2)
// p2: (1, 200)
// Point mutated in a method: (100, 200)
// p2 after passing to a method: (1, 200)
```

如上述代码所示，对值类型的修改只会作用到变量本身，对给他赋值的右值变量没有影响。

<!-- more -->

### 整型数值类型

整形数值类型支持算数、位逻辑、比较和相等运算符，可以用文本进行初始化。包含有以下类型

| C# 类型 | 大小 | .NET 类型 |
| - | - | - |
| sbyte | 8bit | System.SByte |
| byte | 8bit(u) | System.Byte |
| short | 16bit | System.Int16 |
| ushort | 16bit(u) | System.Uint16 |
| int | 32bit | System.Int32 |
| uint | 32bit(u) | System.UInt32 |
| long | 64bit | System.Int64 |
| ulong | 64bit(u) | System.UInt64 |

C# 类型关键字可以和.NET类型互换，前者是后者的别名，每个整形都会被系统初始化为0.每个整形都有`MinValue`和`MaxValue`常量，提供该类型的最大值和最小值。`System.Numerics.BigInteger`用于表示没有上限或下限的带符号整数。

```C#
int a = 123;
System.Int32 b = 123;
```

#### 整数文本

- 十进制：没有前缀
- 十六进制：`0x` `0X`前缀
- 二进制：`0b` `0B`前缀

```c#
var decimalLiteral = 42;
var hexLiteral = 0x2A;
var binaryLiteral = 0b_0010_1010;
```

`_`可以作为数字分隔符用于所有类型的数字文本。整数文本的类型由其后缀决定：

- 如果没有后缀，从下列类型中选取第一个可以表示其类型的类型：`int` `uint` `long` `ulong`
- 如果文本以`U` `u`结尾，`uint` `ulong`
- 文本以`L` `l`结尾，`long` `ulong`

如果整数字面量超过了`UInt64.MaxValue`出现编译器错误。

### 浮点数值类型

浮点数值类型支持算数、位逻辑、比较和相等运算符，可以用文本进行初始化。

| C# 类型   | 精度  | 大小   | .NET 类型      |
| --------- | ----- | ------ | -------------- |
| `float`   | 6-9   | 4byte  | System.Single  |
| `double`  | 15-17 | 8byte  | System.Double  |
| `decimal` | 28-29 | 16byte | System.Decimal |

浮点类型关键字可以和.NET类型互换，默认初始化值为0。浮点类型有`MinValue`和`MaxValue`常量。`float`和`double`提供非数字和无穷大的常量，`NaN` `NegativeInfinity` `PositiveInfinity`。与 `float` 和 `double` 相比，`decimal` 类型具有更高的精度和更小的范围，因此它适合于财务和货币计算。

可在表达式中将整形类型与 `float` 和 `double` 类型混合使用。 在这种情况下，整型类型隐式转换为其中一种浮点类型，`float` 类型可能隐式转换为 `double`。 此表达式的计算方式如下：

- 如果表达式中有 `double` 类型，则表达式在关系比较和相等比较中求值得到 `double` 或 `bool`。
- 如果表达式中没有 `double` 类型，则表达式在关系比较和相等比较中求值得到 `float` 或 `bool`。

你还可在表达式中混合使用整型类型和 `decimal` 类型。 在这种情况下，整型类型隐式转换为 `decimal` 类型，并且表达式在关系比较和相等比较中求值得到 `decimal` 或 `bool`。

不能在表达式中将 `decimal` 类型与 `float` 和 `double` 类型混合使用。 在这种情况下，如果你想要执行算术运算、比较运算或相等运算，则必须将操作数显式转换为 `decimal` 或反向转换，如下例所示：

```c#
double a = 1.0;
decimal b = 2.1m;
Console.WriteLine(a + (double)b);
Console.WriteLine((decimal)a + b);
```

#### 实数文本

实数文本的类型由后缀决定：

- 不带后缀的文本或带有 `d` 或 `D` 后缀的文本的类型为 `double`
- 带有 `f` 或 `F` 后缀的文本的类型为 `float`
- 带有 `m` 或 `M` 后缀的文本的类型为 `decimal`

还可以使用科学计数法表示：

```c#
double d = 0.42e2;
Console.WriteLine(d);  // output 42

float f = 134.45E-2f;
Console.WriteLine(f);  // output: 1.3445

decimal m = 1.5E6m;
Console.WriteLine(m);  // output: 1500000
```

浮点数值类型之间只有一种隐式转换：从 `float` 到 `double`。 但是，可以使用显式强制转换将任何浮点类型转换为任何其他浮点类型。

### 内置数值转换

#### 隐式类型转换

| From   | To                                                           |
| ------ | ------------------------------------------------------------ |
| sbyte  | `short`、`int`、`long`、`float`、`double` 或 `decimal`       |
| byte   | `short`、`ushort`、`int`、`uint`、`long`、`ulong`、`float`、`double` 或 `decimal` |
| short  | `int`、`long`、`float`、`double` 或 `decimal`                |
| ushort | `int`、`uint`、`long`、`ulong`、`float`、`double` 或 `decimal`。 |
| int    | `long`、`float`、`double` 或 `decimal`                       |
| uint   | `long`、`ulong`、`float`、`double` 或 `decimal`              |
| long   | `float`、`double` 或 `decimal`                               |
| ulong  | `float`、`double` 或 `decimal`                               |
| float  | `double`                                                     |

- 任何[整型数值类型](https://docs.microsoft.com/zh-cn/dotnet/C#/language-reference/builtin-types/integral-numeric-types)都可以隐式转换为任何[浮点数值类型](https://docs.microsoft.com/zh-cn/dotnet/C#/language-reference/builtin-types/floating-point-numeric-types)。
- 不存在针对 `byte` 和 `sbyte` 类型的隐式转换。 不存在从 `double` 和 `decimal` 类型的隐式转换。
- `decimal` 类型和 `float` 或 `double` 类型之间不存在隐式转换。
- 类型 `int` 的常量表达式的值（例如，由整数文本所表示的值）如果在目标类型的范围内，则可隐式转换为 `sbyte`、`byte`、`short`、`ushort`、`uint` 或 `ulong`。

#### 显式类型转换

| From    | To                                                           |
| ------- | ------------------------------------------------------------ |
| sbyte   | `byte`、`ushort`、`uint` 或 `ulong`                          |
| byte    | `sbyte`                                                      |
| short   | `sbyte`、`byte`、`ushort`、`uint` 或 `ulong`                 |
| ushort  | `sbyte`、`byte` 或 `short`                                   |
| int     | `sbyte`、`byte`、`short`、`ushort`、`uint` 或 `ulong`        |
| uint    | `sbyte`、`byte`、`short`、`ushort` 或 `int`                  |
| long    | `sbyte`、`byte`、`short`、`ushort`、`int`、`uint` 或 `ulong`。 |
| ulong   | `sbyte`、`byte`、`short`、`ushort`、`int`、`uint` 或 `long`。 |
| float   | `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong` 或 `decimal` |
| double  | `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`float` 或 `decimal` |
| decimal | `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`float` 或 `double` |

- 整形之间的转换取决于 checking context。如果在checked context中，值不越界才能转换成功，否则就会抛出OverflowException。如果在unchecked context，转换总是会成功，执行结果如下：
  - 如果源类型大于目标类型，会舍弃最高有效位。
  - 如果源类型小于目标类型，会被sign-extended或者zero-extended，有符号数被sign-extended，无符号数被zero-extended。
  - 如果大小相同会被直接转换
- 如果`decimal`类型转换为整形，首先会去掉小数部分，如果结果比目标类型的范围大，抛出OverflowException。
- 如果`float`和`double`类型向整形转换，那么在去掉小数部分之后，需要查看checking context，看情况是否抛出OverflowException。
- 将 `double` 转换为 `float`，`doube`值会被舍入为最接近的`float`值，如果`double`值太大或者太小，结果是0或者无穷大。
- `float`或者`double`转换为`decimal`的时候，源值转换为 `decimal` 表示形式，并并五入到第 28 位小数后最接近的数（如果需要）。 根据源值的值，可能出现以下结果之一：
  - 如果源值太小，无法表示为 `decimal`，结果则为零。
  - 如果源值为 NaN（非数值）、无穷大或太大而无法表示为 `decimal`，则引发 OverflowException。

### char

`char` 类型关键字是 .NET System.Char 结构类型的别名，它表示 Unicode UTF-16 字符。

| 类型   | 范围             | 大小  | .NET 类型   |
| ----- | --------------- | ---- | ---------- |
| `char` | U+0000 到 U+FFFF | 16 位 | System.Char |

`char` 类型的默认值为 `\0`，即 U+0000。

`char` 类型支持比较、相等、增量和减量运算符。 此外，对于 `char` 操作数，算数和逻辑位运算符对相应的字符代码执行操作，并得出 `int` 类型的结果。

字符串类型将文本表示为 `char` 值的序列。

#### 文本

可以使用以下命令指定 `char` 值：

- 字符文本。
- Unicode 转义序列，它是 `\u` 后跟字符代码的十六进制表示形式（四个符号）。
- 十六进制转义序列，它是 `\x` 后跟字符代码的十六进制表示形式。

```c#
var chars = new[]
{
    'j',
    '\u006A',
    '\x006A',
    (char)106,
};
Console.WriteLine(string.Join(" ", chars));  // output: j j j j
```

> 对于 Unicode 转义序列，必须指定全部四位十六进制值。 也就是说，`\u006A` 是一个有效的转义序列，而 `\u06A` 和 `\u6A` 是无效的。
>
> 对于十六进制转义序列，可以省略前导零。 也就是说，`\x006A`、`\x06A` 和 `\x6A` 转义序列是有效的，并且对应于同一个字符。

#### 转换

`char` 类型可隐式转换为以下整型类型：`ushort`、`int`、`uint`、`long` 和 `ulong`。 它也可以隐式转换为内置浮点数值类型：`float`、`double` 和 `decimal`。 它可以显式转换为 `sbyte`、`byte` 和 `short` 整型类型。

无法将其他类型隐式转换为 `char` 类型。 但是，任何整型或浮点数值类型都可显式转换为 `char`。

### 枚举类型

枚举类型 是由基础整型数值类型的一组命名常量定义的值类型。 若要定义枚举类型，使用 `enum` 关键字并指定枚举成员 的名称：

```c#
enum Season
{
    Spring,
    Summer,
    Autumn,
    Winter
}
```

默认情况下，枚举成员的关联常数值为类型 `int`；它们从零开始，并按定义文本顺序递增 1。 可以显式指定任何其他整数数值类型作为枚举类型的基础类型。 还可以显式指定关联的常数值，如下面的示例所示：

```c#
enum ErrorCode : ushort
{
    None = 0,
    Unknown = 1,
    ConnectionLost = 100,
    OutlierReading = 200
}
```

### 结构类型

结构类型（“structure type”或“struct type”）是一种可封装数据和相关功能的值类型 。 使用 `struct` 关键字定义结构类型：

```c#
public struct Coords
{
    public Coords(double x, double y)
    {
        X = x;
        Y = y;
    }

    public double X { get; }
    public double Y { get; }

    public override string ToString() => $"({X}, {Y})";
}
```

#### `readonly`结构

从 C# 7.2 开始，可以使用 `readonly` 修饰符来声明结构类型不可变：

```c#
public readonly struct Coords
{
    public Coords(double x, double y)
    {
        X = x;
        Y = y;
    }

    public double X { get; init; }
    public double Y { get; init; }

    public override string ToString() => $"({X}, {Y})";
}
```

#### `readonly`实例成员

从 C#8.0 开始，还可以使用 `readonly` 修饰符声明实例成员不会修改结构的状态。 如果不能将整个结构类型声明为 `readonly`，可使用 `readonly` 修饰符标记不会修改结构状态的实例成员。

在 `readonly` 实例成员内，不能分配到结构的实例字段。 但是，`readonly` 成员可以调用非 `readonly` 成员。 在这种情况下，编译器将创建结构实例的副本，并调用该副本上的非 `readonly` 成员。 因此，不会修改原始结构实例。

通常，将 `readonly` 修饰符应用于以下类型的实例成员：

- 方法：

  ```c#
  public readonly double Sum()
  {
      return X + Y;
  }
  ```

- 属性和索引器

  ```c#
  private int counter;
  public int Counter
  {
      readonly get => counter;
      set => counter = value;
  }
  ```

### 元组类型

元组功能在 C# 7.0 及更高版本中可用，它提供了简洁的语法，用于将多个数据元素分组成一个轻型数据结构。 下面的示例演示了如何声明元组变量、对它进行初始化并访问其数据成员：

```c#
(double, int) t1 = (4.5, 3);
Console.WriteLine($"Tuple with elements {t1.Item1} and {t1.Item2}.");
// Output:
// Tuple with elements 4.5 and 3.

(double Sum, int Count) t2 = (4.5, 3);
Console.WriteLine($"Sum of {t2.Count} elements is {t2.Sum}.");
// Output:
// Sum of 3 elements is 4.5.
```

#### 元组赋值与析构

C# 支持满足以下两个条件的元组类型之间的赋值：

- 两个元组类型有相同数量的元素
- 对于每个元组位置，右侧元组元素的类型与左侧相应的元组元素的类型相同或可以隐式转换为左侧相应的元组元素的类型

还可以使用赋值运算符 `=` 在单独的变量中析构元组实例。 为此，可以使用以下方式之一进行操作：

- 在括号内显式声明每个变量的类型：

  ```C#
  var t = ("post office", 3.6);
  (string destination, double distance) = t;
  Console.WriteLine($"Distance to {destination} is {distance} kilometers.");
  // Output:
  // Distance to post office is 3.6 kilometers.
  ```

- 在括号外使用 `var` 关键字来声明隐式类型化变量，并让编译器推断其类型：

  ```C#
  var t = ("post office", 3.6);
  var (destination, distance) = t;
  Console.WriteLine($"Distance to {destination} is {distance} kilometers.");
  // Output:
  // Distance to post office is 3.6 kilometers.
  ```

- 使用现有变量：

  ```C#
  var destination = string.Empty;
  var distance = 0.0;
  
  var t = ("post office", 3.6);
  (destination, distance) = t;
  Console.WriteLine($"Distance to {destination} is {distance} kilometers.");
  // Output:
  // Distance to post office is 3.6 kilometers.
  ```

#### 元组相等

从 C# 7.3 开始，元组类型支持 `==` 和 `!=` 运算符。 这些运算符按照元组元素的顺序将左侧操作数的成员与相应的右侧操作数的成员进行比较。

```C#
(int a, byte b) left = (5, 10);
(long a, int b) right = (5, 10);
Console.WriteLine(left == right);  // output: True
Console.WriteLine(left != right);  // output: False

var t1 = (A: 5, B: 10);
var t2 = (B: 5, A: 10);
Console.WriteLine(t1 == t2);  // output: True
Console.WriteLine(t1 != t2);  // output: False
```

同时满足以下两个条件时，两个元组可比较：

- 两个元组具有相同数量的元素。 例如，如果 `t1` 和 `t2` 具有不同数目的元素，`t1 != t2` 则不会进行编译。
- 对于每个元组位置，可以使用 `==` 和 `!=` 运算符对左右侧元组操作数中的相应元素进行比较。 例如，`(1, (2, 3)) == ((1, 2), 3)` 不会进行编译，因为 `1` 不可与 `(1, 2)` 比较。

`==` 和 `!=` 运算符将以短路方式对元组进行比较。 也就是说，一旦遇见一对不相等的元素或达到元组的末尾，操作将立即停止。 但是，在进行任何比较之前，将对所有元组元素进行计算，如以下示例所示：

```C#
Console.WriteLine((Display(1), Display(2)) == (Display(3), Display(4)));

int Display(int s)
{
    Console.WriteLine(s);
    return s;
}
// Output:
// 1
// 2
// 3
// 4
// False
```

### 可以为null的值类型

可为 null 值类型 `T?` 表示其基础值类型 `T` 的所有值及额外的 null 值。 例如，可以将以下三个值中的任意一个指定给 `bool?` 变量：`true`、`false` 或 `null`。 基础值类型 `T` 本身不能是可为空的值类型。

## 引用类型

内置的引用类型有**object**、**dynamic**和**string**。**Object**类型是C#通用类型系统中所有数据类型的终极基类。Object是System.Object类的别名。Object类型可以在类型转换之后被分配任何其他类型的值（编译时检查）。任何类型的值都可以存储在动态数据类型变量中。这些变量的类型检查在运行时发生。String类型可以被分配任何字符串的值。String是System.String类的别名。

### 内置引用类型

#### object类型

`object`类型是System.Object的别名，所有类型都直接或者间接的从System.Object类型派生。值类型转换为object类型的过程被称为装箱，object类型转换为值类型的过程被称为取消装箱。

#### string类型

string类型是Unicode字符的集合，是System.String的别名。使用==和!=比较Unicode集合的值是否相等，使用Object.ReferenceEquals()比较引用值本身是否相等。逐字字符串以@开头，不处理转义序列。若要在 @-quoted 字符串中包含双引号，双倍添加即可：

```C#
@"""Ahoy!"" cried the captain." // "Ahoy!" cried the captain.
```

#### delegate类型

delegate类型的声明和method signature相似。有一个返回值和任意数目的参数列表。delegate用于封装命名方法或者匿名方法的引用类型，类似于函数指针。委托是类型安全的。

#### dynamic类型

dynamic 类型表示变量的使用和对其成员的引用绕过编译时类型检查。 改为在运行时解析这些操作。 dynamic 类型简化了对 COM API（例如 Office Automation API）、动态 API（例如 IronPython 库）和 HTML 文档对象模型 (DOM) 的访问。

### class

C#只允许单一继承，但是一个类可以实现多个接口。

### interface

从C# 8.0开始，接口可为成员定义默认实现。 它还可以定义static成员，以便提供常见功能的单个实现。

### 可为null的引用类型

在可为 null 的感知上下文中：
- 引用类型 T 的变量必须用非 null 值进行初始化，并且不能为其分配可能为 null 的值。
- 引用类型 T? 的变量可以用 null 进行初始化，也可以分配 null，但在取消引用之前必须对照 null 进行检查。
- 类型为 T? 的变量 m 在应用 null 包容运算符时被认为是非空的，如 m! 中所示。

## 参考资料

> - [C#教程 菜鸟教程](https://www.runoob.com/C#/)
> - [C#入门经典](https://shenjun4C#.github.io/C#html/)
> - [C#指南 语言参考](https://docs.microsoft.com/zh-cn/dotnet/C#/language-reference/builtin-types/floating-point-numeric-types)
