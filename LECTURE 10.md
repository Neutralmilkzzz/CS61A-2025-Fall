# LECTURE 10

## Order of Recursive Calls

​	如果一个函数调用另一个函数，然后在那之后做一些事情，那个函数调用必须在之后的任何其他操作发生之前返回。

```python
def cascade(n):
    if n < 10:
        print(n)
    else:
        print(n)
        cascade(n // 10)
        print(n)

>>> cascade(12345)
>>>12345
>>>1234
>>>123
>>>12
>>>1
>>>12
>>>123
>>>1234
>>>12345
```

![微信图片_20250907165752](C:\Users\ZHAOKAI\Desktop\typora\cs61a\微信图片_20250907165752.png)

----------

## Tree Recursion

> 斐波那契数列

```python
def fib(n):
    if n = 0:
        return 0
    elif n = 1:
        return 1
    else:
    	return fib(n-1) + fib(n-2)
```



--------

补充

在Python中，`*args` 和 `**kwargs` 允许函数定义时接受任意数量的参数，这主要得益于它们的特殊语法和机制。根据资料，`*args` 用于接收任意数量的位置参数，这些参数会被打包成一个元组；而 `**kwargs` 用于接收任意数量的关键字参数，这些参数会被打包成一个字典。这种机制使得函数能够灵活地处理不同数量和类型的参数，增强了代码的灵活性和可扩展性。

----------------------

```python
def trace1(fn):
    print(fn.__name__,'is calling')
    def same(*args, **kwargs):
        return fn(*args, **kwargs)
    return same
###我写的装饰器
```

-----------

## 计算分区（counting partitions)

![image-20250907184206907](C:\Users\ZHAOKAI\AppData\Roaming\Typora\typora-user-images\image-20250907184206907.png)

### 1. 问题定义

“分拆”是指把正整数 表示为**若干正整数之和**（顺序递增，且每个数不超过 ）。例如 `count_partitions(6, 4)` 要计算“用 1、2、3、4 这几个数，把 6 拆成递增序列的方式数”。

### 2. 递归思路（分治思想）

分拆问题可通过**递归分解**为更简单的子问题，核心是“探索两种选择”：

- **选择1：用至少一个** 
  假设拆分中包含 （比如 4），那么剩下的数需要拆成 （即 ），且仍可以用不超过 的数。因此子问题是 `count_partitions(2, 4)`。
- **选择2：不用** 
  假设拆分中完全不用 （比如不用 4），那么只能用比 小的数（即 1、2、3），因此子问题是 `count_partitions(6, 3)`。

### 3. 递归的本质

通过“用/不用 ”两种选择，把原问题 `count_partitions(n, m)` 拆分为两个子问题：

（直到 或 时，递归终止，返回边界值。）

# 基本情况是一坨屎。

```python
def cp(n, m):
    if m == 1:  # 边界：只能用1，只有1种拆法（全1）
        return 1
    elif n == 0:  # 边界：n=0，无拆法（题目要求正整数分拆）
        return 0
    elif n < m:  # 新增：n < m 时，"用至少一个m"的子问题无解，返回0
        return cp(n, m - 1)
    else:  # 正常情况：分"用至少一个m"和"不用m"两种选择
        return cp(n - m, m) + cp(n, m - 1)

```

------















































作业补充

```python
def make_onion(f, g):
    """Return a function can_reach(x, y, limit) that returns
    whether some call expression containing only f, g, and x with
    up to limit calls will give the result y.

    >>> up = lambda x: x + 1
    >>> double = lambda y: y * 2
    >>> can_reach = make_onion(up, double)
    >>> can_reach(5, 25, 4)      # 25 = up(double(double(up(5))))
    True
    >>> can_reach(5, 25, 3)      # Not possible
    False
    >>> can_reach(1, 1, 0)      # 1 = 1
    True
    >>> add_ing = lambda x: x + "ing"
    >>> add_end = lambda y: y + "end"
    >>> can_reach_string = make_onion(add_ing, add_end)
    >>> can_reach_string("cry", "crying", 1)      # "crying" = add_ing("cry")
    True
    >>> can_reach_string("un", "unending", 3)     # "unending" = add_ing(add_end("un"))
    True
    >>> can_reach_string("peach", "folding", 4)   # Not possible
    False
    """
    def can_reach(x, y, limit):
        if limit < 0:
            return False
        elif x == y:
            return True
        else:
            return can_reach(f(x), y, limit - 1) or can_reach(g(x), y, limit - 1)
    return can_reach    
```

Tree Recursion的完美例子。

----------

经典数学题之用（1，3，9，27）和加法组合出某个数字。

代码实现：

```python
def count_dollars_upward(total):
    bills = [1, 5, 10, 20, 50, 100]
	#一种常规做法：先建立索引
    def helper(total, i):
        if total == 0:
            return 1
        if total < 0 or i >= len(bills):
            return 0
        # 选择一张 bills[i]（继续允许用它），或者不用它（跳到更大面额）
        return helper(total - bills[i], i) + helper(total, i + 1)

    return helper(total, 0)

```

