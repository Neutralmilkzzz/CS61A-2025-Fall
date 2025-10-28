# LECTURE 13

## Data Abstraction

> 2025.9.10

![image-20250910102828409](C:\Users\ZHAOKAI\AppData\Roaming\Typora\typora-user-images\image-20250910102828409.png)

一种有理数的数据抽象

----------

## Abstraction Barriers

![image-20250910110430734](C:\Users\ZHAOKAI\AppData\Roaming\Typora\typora-user-images\image-20250910110430734.png)

我反正看不懂，谁看懂了告诉我

-----------

## Representing Pairs Using Lists

![image-20250910113048153](C:\Users\ZHAOKAI\AppData\Roaming\Typora\typora-user-images\image-20250910113048153.png)

-----------

````python
```python
def add_rational(x, y):
    """Add rational numbers x and y."""
    nx, dx = numer(x), denom(x)
    ny, dy = numer(y), denom(y)
    return rational(nx * dy + ny * dx, dx * dy)

def mul_rational(x, y):
    """Multiply rational numbers x and y."""
    return rational(numer(x) * numer(y), denom(x) * denom(y))

def rationals_are_equal(x, y):
    """Return whether rational numbers x and y are equal."""
    return numer(x) * denom(y) == numer(y) * denom(x)

def print_rational(x):
    """Print rational x."""
    print(numer(x), "/", denom(x))


# Constructor and selectors

def rational(n, d):
    """Construct a rational number x that represents n/d."""
    return [n, d]

def numer(x):
    """Return the numerator of rational number x."""
    return x[0]

def denom(x):
    """Return the denominator of rational number x."""
    return x[1]
```
````

是的——这段代码**就是**在示范 *data abstraction*（数据抽象），而且做得挺标准：

## 抽象屏障在哪里？

- **构造器**：`rational(n, d)`（负责“怎么造一个有理数”）
- **选择器**：`numer(x)` / `denom(x)`（负责“怎么取分子/分母”）
- **操作**：`add_rational`、`mul_rational`、`rationals_are_equal`、`print_rational`
   这些函数**只通过选择器读数据、只通过构造器创建结果**，从不直接写 `x[0]`/`x[1]`——这就遵守了**抽象屏障**。

## 为什么这就叫数据抽象？

因为外部代码**不知道也不需要知道**“有理数内部怎么存”（当前用的是 `[n, d]` 列表），只认接口：

> “怎么造（constructor）+ 怎么取（selectors）+ 能做什么（operations）”。

因此你可以**随时替换内部表示**而不影响外部用法。比如把内部从 `list` 改成 `tuple` 或 `dict`：

```python
# 只改这三样，其它 add/mul/equal/print 都不用改
def rational(n, d):      # tuple 版
    return (n, d)

def numer(x): return x[0]
def denom(x): return x[1]
```

或

```python
def rational(n, d):      # dict 版
    return {'n': n, 'd': d}

def numer(x): return x['n']
def denom(x): return x['d']
```

操作函数依旧能跑，这就是数据抽象的价值。

## 这段代码还能更“工程化”吗？

可以，把**不变量**放进构造器里（例如禁止 0 分母、统一约分、分母恒正），抽象会更强：

```python
import math
def rational(n, d):
    assert d != 0
    g = math.gcd(n, d); n//=g; d//=g
    if d < 0: n, d = -n, -d
    return (n, d)  # 或 dict/类/闭包任意
```

> 注意：Python 不强制封装，所以别的代码**仍然可以**直接写 `[1,2]` 或 `x[0]` 来“破坏屏障”。靠的是**纪律/接口文档/测试**来约束；如果想“更硬”的屏障，可以用**类 + 私有字段**或**闭包**把内部完全藏起来。

**结论**：只要大家都通过 `rational/numer/denom` 这组接口来用，而不是直接摸内部结构，这就是合格的 *data abstraction* 实现。
