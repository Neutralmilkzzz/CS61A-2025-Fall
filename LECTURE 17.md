# LECTURE 17

----

在一开始，首先要处理这个问题：

一个函数体如果含有`yield`，那么他就是一个生成器**惰性**函数。使用它必须先a=func()之后next(a)

# `yield from`

```python
def a_then_b(a, b):
    for x in a:
        yield x
    for x in b:
        yield x
###这固然可以，但2012年，python新增了yield from
def a_then_b(a, b):
    yield from a
    yield from b
```

---------

> 对于for i in b, 这个b并不一定要是一个列表，只要可以迭代就行。
>
> 例子:
>
> ```python
> for i in t
> ###t是一个迭代器
> ```
>
> 
>
> 

--------------

## 用yield处理partition

```python
def list_partitions(n, m):
    """List partitions.

    >>> list_partitions(6, 4)
    9
    """
    if n < 0 or m == 0:
        return []
    else:
        exact_match = []
        if n == m:
            exact_match = [[m]]
        with_m = [p + [m] for p in list_partitions(n-m, m)]
        without_m = list_partitions(n, m-1)
        return exact_match + with_m + without_m

```

还是一直在强调的：**for a in b，b只要是iterable的都可以。

```python
def partitions(n, m):
    """Yield partitions.

    >>> for p in partitions(6, 4): 
    ...     print(p)
    2 + 4
    1 + 1 + 4
    3 + 3
    1 + 2 + 3
    1 + 1 + 1 + 3
    2 + 2 + 2
    1 + 1 + 2 + 2
    1 + 1 + 1 + 1 + 2
    1 + 1 + 1 + 1 + 1 + 1
    """
    if n > 0 and m > 0:
        if n == m:
            yield str(m)
        for p in partitions(n-m, m):
            yield p + ' + ' + str(m)
        yield from partitions(n, m-1)

```

