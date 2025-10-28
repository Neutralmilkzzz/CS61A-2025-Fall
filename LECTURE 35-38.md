# SQL LESSON 35

## 0.intro

> Structured Query Language

SQL是一种**Declarative programming language**

> Declarative language 程序描述期望的结果
>
> Imperative language 是对计算过程的描述

一个例子：

### 如何建立一个数据库？

```sql
create table cities as
	select 38 as latitude, 122 as longitude, "Berkeley" as name union
	select 42,           , 71,             , 
```

以上是标准做法。

`create table`制造一个表

`as`结语词

`select`  A `as` B `union`:union可以增加，但省略掉后面的as，，，

### 如何用一个数据库制造另一个数据库？

```sql
select "west coast" as region, name from cities where longitude >= 115 union
```

## 如何用sql搜索？

```sqlite
SELECT child FROM parents where parent = 'abraham'
```

!!字符串要用单引号

## 如何做计算？

```sqlite
 select word, one + two * 2 + four * 4 + eight * 8 as sums from ints
```

SQL会根据你的需求返回一个新表，但**未必按照你想要的顺序**，这恰恰是**declarative language**的精髓：

> 怎么干老子说了算，保证达到你的要求
>
> btw,注释为--

-----------------

# SQL LESSON 36

> 假设我们有两张表，一张表记录了狗狗之间parent与child的关系，另一张记录了每条狗狗的毛发类型 

```sqlite
-- 建立表 dogs
create table dogs as
select 'abraham'  as name, 'long'  as fur union
select 'barack'             , 'short' union
select 'clinton'            , 'long'  union
select 'delano'             , 'long'  union
select 'eisenhower'         , 'short' union
select 'fillmore'           , 'curly' union
select 'grover'             , 'short' union
select 'herbert'            , 'curly';

-- 建立表 parents
create table parents as
select 'abraham' as parent, 'barack' as child union
select 'abraham'          , 'clinton' union
select 'fillmore'         , 'grover'  union
select 'fillmore'         , 'herbert' union
select 'grover'           , 'delano'  union
select 'clinton'          , 'eisenhower';

```

如何提取一些公共项？

```python
select * from parents, dogs
	where child=name and fur='curly';
```

好了。

## 两个表共用一个列名怎么办？

> Dot Expression and Aliases

select a.xxx and b.xxx from xxx as a and xxx as b

### 🧱 

## 字符串处理

用||可以串联

### 第一步：创建 ands 表

```
CREATE TABLE ands AS
SELECT first.phrase || ' and ' || second.phrase AS phrase
FROM nouns AS first, nouns AS second
WHERE first.phrase <> second.phrase;
```

------

### 🧱 第二步：生成句子

```
SELECT subject.phrase || ' chased ' || object.phrase
FROM ands AS subject, ands AS object
WHERE subject.phrase <> object.phrase;
```

# LESSON 37 Aggregation

非常棒的问题，小明，这正是理解 SQL 聚合函数的核心关键 🔥

------

## 🎯 一句话总结

> 聚合函数的参数是：**一列（column）或一个表达式（expression）**。
>  它告诉数据库要“汇总哪一列的值”或“计算哪种表达式的结果”。

------

## 🧩 二、详细说明

### ✅ 1️⃣ 典型形式

```sql
SELECT MAX(column_name) FROM table_name;
```

也可以是表达式，比如：

```sql
SELECT AVG(weight * 2) FROM animals;
```

👉 聚合函数会**对整列**（或表达式计算后的整列）进行汇总处理。

------

### ✅ 2️⃣ 参数可以是：

| 类型            | 举例                   | 含义                                                         |
| --------------- | ---------------------- | ------------------------------------------------------------ |
| 列名            | `SUM(weight)`          | 把 `weight` 这一列所有行加起来                               |
| 算术表达式      | `AVG(weight * 2)`      | 先算每行的 `weight * 2`，再取平均                            |
| 逻辑表达式      | `SUM(legs = 4)`        | 统计满足 `legs = 4` 的行数（因为 SQLite 中 `TRUE=1, FALSE=0`） |
| DISTINCT 关键字 | `COUNT(DISTINCT kind)` | 去重后再统计                                                 |
| 常量（不推荐）  | `MAX(1)`               | 结果永远是 `1`，没意义                                       |

------

### ✅ 3️⃣ 参数可以带条件（结合 `WHERE`）

```sql
SELECT AVG(weight)
FROM animals
WHERE legs = 4;
```

👉 只计算四条腿动物的平均体重。

------

## 🧮 三、聚合函数常见列表

| 函数          | 参数类型       | 作用         |
| ------------- | -------------- | ------------ |
| `COUNT(expr)` | 任意列或表达式 | 统计非空行数 |
| `SUM(expr)`   | 数值列         | 求和         |
| `AVG(expr)`   | 数值列         | 平均值       |
| `MAX(expr)`   | 任意可比较列   | 最大值       |
| `MIN(expr)`   | 任意可比较列   | 最小值       |

------

## 🧠 四、小结一句话

> 聚合函数不是“行行算”，而是“列列算”。
>  它接受的参数可以是**列名**、**计算表达式**，甚至带上 **DISTINCT 或 WHERE 条件**。

------

要不要我帮你写几条小实验 SQL，
 比如「统计四条腿动物平均体重」「两条腿动物中最重是谁」？
 这些能让你立刻感受到参数不同带来的区别。

## LECTURE 38 

INSERT INTO 

UPDATE

SELECT

CREATE
