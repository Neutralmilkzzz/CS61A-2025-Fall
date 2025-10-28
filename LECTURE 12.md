# LECTURE 12

## The closure property of Data types(闭包)

* Closure is powerful because it permits us to create hierarchical structures

----

## Box-and-Pointer Notation在环境图中

![image-20250909192211196](C:\Users\ZHAOKAI\AppData\Roaming\Typora\typora-user-images\image-20250909192211196.png)

-----------

## Slicing

一种可以对list and range执行的操作

```python

>>> [odds[i] for i in range(1, 3)]
[5, 7]
>>> odds[1:3]
[5, 7]
>>> odds[:3]
[3, 5, 7]
>>> odds[:]
[3, 5, 7, 9, 11]
>>> 
```

![image-20250909192920319](C:\Users\ZHAOKAI\AppData\Roaming\Typora\typora-user-images\image-20250909192920319.png)

------

## Processing Container Values

有一些内置函数可以帮助我们。

SUM：

```python
list + list = list
sum(list, list, list(list, list))
```

MAX:

``` python
max(range(5))
max(0, 1, 2, 3, 4)
max(range(10), key = lambda x: 7 - (x-4)*(x-2))
```

ALL:

```python
[x < 5 for x in range(5)]
all(range(5))
False
```

----

Strings

```\n```是用来表示换行的

在使用in的时候是可以连续查询的（here in where）

-----------

 # 字典（dictionary）

Dictionaries are collections of key-value pairs

字典推导式：

```python
{<key exp>: <value exp> for <name> in <iter exp> if <filter exp>}
```

e.g:

```python
{ x * x: x for x in [1, 2, 3, 4, 5] if x > 2}
###等同于
{9: 3, 16: 4, 25: 5}
```

```python
def index(keys, values, match):
    return {k:[v for v in values if match(k, v)] for k in keys}
```

-----

做题：

把一个数组替换成另一个数组的形状：

yigeshuzu[:] = lingyigeshuzu!!!!!

(源自leetcode)

--------------------

```python
for t in total:
            unique[tuple(t)] = True
        total = list(unique.keys())
        return total
```

看起来很牛逼，虽然我还没h

## 字典推导式的练习：

```python
def divide(quotients, divisors):
    """Return a dictonary in which each quotient q is a key for the list of
    divisors that it divides evenly.

    >>> divide([3, 4, 5], [8, 9, 10, 11, 12])
    {3: [9, 12], 4: [8, 12], 5: [10]}
    >>> divide(range(1, 5), range(20, 25))
    {1: [20, 21, 22, 23, 24], 2: [20, 22, 24], 3: [21, 24], 4: [20, 24]}
    """
    return {q:[d for d in divisors if d % q == 0] for q in quotients}
```

就是 a:b for a in A
