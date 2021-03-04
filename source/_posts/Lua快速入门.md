---
title: Lua 快速入门
toc: true
date: 2020-12-02 09:04:23
tags: [Lua]
categories:
    - 编程语言
    - Lua
---

## Lua 数据类型

Lua是动态类型的语言，不需要给变量声明类型。Lua有8个基本类型：

<!-- more -->

| 数据类型 | 描述                                                         |
| :------- | :----------------------------------------------------------- |
| nil      | 这个最简单，只有值nil属于该类，表示一个无效值（在条件表达式中相当于false）。 |
| boolean  | 包含两个值：false和true。                                    |
| number   | 表示双精度类型的实浮点数                                     |
| string   | 字符串由一对双引号或单引号来表示                             |
| function | 由 C 或 Lua 编写的函数                                       |
| userdata | 表示任意存储在变量中的C数据结构                              |
| thread   | 表示执行的独立线路，用于执行协同程序                         |
| table    | Lua 中的表（table）其实是一个"关联数组"（associative arrays），数组的索引可以是数字、字符串或表类型。在 Lua 里，table 的创建是通过"构造表达式"来完成，最简单构造表达式是{}，用来创建一个空表。 |

## Lua 变量

Lua 变量有三种类型：全局变量、局部变量和表中的域。Lua 变量默认为全局变量，除非用local显式声明为局部变量。局部变量的作用域从声明位置开始到所在语句块结束。变量的默认值都是nil。

## Lua 循环

### for 循环

#### 数值 for 循环

```lua
for var=exp1, exp2, exp3 do
    <...>
end
```

var从exp1变化到exp2，每次以exp3为步长递增var，如果exp3缺省，则默认为1。

#### 泛型 for 循环

使用一个迭代器来遍历所有的值

```lua
a = {"one", "two", "three"}
for i, v in ipairs(a) do
    print(i, v)
end
```

i是数组索引值，v是对应索引的数组元素值，ipairs是Lua提供的迭代器，用来迭代数组。

### while 循环

```lua
while(condition)
do
    statements
end
```

### repeat...until 循环

```lua
repeat
   statements
until( condition )
```

## Lua 流程控制

### if 语句

```lua
if(布尔表达式)
then
   --[ 在布尔表达式为 true 时执行的语句 --]
end
```

### if...else 语句

```lua
if(布尔表达式)
then
   --[ 布尔表达式为 true 时执行该语句块 --]
else
   --[ 布尔表达式为 false 时执行该语句块 --]
end
```

### if...elseif...else 语句

```lua
if( 布尔表达式 1)
then
   --[ 在布尔表达式 1 为 true 时执行该语句块 --]

elseif( 布尔表达式 2)
then
   --[ 在布尔表达式 2 为 true 时执行该语句块 --]

elseif( 布尔表达式 3)
then
   --[ 在布尔表达式 3 为 true 时执行该语句块 --]
else 
   --[ 如果以上布尔表达式都不为 true 则执行该语句块 --]
end
```

## Lua 函数

### 定义的格式

```lua
optional_function_scope function function_name( argument1, argument2, argument3..., argumentn)
    function_body
    return result_params_comma_separated
end
```

- **optional_function_scope:** 该参数是可选的制定函数是全局函数还是局部函数，未设置该参数默认为全局函数，如果你需要设置函数为局部函数需要使用关键字 **local**。
- **function_name:** 指定函数名称。
- **argument1, argument2, argument3..., argumentn:** 函数参数，多个参数以逗号隔开，函数也可以不带参数。
- **function_body:** 函数体，函数中需要执行的代码语句块。
- **result_params_comma_separated:** 函数返回值，Lua语言函数可以返回多个值，每个值以逗号隔开。

### 函数调用

- 一般形式`func(<para>)`，圆括号是必须的
- 当函数只有一个参数，并且此参数为字面字符串或者table构造式的时候，**圆括号可以缺省**
- 当实参少于形参的时候，不足的形参会以nil不足
- 当实参多于形参的时候，多余的实参会被抛弃

### 将函数作为参数传递给函数

```lua
myprint = function(param)
   print("这是打印函数 -   ##",param,"##")
end

function add(num1,num2,functionPrint)
   result = num1 + num2
   -- 调用传递的函数参数
   functionPrint(result)
end
myprint(10)
-- myprint 函数作为参数传递
add(2,5,myprint)
```

### 返回多个结果值。

```lua
function maximum (a)
    local mi = 1             -- 最大值索引
    local m = a[mi]          -- 最大值
    for i,val in ipairs(a) do
       if val > m then
           mi = i
           m = val
       end
    end
    return m, mi
end

print(maximum({8,10,23,12,5}))
```

### 可变参数

```lua
function add(...)  
local s = 0  
  for i, v in ipairs{...} do   --> {...} 表示一个由所有变长参数构成的数组  
    s = s + v  
  end  
  return s  
end  
print(add(3,4,5,6,7))  --->25
```

## Lua 运算符

| 操作符 | 描述 |
| :----- | :--- |
| +      | 加法 |
| -      | 减法 |
| *      | 乘法 |
| /      | 除法 |
| %      | 取余 |
| ^      | 乘幂 |

| 操作符 | 描述                                                         |
| :----- | :----------------------------------------------------------- |
| ==     | 等于，检测两个值是否相等，相等返回 true，否则返回 false      |
| **~=** | **不等于，检测两个值是否相等，不相等返回 true，否则返回 false** |
| >      | 大于，如果左边的值大于右边的值，返回 true，否则返回 false    |
| <      | 小于，如果左边的值大于右边的值，返回 false，否则返回 true    |
| >=     | 大于等于，如果左边的值大于等于右边的值，返回 true，否则返回 false |
| <=     | 小于等于， 如果左边的值小于等于右边的值，返回 true，否则返回 false |

| 操作符 | 描述                                                         |
| :----- | :----------------------------------------------------------- |
| and    | 逻辑与操作符。 若 A 为 false，则返回 A，否则返回 B。         |
| or     | 逻辑或操作符。 若 A 为 true，则返回 A，否则返回 B。          |
| not    | 逻辑非操作符。与逻辑运算结果相反，如果条件为 true，逻辑非为 false。 |

| 操作符 | 描述                               |
| :----- | :--------------------------------- |
| ..     | 连接两个字符串                     |
| #      | 一元运算符，返回字符串或表的长度。 |

### Lua 迭代器

#### 泛型for迭代器

泛型for在自己内部保存迭代函数，实际上它保存三个值：迭代函数，状态常量，控制变量。

```lua
array = {"Google", "Runoob"}

for key,value in ipairs(array)
do
   print(key, value)
end
```

- 首先，计算in之后的表达式的值，表达式会返回for需要的三个值：**迭代函数、状态常量和控制变量**，如果表达式返回的结果个数不足三个会用nil补足，多出的部分会被忽略。
- 第二，**将状态常量和控制变量作为参数调用迭代函数**。
- 第三，**将迭代函数返回的值赋给变量列表**。
- 第四，如果返回的第一个值为nil循环结束，否则执行循环体。
- 第五，回到第二步再次调用迭代函数

#### 无状态的迭代器

无状态的迭代器是指不保留任何状态的迭代器，因此在循环中我们可以利用无状态迭代器避免创建闭包花费额外的代价。

#### 多状态的迭代器

很多情况下，迭代器需要保存多个状态信息而不是简单的状态常量和控制变量，最简单的方法是使用闭包，还有一种方法就是将所有的状态信息封装到 table 内，将 table 作为迭代器的状态常量，因为这种情况下可以将所有的信息存放在 table 内，所以迭代函数通常不需要第二个参数。

## Lua table

Lua table是关联型数组，可以使用nil之外任意类型的值来索引。table可以使用以下操作：

| 序号 | 方法 & 用途                                                  |
| :--- | :----------------------------------------------------------- |
| 1    | table.concat (table [, sep [, start [, end]]]): concat是concatenate(连锁, 连接)的缩写. table.concat()函数列出参数中指定table的数组部分从start位置到end位置的所有元素, 元素间以指定的分隔符(sep)隔开。 |
| 2    | table.insert (table, [pos,] value): 在table的数组部分指定位置(pos)插入值为value的一个元素. pos参数可选, 默认为数组部分末尾. |
| 3    | table.remove (table [, pos]) 返回table数组部分位于pos位置的元素. 其后的元素会被前移. pos参数可选, 默认为table长度, 即从最后一个元素删起。 |
| 4    | table.sort (table [, comp]) 对给定的table进行升序排序。      |

## Lua metatable

```lua
mytable = {}                          -- 普通表
mymetatable = {}                      -- 元表
setmetatable(mytable,mymetatable)     -- 把 mymetatable 设为 mytable 的元表
mytable = setmetatable({},{})         -- 另一种可行的写法
```

## Lua 协程

Lua 协程和线程类似，拥有独立的堆栈，独立的局部变量，独立的指令指针，同时又与其它协同程序共享全局变量和其它大部分东西。线程与协同程序的主要区别在于，一个具有多个线程的程序可以同时运行几个线程，而协同程序却需要彼此协作的运行。在任一指定时刻只有一个协同程序在运行，并且这个正在运行的协同程序只有在明确的被要求挂起的时候才会被挂起。协同程序有点类似同步的多线程，在等待同一个线程锁的几个线程有点类似协同。

## Lua 错误处理

### assert

可以使用两个函数：`assert`和`error`来处理错误：

```lua
local function add(a,b)
   assert(type(a) == "number", "a 不是一个数字")
   assert(type(b) == "number", "b 不是一个数字")
   return a+b
end
add(10)
```

`assert`首先检查第一个参数，如果没问题，就执行下一行语句，如果第一个参数出错，就将第二个参数作为错误信息抛出。

### error

`error`函数终止正在执行的函数，并返回message的内容作为错误信息。

```lua
error (message [, level])
```

- Level=1[默认]：为调用error位置(文件+行号)
- Level=2：指出哪个调用error的函数的函数
- Level=0:不添加错误位置信息

### pcall

函数pcall（protected call）来包装需要执行的代码。pcall接受一个函数和一个需要传递给函数的参数。执行结果如下：

| 结果   | 返回值           |
| ------ | ---------------- |
| 无错误 | true             |
| 有错误 | false, errorinfo |

### xpcall

Lua提供了xpcall函数，xpcall接收第二个参数——一个错误处理函数，当错误发生时，Lua会在调用桟展开（unwind）前调用错误处理函数，于是就可以在这个函数中使用debug库来获取关于错误的额外信息了。debug库提供了两个通用的错误处理函数:

- debug.debug：提供一个Lua提示符，让用户来检查错误的原因
- debug.traceback：根据调用桟来构建一个扩展的错误消息

```lua
function myfunction ()
   n = n/nil
end

function myerrorhandler( err )
   print( "ERROR:", err )
end

status = xpcall( myfunction, myerrorhandler )
print( status)
```



## 参考资料

> - [Lua 教程](https://www.runoob.com/lua/lua-tutorial.html)
> - [Lua中assert( )函数的使用](https://blog.csdn.net/lzj849736336/article/details/51834081)
> - [lua自定义排序函数](https://blog.csdn.net/song527730241/article/details/52126599)
