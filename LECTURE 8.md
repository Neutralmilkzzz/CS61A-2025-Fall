装饰器（decorator）

## What would Python print?

The print function returns **None**. It also displays its arguments when it is called.

 先把函数搞清楚：

```python
def delay(arg):
    print('delayed')
    def g():
        return arg
    return g
```

- `delay(arg)`：被调用时会先打印一次 `'delayed'`，然后**返回一个零参数函数** `g`。
- 这个 `g()` 是闭包，**捕获并返回**当时传入 `delay` 的 `arg`。

现在看表达式：`delay(delay)()(6)()`

### 按调用顺序一步步展开

1. **`delay(delay)`**
   - 这是第一次调用 `delay`，`arg` = 函数对象 `delay`（注意：不是调用，只是把函数本身当作值传进去）。
   - 立刻打印：`delayed`
   - 返回一个零参闭包：记为 `g0`，其中 `g0()` 返回被捕获的 `arg`，也就是**函数 `delay` 本身**。
   - 此时：`delay(delay)` 的值是 `g0`（不是 `delay`，是“返回 delay 的零参函数”）。
2. **后面的第一个 `()`**：`delay(delay)()`
   - 调用 `g0()`，它返回捕获的 `arg` → **返回函数 `delay`**。
   - 现在表达式变为：`delay(6)()`。
3. **`delay(6)`**
   - 这是第二次调用 `delay`，`arg` = `6`。
   - 立刻打印：`delayed`
   - 返回另一个零参闭包：记为 `g1`，其中 `g1()` 返回 **6**。
   - 现在表达式变为：`g1()`。
4. **最后一个 `()`**：`g1()`
   - 返回 **6**。

**最终结果是 6**，并且整个过程中打印了两次 `delayed`（对应两次调用 `delay`）。

------

### 为什么不是你说的 “`delay(0)(6)(0)`”？

- `delay(delay)` 的结果是 **零参函数 `g0`**，不是 `delay` 本身；要通过后面的 `()`（第 2 步）调用 `g0()`，**拿到** `delay` 才能继续 `delay(6)`。
- 另外，`delay(0)` 返回的是一个 **零参函数**。如果你写 `delay(0)(6)`，会报错：
   `TypeError: g() takes 0 positional arguments but 1 was given`
   因为 `g` 不接受参数。

------

### 一个便于记忆的小等式

把 `delay(x)` 想象成：

```
delay(x)  =>  （打印 'delayed'）并返回  thunk(x)  # thunk 是一个零参函数，调用时返回 x
```

于是：

```
delay(delay)()(6)()
= thunk(delay)()(6)()
= delay(6)()
= thunk(6)()
= 6
```

所以它当然不是 0，而是 6。

-------

# 装饰器

```python
def sum_squares_up_to(n): ...
sum_squares_up_to = trace1(sum_squares_up_to)
```

用`@trace1`一步实现记录调用

```python
def trace1(fn):
    """Returns a version of fn that first print what it is called
"""
    def traced(x):
        print('Calling', fn, 'on argument', x)
        return fn(x)
    return traced


@trace1
def square(x):
    return x * x
@trace1
def sum_squares_up_to(n):
    k = 1
    total = 0
    while k <= n:
        total, k = total + square(k), k + 1
    return total

```



实质是：

```python
def trace1(fn):
    def traced(x):
        print('Calling', fn, 'on argument', x)
        return fn(x)
    return traced

def square(x):
    return x*x
square = trace1(square)  # 此时 square 绑定到 traced 闭包，traced 捕获了原始的 square 作为 fn

def sum_squares_up_to(n):
    k = 1
    total = 0
    while k <= n:
        total, k = total + square(k), k + 1  # 注意，这里调用的是“被装饰后的 square”，也就是 traced
    return total
sum_squares_up_to = trace1(sum_squares_up_to)
```

