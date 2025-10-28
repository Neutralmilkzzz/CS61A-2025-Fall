> 2025.9.4

## Iteration（迭代）

### Fibonacci Sequance

```python
def fib(n):
    pred, curr = 0, 1
    k = 1
    while k < n:
        pred, curr = curr, pred + curr
        k = k + 1
    return curr 
```

## Control

> if真麻烦，为什么自己定义一个if_(c,t,f)呢？

``` python
def real_sqrt(x):
    if x >= 0:
        return sqrt(x)
    else:
        return 0
```

这样当然可以,但是.....

```python
def if_(c, t, f):
    if c:
        t
    else:
        f
        
from math import sqrt
def real_sqrt(x):
	return if_(x >= 0, sqrt(x), 0)

```

但是，call expression会强制evaluate every parts.

正因如此我们需要条件语句

## Control Expression

有一些表达式允许Python解释器跳过一些subexpression。

### Logical Operator

and和or

```python
from math import sqrt
def has_big_sqrt(x):
	return x > 0 and sqrt(x) > 10
```

恭喜，这回没有崩溃。

```python
def reasonable(n):
    return n == 0 or 1/n != 0
```

## High-Order Functions

```python

from math import pi, sqrt

def area(r, shape_constant):
    assert r > 0, 'A length must be positive'
	return r * r * shape_constant

def area_square(r):
    return area(r,1)

def area_circle(r):
    return area(r, pi)

def area_hexagon(r):
    return area(r, 3 * sqrt(3) / 2)
```

!!!!!!新知识！！！！！！！！

*** assert***

如果表达式的值是假，回复一句话

> 我们不仅可以概括数字，还可以概括计算过程

```python
def identity(k):
    return k

def cube(k):
    return pow(k, 3)

def summation(n,term):
    """Sum the first N terms of a sequence"""
    total, k = 0, 1
    while k <= n:
        total, k = total + term(k), k + 1
        return 
def sum_natruals(n):
    return summation(n, identity)

def sum_cubes(n):
    return summation(n, tube)

from operator import mul
def pi_term(k)
	return 8 / mul(4 * k - 3, 4 * k -1)

```

```python
def make_adder(n):
    def adder(k):
        return k + n
```

![微信图片_20250904201906](C:\Users\ZHAOKAI\Desktop\typora\cs61a\微信图片_20250904201906.png)

看不懂

### Functions as return value

闭包？学一学

函数也是一类值，高阶函数是可以接受另一个函数，或者输出另一个函数的函数。

我们可以凭借他：

* Express general methods
* Remove repetition

-------------------

闭包：

``` def f():
def f():
	a = 5
	def g():
		print(a)
    
    g()
```

* 闭包：一个可以访问别的函数作用域里面东西的函数

```python
def f():
    a = 1
    
    def g():
		print(a)
        ...
    
    def add():
        nonlocal a
        a += 1
        
    return add, visit

add_func, v =f()
add_func()
v()


```



作业

：for _ in range(n)

就是执行n次
