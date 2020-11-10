---
title: 关系数据库设计
tag: Database
abstract: 数据库第7、8章复习提纲
---

## 数据库设计与E-R模型

### 实体-联系模型

#### 实体集&联系集

实体-联系（entity-relationship，E-R）。实体表示现实世界中可区别于所有其他对象的的一个“事物”或“对象”。每个实体有一组性质，其中一些性质的值可以唯一的标示一个实体。实体集（entity set）是具有相同类型的一个实体集合。外延（extension）指属于实体集的实体的实际的集合。实体通过一组属性（attribution）来表示，每个属性有一个值（value）。

联系（relationship）指多个实体间的相互关联。联系集（relationship set）是相同类型联系的集合。实体集之间的关联称为参与（participation）。

#### 属性

每个属性都有一个可以取值的范围称为该属性的域（domain）。属性可以如下划分：

- 简单（simple）和复合（composite）属性
- 单值（single-valued）和多值（multiple-valued）属性
- 派生（derived）属性

### 约束

#### 映射基数（mapping cardinality）

对于二元关系，映射基数必然是以下情况

- 一对一（one-to-one）
- 一对多（one-to-many）
- 多对一（many-to-one）
- 多对多（many-to-many）

#### 参与约束

实体集（E），联系集（R）。E中的每个实体都参与到R的至少一个联系中，E到R的参与称为全部（total）。如果只有部分参与，称为部分（partial）。

#### 码

R是一个涉及实体集$E_1, E_2, ..., E_n$的联系集。$primary-key(E_i)$代表$E_i$的主码。R的主码构成依赖于同R关联的属性的集合。

如果R没有属性与之关联
$$
primary-key(E_1) \cup primary-key(E_2) \cup ... \cup primary-key(E_n)
$$
描述了R中的一个联系

如果R有属性$a_1, a_2, ..., a_m$与之关联
$$
primary-key(E_1) \cup primary-key(E_2) \cup ... \cup primary-key(E_n) \cup \{a_1, a_2, ..., a_m\}
$$
描述了R中的一个联系

在上述关系中
$$
primary-key(E_1) \cup primary-key(E_2) \cup ... \cup primary-key(E_n)
$$
是R的一个超码

### 从实体集中删除冗余属性

当一个属性A在两个实体集中都出现时，若A是$E_1$的主码，而在$E_2$中出现，但不构成主码。那么A应该从$E_2$中移除。

### 实体联系图

#### 基本结构

- 分成两部分的矩形代表实体集
- 棱形代表联系集
- 未分割的矩形代表联系集的属性，主码带下划线
- 线段将实体集连接到联系集
- 虚线将联系集属性连接到联系集
- 双线表示一个实体集全部的参与到联系集中
- 双菱形代表连接到弱实体集的标志性联系集

#### 映射基数

- 一对一：两侧各画一个箭头
- 一对多：画箭头到一的实体集，线段到多的实体集
- 多对一：同理
- 多对多：两侧都画线段

实体集和二元联系集之间的一条边可以有一个关联最大和最小的映射基数，用l..h表示。最大值为*代表没有限制。

#### 复杂属性

- 缩进表示复合属性
- "{example-attribute}"表示多值属性
- "example-attribute"表示派生属性

#### 角色

在矩形与棱形之间的连线上进行标注来表示角色

#### 非二元的实体联系集

在非二元的联系集中，我们最多只允许一个箭头。

#### 弱实体集

没有足够的属性构成主码的实体集是弱实体集（weak entity set）。弱实体集必须与另一个属主实体集（owner entity set）关联才有意义。这个关系称为存在依赖（existence dependent）。该联系称为标示性联系（identify relationship）。标示性联系是弱实体集到标示实体集的多对一的关系，而且弱实体集的参与是全部的，标示性联系集不应该有任何描述性属性。弱实体集的主码由标示性实体集的主码加上分辨符组成。

- 分辨符用虚下划线表示
- 联系集用双菱形表示

## 关系数据库设计

### 原子域与第一范式

一个域是原子的（atomic），如果该域的元素被认为是不可分的单元。R是第一范式（First Normal Form， 1NF），如果R中所有属性的域都是原子的。

### 使用函数依赖进行分解

- 使用希腊字母表示属性集（e.g. $\alpha$）。小写罗马字母后跟用圆括号括住的大写字母来表示关系模式（e.g. $r(R)$）
- 属性集是超码时用，用$K$表示
- 对关系使用小写名称

#### 码和函数依赖

一个关系的满足现实世界约束的实例，成为关系的合法实例（legal instance）。

令$r(R)$是一个关系模式。在关系$r(R)$的任何合法实例中，对于$r$的实例中的所有元组对$t_1$和$t_2$总满足，若$t_1 \neq t_2$，则$t_1[K] \neq t_2[K]$。

有$r(R)$，$\alpha \subseteq R$且$\beta \subseteq R$

- 给定一个实例，满足函数依赖$\alpha \rightarrow \beta$的条件是：对于所有元组对$t_1$和$t_2$，若$t_1[\alpha]=t_2[\alpha]$，则$t_1[\beta]=t_2[\beta]$
- 若所有合法实例都满足函数依赖$\alpha \rightarrow \beta$，该函数依赖在$r(R)$上成立

有以下两种方式使用函数依赖

- 判定关系的实例是否满足给定函数一俩集$F$
- 说明合法关系上的约束

如果函数依赖在所有的关系中都满足，那么就称该函数关系是平凡（trival）的。给定关系$r(R)$上成立的函数依赖集$F$。使用$F^+$表示$F$集合的闭包（closure）。

#### Boyce-Codd Normal Form

具有函数依赖集$F$的关系模式$R$属于BCNF的条件是，对$F^+$中所有的函数依赖$\alpha \rightarrow \beta$

- 函数依赖$\alpha \rightarrow \beta$是平凡的
- $\alpha$是$R$的一个超码

设$R$为不属于BCNF的一个模式，存在非平凡的函数依赖$\alpha \rightarrow \beta$。用以下两个模式取代$R$

- $(\alpha \cup \beta)$
- $(R - (\beta - \alpha))$

#### 第三范式

具有函数依赖集$F$的关系模式$R$属于3NF的条件是，对$F^+$中所有的函数依赖$\alpha \rightarrow \beta$

- $\alpha \rightarrow \beta$是一个平凡的函数依赖
- $\alpha$是$R$的一个超码
- $\beta - \alpha$中每个属性$A$都包含在$R$的一个候选码中

### 函数依赖理论

#### 函数依赖的闭包

如果$R$中每一个满足$F$的实例也满足$f$，则$R$上的函数依赖$f$被$r$上的函数依赖集$F$逻辑蕴含（logically imply）。用大写罗马字母表示单个属性，$\alpha\beta$表示$\alpha \cup \beta$。使用**Armstrong's axiom**寻找逻辑蕴含的函数依赖。

- 自反律（reflexivity rule）。若$\beta \subseteq \alpha$，则$\alpha \rightarrow \beta$
- 增补律（augmentation rule）。若$\alpha \rightarrow \beta$，则$\alpha\gamma \rightarrow \beta\gamma$
- 传递律（transitivity rule）。若$\alpha \rightarrow \beta$且$\beta \rightarrow \gamma$，则$\alpha \rightarrow \gamma$

通过**Armstrong's axiom**可以推出

- 合并律（union rule）。若$\alpha \rightarrow \beta$且$\alpha \rightarrow \gamma$，则$\alpha \rightarrow \beta\gamma$
- 分解率（decomposition rule），若$\alpha \rightarrow \beta\gamma$，则$\alpha \rightarrow \beta$且$\alpha \rightarrow \gamma$
- 伪传递律（pseudotransitivity rule），$\alpha \rightarrow \beta$且$\beta\lambda \rightarrow \gamma$，则$\alpha\lambda \rightarrow \gamma$

#### 属性的闭包

如果$\alpha \rightarrow B$，称属性$B$被$\alpha$函数确定（functionally determine）。
