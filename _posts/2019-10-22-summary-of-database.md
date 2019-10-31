---
layout: post
title:  数据库总结
date:   2019-10-22 21:23:00 +0800
categories: document
tag: [SQL]
music-id: 481853665
---

> 数据库好多概念性知识……

<!-- more -->

# 书写规范

1. 定义与定理的格式如下：

   【定义】**数据库**：……

   【定理】**数据库定理**：……

   若需要在下面写注释，则用引用的形式：

   > 注：……

2. 列表一律用有序表，而不是无需表，并且每一项若有标题，则标题粗体

3. 凡是有一定”步骤“的解题方法，均使用类 Python 写：

   【方法】：
   
   ```python
   if …… #如果
while …… #循环……
   ```
   
   

# 第四章 关系模型（重要）

## 定义

【定义】**域**：一组相同数据类型的值的集合

【定义】**笛卡尔积**：一组域的乘积，即 $D_1 \times D_2 \times \cdots D_n = \{  \}$





# 第五章 关系模式设计

【定义】**泛模式**：将所有的属性放在一个表中

设计数据库主要的就是设计表的格式，也就是设计关系模式。如果关系模式没设计好，那么可能出现以下问题：

1. **数据冗余**

   比如学生和课程如果放在一张表存，那么每个学生的姓名就会出现多次

2. **更新异常**

   还是学生和课程如果放在一张表存，如果只更新了其中一条记录，那么冗余的记录就没有更新到

3. **插入异常**

   如果学生未选课，那么可能导致学生无法插入

4. **删除异常**

   如果删除某个学生的记录，而那个学生恰好是某门课的最后一个学生，那么可能会将那门课也一起删除。（虽然听起来没什么毛病，如果把课换成班主任就有毛病了）

为了消除这些问题，需要先理解属性之间的“联系”



## 函数依赖（Functional Dependency）

### 定义

【定义】**函数依赖**：设关系模式$R(U, F)$，$U$ 是属性集，$F$ 是函数依赖集。设 $X, Y$ 是 $U$ 的子集，如果对于 $X$ 的每一个值，$Y$ 都有唯一的值与之对应，则 $X$ 决定 $Y$，或 $Y$ 依赖 $X$，记作 $X \rightarrow Y$，称为模式 $R$ 的一个**函数依赖**

> 注：依赖的分类：
>
> 1. 完全函数依赖
> 2. 部分函数依赖
> 3. 

【定义】**逻辑蕴涵**：若从函数依赖集 $F$ 中的函数依赖能推出 $X \rightarrow Y$，则称 $F$ 逻辑蕴涵 $X, Y$，记作 $F |= X \rightarrow Y$

【定义】**函数依赖的闭包**：函数依赖集 $F$ 和 所有被它函数蕴涵的函数依赖 所组成的集合称为 $F$ 的闭包，记作 $F+$

【定义】**属性集的闭包**：设关系模式$R(U, F)$，$U$ 是属性集，$F$ 是函数依赖集。属性集 $X$ 是 $U$ 的子集。用 $F$ 推出所有 $X \rightarrow A$ （$A$ 是属性集），全体 $A$ 的集合称为 $X$ 关于 $A$ 的闭包，记为 $ X+ $

由上面的定义，我们可以重写候选键的定义：设关系模式$R(U, F)$，$U$ 是属性集，$F$ 是函数依赖集。设 $K$ 是 $U$ 的子集。若 $K \rightarrow U \in F+$ ，且不存在 $K$ 的子集 $K’$ ，使 $K' \rightarrow U \in F+$ 成立，则 $K$ 是 $R$ 的一个候选键。

【方法】下面解释求属性集 $X$ 的闭包 $X+$ 的过程（有点类似于 FBS）：

```python
# X = [x1, x2, x3 ……]
#最开始时设置一个A和B
A = []
B = X
while A!=B:
    A=B
    for i in F: #F为函数依赖集
        #对于 F 中函数依赖 i：X->Y
        if i.X in B:
            B.append(i.Y)
```




### 定理

设关系模式$R(U, F)$，$U$ 是属性集，$F$ 是函数依赖集。设 $W, X, Y, Z$ 是 $U$ 的子集，则有以下定理成立：

【定理】**Armstrong 推理规则**

1. 自反率：若$Y \subseteq X$，则 $X \rightarrow Y$ 
2. 增广率：若 $X \rightarrow Y$，则 $XZ \rightarrow YZ$ 
3. 传递率：若 $X \rightarrow Y, Y \rightarrow Z$，则 $X \rightarrow Z$
4. 合并率：若 $X \rightarrow Y, X \rightarrow Z$，则 $X \rightarrow YZ$
5. 伪传递率：若 $X \rightarrow Y, WY \rightarrow Z$，则 $WX \rightarrow Z$
6. 分解率：若 $X \rightarrow Y, Z \subseteq Y$，则 $X \rightarrow Z$ 
7. 伪增高率：若 $X \rightarrow Y, Z \subseteq W$，则 $XW \rightarrow YZ$

 
<<<<<<< HEAD

## 最小函数依赖集

【定义】**等价（覆盖）**：若对于函数依赖 $F, G$，有 $F \subseteq G+, G \subseteq F+, F+ = G+$，则 $F$ 和 $G$ **等价（覆盖）**

【定义】**最小函数依赖集**：满足以下条件的函数依赖集称为**最小函数依赖集**：
1. 每个函数依赖，其右部均为单个属性；（右边无冗余）
2. 左部不可拆分；（左边无冗余）
3. 删去一个函数依赖后的依赖集不等价于原依赖集；（整个都不冗余）

【方法】下面是求最小函数依赖集的过程：

```python
for i in F: 
    #对于 F 中函数依赖 i：X->Y
    F = F-i;
    F = F+seperate(i.Y)#将右边拆成单个

for i in F:
    if F == F-i:
        F = F-i;#检查每个依赖是否冗余

for i in F:
    #删除 i.X 中的部分属性，看 F 是否还等价于原 F
    #是，则删除 i.X 中的冗余部分
```



## 分解

### 无损连接分解（数据完整性）

【定义】**无损连接分解**：若分解后的关系能通过自然连接得到原关系，则称这种分解为为**无损关系分解**

【定理】**测试定理**：对于一分为二得分解，其为无损连接的充要条件是：

【方法】判断某个分解是不是无损连接分解：

```python
#第一步：列表，每一列为每个属性，每一行为每一分解关系
#第二步：若对于第 i 行第 j 列，若属性 j 在关系 i 中，则填入“ai”,否则填入“bij”
#第三步：
while 表不可再修改:
    for 每个函数依赖X->Y in 函数依赖集:
        if 有一行的 X,Y 列上都是 a:
            将其它有 X 的行对应得 Y 列改为 a
        else:
            将有 X 的行上的 Y 列的 bij 改为最这些 bij 中最小的 bij
```



### 保持函数依赖分解（语义完整性）



## 范式

将分解的程度分为：

1. 第一范式（1st Normal Form, 1NF）：最基本的范式，关系中的每个属性都是不可再分的最小数据单位
2. 第二范式：满足 1NF，且非主属性都完全函数依赖于某个候选关键字（候选键）
3. 第三范式：满足 2NF，且非主属性都非传递依赖于某个候选关键字（候选键）
4. BC范式（Boyce-Codd Normal Form，BCNF）：满足 1NF，且所有函数依赖 $X \rightarrow Y$ 中的 $X$ 都是候选键



【方法】已知关系模式 $R(U, F)$，$U$ 是属性集，$F$ 是函数依赖集。将 $R$ 分解为第三范式：

```python
#若 F 中只有一个依赖：X->A，且满足 XA=R，则 R 已是第三范式
#对每一个 X->A，都分解成一个关系模式{X, A}
#若有多个 X->A1, X->A2, X->A3，将其合并为一个关系模式{X, A1, A2, A3}
#若分解后，没有一个关系模式包含 R 的所有关键字，则创建一个包含 R 的所有关键字的关系模式
#分解结束
```



【方法】已知关系模式 $R(U, F)$，$U$ 是属性集，$F$ 是函数依赖集。将 $R$ 分解为 BCNF：

```python
#假设初始的分解 p={R}
#若 p 中的所有关系模式都是 BCNF，则分解结束，否则进行如下分解：
#对于 p 中非 BCNF 的关系模式 S，S 中必包含一个函数依赖 X->A，满足 X 不是 S 的键，且 A 不属于 X。设 S1 = XA，S2=S-A，用 S1,S2 代替 S
#循环上一步骤直到 p 中的所有关系模式都是 BCNF
#分解结束
```

=======
>>>>>>> db822d2b04c41fcabd1aac8538b6052548fb51d7