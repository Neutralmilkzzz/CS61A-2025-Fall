# LECTURE 16

小明，下面给你一份**CS61A Lecture 15 & 16** 的系统级笔记+讲解，按你已经掌握前面内容为前提，读一遍就能上手做题。配了完整、可运行的小例子与常见坑位说明。若你更喜欢“先听我讲一遍”，就顺着读，每段都像我在板书讲解一样。

> 说明：不同学期编号会有轻微差异，但核心主题稳定——**L15：可变值/可变函数（mutation & nonlocal）**，**L16：迭代器与生成器（iterators & generators）**。官方学习指引与迭代器讲义可参考这里。([CS 61A](https://cs61a.org/study-guide/mutation/?utm_source=chatgpt.com))
>  第三方同学笔记与历年安排也对应这一主题划分。([winstonpurnomo.github.io](https://winstonpurnomo.github.io/homepage/cs61a.html?utm_source=chatgpt.com))

------

# Lecture 15｜可变值 & 可变函数（Mutation, Aliasing, `nonlocal`）

## 1. 不可变 vs 可变（回顾）

- **不可变**：`int`、`float`、`bool`、`str`、`tuple`。值一旦创建就不能原地改。
- **可变**：`list`、`dict`、`set`等。可以**就地修改**（in-place），如 `append`、`pop`、`dict.__setitem__`。

### 1.1 “就地改”和“新建”的差别

```python
# 就地修改（影响原对象）
a = [1, 2]
b = a
a.append(3)
print(a, b)  # [1, 2, 3] [1, 2, 3] —— 别名共享同一块内存

# 新建（返回新对象）
x = [1, 2]
y = x + [3]
print(x, y)  # [1, 2] [1, 2, 3]
```

**要点**：可变对象经常产生**别名（aliasing）**，函数间传递列表/字典很容易互相“牵一发动全身”。

### 1.2 浅拷贝 vs 深拷贝

```python
import copy
m = [[1], [2]]
shallow = list(m)        # 或 m[:]
deep = copy.deepcopy(m)

m[0].append(99)
print(shallow)  # [[1, 99], [2]]  子列表仍共享
print(deep)     # [[1], [2]]      完全独立
```

### 1.3 可变默认参数大坑（必须会考）

```python
def bad_append(x, lst=[]):   # 坑：默认列表只创建一次，会“记住历史”
    lst.append(x)
    return lst

print(bad_append(1))  # [1]
print(bad_append(2))  # [1, 2]  WTF?

# 正解
def good_append(x, lst=None):
    if lst is None:
        lst = []
    lst.append(x)
    return lst
```

## 2. 纯函数 vs 有副作用的函数

- **纯函数**：仅由输入决定输出，不读写外部可变状态，无副作用（便于推理、测试）。
- **有副作用**：修改了外部状态（如全局变量、闭包中的变量、传入的列表/字典等）。
- CS61A强调**抽象屏障**：外部只依赖承诺的行为，不关心内部表示是否可变（换实现不破坏对外契约）。([CS 61A](https://cs61a.org/study-guide/mutation/?utm_source=chatgpt.com))

## 3. 可变函数（Mutable Functions）与 `nonlocal`

**场景**：我们希望用**函数＋闭包**保存“累积状态”（比如计数器、余额）。Python 用 `nonlocal` 在*最近的非全局父帧*中**重绑定**变量。

### 3.1 计数器示例（环境图关键）

```python
def make_counter():
    count = 0
    def inc():
        nonlocal count       # 不写 nonlocal -> UnboundLocalError（赋值会创本地名）
        count += 1
        return count
    return inc

c1 = make_counter()
print(c1(), c1(), c1())  # 1 2 3
c2 = make_counter()
print(c2())              # 1  （c2状态独立）
```

**环境图要点**：

- `make_counter` 返回的 `inc` 持有 **自由变量** `count` 的引用。
- `nonlocal` 告诉解释器：对 `count` 的赋值**发生在父帧**，不是创建新的局部变量。
- 若漏掉 `nonlocal`，`count += 1` 会被解析为**先读再写**局部 `count`，导致**未绑定错误**。

### 3.2 银行账户（封装+不变式）

```python
def make_account(balance, pin):
    def deposit(amount, p):
        nonlocal balance
        if p != pin: return "PIN error"
        balance += amount
        return balance
    def withdraw(amount, p):
        nonlocal balance
        if p != pin: return "PIN error"
        if amount > balance: return "Insufficient funds"
        balance -= amount
        return balance
    return deposit, withdraw

dep, wdr = make_account(100, "1234")
print(dep(50, "1234"))   # 150
print(wdr(70, "0000"))   # PIN error
```

**抽象屏障**：外部无法直接改 `balance`，只能通过闭包方法遵守规则（不变式：余额≥0）。这正是“用函数实现对象”的思想。([CS 61A](https://cs61a.org/study-guide/mutation/?utm_source=chatgpt.com))

### 3.3 Memoization（带状态的缓存）

```python
def memo(fn):
    cache = {}
    def wrapped(x):
        if x in cache:
            return cache[x]
        val = fn(x)
        cache[x] = val
        return val
    return wrapped

@memo
def fib(n):
    return n if n < 2 else fib(n-1) + fib(n-2)

print(fib(35))  # 很快
```

### 3.4 在函数里“就地改”和“重绑定”的区别

```python
def f(lst):
    lst.append(9)   # 就地改：影响外部同一对象
def g(lst):
    lst = lst + [9] # 重新绑定到新对象：外部不变

a = [1,2]
f(a);  print(a)  # [1,2,9]
g(a);  print(a)  # [1,2,9]


```

# LECTURE 15 RE.RE

![image-20251006204024430](C:\Users\ZHAOKAI\AppData\Roaming\Typora\typora-user-images\image-20251006204024430.png)

对于不可变数据比如string和integer，a=b，b改变不影响a，本质创建一个新的object。

对于可变数据比如list1=list2，list2变会影响list1，因为本质是一个object的两个名字

```python
>>> a = [1, 2, 3]
>>> b = 1
>>> b = a
>>> b
[1, 2, 3]
>>> a = 1
>>> a
1
>>> b
[1, 2, 3]
>>>
```

是一个绑定的关系

如果在immutable的地方含有mutable的东西，还是可能改的。

```python
>>> s = ([0, 1], 1)
>>> s
([0, 1], 1)
>>> s[0] = 1
Traceback (most recent call last):
  File "<python-input-22>", line 1, in <module>
    s[0] = 1
    ~^^^
TypeError: 'tuple' object does not support item assignment
>>> s[0][0] = 1
>>> s
([1, 1], 1)
>>>
```

## Mutation

简单来说，对于两个不同的列表，在原表上操作一定会mutate，但是另外赋值就不会。

```python
a = [1,2,3]
b = a
a.pop()
[1,2]
b
[1,2]
a.append(1)
[1,2,1]
b
[1,2,1]
a = [1,2,3]
b
[1,2,1]
```

你怎么知道两个东西是否相同？

用`is`

==表示值的相等，is表示引用对象的相同

```python
>>> a = [1,2,3]
>>> b =a
>>> b
[1, 2, 3]
>>> b is a
True
>>> b = [1,2,3]
>>> b is a
False
>>> b == a
True
```

Mutable Functions



一个好的例子：

```python
def time_to_retire(self, amount):
    """Return the number of years until balance would grow to amount."""
    assert self.balance > 0 and amount > 0 and self.interest > 0
    n = 0
    balance = self.balance          # 局部拷贝，修改 balance 不影响 self.balance
    while balance < amount:
        balance *= (1 + self.interest)  # 注意要用 self.interest
        n += 1
    return n
```



------

# Lecture 16｜迭代器与生成器（Iterators & Generators）

## 1. 可迭代 vs 迭代器

- **可迭代（iterable）**：能产生迭代器的对象（`list`、`dict`、`set`、`str`、`range` 等）。
- **迭代器（iterator）**：遵循**迭代协议**的对象，支持：
  - `iter(it)` 返回自身或一个迭代器
  - `next(it)` 产出下一个元素；无元素时抛 `StopIteration`
- **for循环工作原理**：`it = iter(iterable)`；循环中反复 `next(it)`；捕获 `StopIteration` 后结束。官方迭代器讲义与实验详见下列材料。([CS 61A](https://cs61a.org/assets/slides/16-Iterators_1pp.pdf?utm_source=chatgpt.com))

### 1.1 一次性 vs 可复用

```python
# 一次性迭代器
it = iter([3, 4, 5])
print(next(it), next(it))  # 3 4
print(list(it))            # [5]
print(list(it))            # [] 已耗尽

# 可复用容器
s = [3,4,5]
print(list(s), list(s))    # [3,4,5] [3,4,5]
```

**要点**：把迭代器传下去时，**它的“指针”会移动且会被耗尽**，这是大量BUG的来源。([CS 61A](https://cs61a.org/study-guide/iterators/?utm_source=chatgpt.com))

## 2. 常用迭代器家族（必须熟）

```python
# range：惰性产生等差序列
for x in range(2, 10, 3):  # 2,5,8
    pass

# 字典视图：动态反映字典内容
d = {"a":1, "b":2}
k = d.keys()      # 可迭代视图
i = iter(k)
print(next(i))    # 'a'（实现相关，但插入顺序保持）
d["c"] = 3        # 迭代时结构性修改可能使已持有的迭代器失效（务必避免）
                  #（CS61A讲义会提醒：不要边迭代边改底层容器）
# enumerate：带下标
for idx, val in enumerate(["x","y"]): pass

# zip：对齐最短长度
for a, b in zip([1,2,3], [10,20]): pass   # (1,10) (2,20)

# map/filter：返回迭代器（惰性），常与 list(...) 或 for 配合
```

更多细节与题目在对应的学习指引与讲义里。([CS 61A](https://cs61a.org/study-guide/iterators/?utm_source=chatgpt.com))

## 3. 生成器函数与 `yield`

- **生成器函数**：包含 `yield` 的函数。调用时**不会立刻执行完**，而是返回一个**生成器对象**（本质是迭代器）。
- 每次 `next()` 执行到下一个 `yield` 暂停并返回值；再次 `next()` 从暂停处继续运行，直到抛 `StopIteration`。

```python
def countdown(n):
    while n > 0:
        yield n   # 暂停并产出
        n -= 1

cd = countdown(3)
print(next(cd))   # 3
print(list(cd))   # [2, 1]
print(list(cd))   # [] 已耗尽
```

### 3.1 生成器表达式（更轻量）

```python
# 列表推导是立刻构造完整列表；生成器表达式是惰性
squares_list = [x*x for x in range(5)]     # [0,1,4,9,16]
squares_gen  = (x*x for x in range(5))     # 迭代器
print(next(squares_gen), list(squares_gen))# 0 [1,4,9,16]
```

### 3.2 `yield from`（拆分委托）

```python
def flatten_once(seq_of_seqs):
    for seq in seq_of_seqs:
        yield from seq   # 直接把子可迭代的元素“透传”出来
```

### 3.3 迭代器流水线（内存友好）

```python
def naturals(start=1):
    while True:
        yield start
        start += 1

def take(n, it):
    out = []
    try:
        for _ in range(n):
            out.append(next(it))
    except StopIteration:
        pass
    return out

# 取出能被3整除的前5个自然数
def divisible_by(k, it):
    for x in it:
        if x % k == 0:
            yield x

five = take(5, divisible_by(3, naturals()))
print(five)  # [3, 6, 9, 12, 15]
```

## 4. 常见坑与规约

1. **不要一边迭代一边修改底层容器**（特别是 `dict`/`list` 结构性修改）。
2. **迭代器会被消耗**：`list(it)` 之后 `it` 为空，再用会得到 `[]`。
3. **多迭代器独立性**：对同一容器 `iter(s)` 多次得到的多个迭代器**互不影响**；但对同一个**迭代器对象**复制引用则共享指针。
4. **for 的本质**：自动创建迭代器与处理 `StopIteration`；理解后能写更地道的代码。
    （这些点都在迭代器讲义/实验中反复强调。）([CS 61A](https://cs61a.org/assets/slides/16-Iterators_1pp.pdf?utm_source=chatgpt.com))

------

# 速通对比表（一图贯穿）

| 概念       | 关键API/语法           | 行为                   | 常见错误                                 |
| ---------- | ---------------------- | ---------------------- | ---------------------------------------- |
| 可变对象   | `list`, `dict`, `set`  | 就地改动影响别名       | 误把 `lst = lst + [...]` 当成就地修改    |
| 不可变对象 | `int`, `str`, `tuple`  | 无法原地更改           | 想改字符串某字符                         |
| `nonlocal` | 在闭包里重绑定父帧变量 | 让闭包“记住并更新”状态 | 漏写 `nonlocal` 导致 `UnboundLocalError` |
| 迭代器     | `iter(x)`, `next(it)`  | 一次性、会被消耗       | `list(it)` 之后再次使用                  |
| 生成器     | `def ...: yield ...`   | 惰性产出、可暂停恢复   | 忘了是一次性的                           |
| 容器视图   | `dict.keys()` 等       | 反映底层变化           | 迭代时结构性修改字典                     |

------

# 真题/练习风格的小题（含完整解）

### 题1：写一个闭包计数器，支持 `inc()`、`dec()`、`reset()`，并保证计数永不掉到负数。

```python
def make_counter():
    count = 0
    def inc():
        nonlocal count
        count += 1
        return count
    def dec():
        nonlocal count
        if count == 0:
            return 0
        count -= 1
        return count
    def reset():
        nonlocal count
        count = 0
        return count
    return inc, dec, reset

inc, dec, reset = make_counter()
print(inc(), inc(), dec(), reset(), dec())  # 1 2 1 0 0
```

### 题2：实现 `scale(it, k)`——把任意可迭代 `it` 的元素按倍数 `k` 惰性放大（实验风格题）。

```python
def scale(it, k):
    for x in it:
        yield x * k

print(list(scale([1,2,3], 10)))  # [10, 20, 30]
```

### 题3：实现 `differences(it)`——产出相邻差值，如输入 `1,4,9,10` 产出 `3,5,1`。

```python
def differences(it):
    it = iter(it)
    try:
        prev = next(it)
    except StopIteration:
        return
    for cur in it:
        yield cur - prev
        prev = cur

print(list(differences([1,4,9,10])))  # [3, 5, 1]
```

### 题4：写 `flatten_once(seq_of_seqs)`，只拍平一层（用 `yield from`）。

```python
def flatten_once(seq_of_seqs):
    for seq in seq_of_seqs:
        yield from seq

print(list(flatten_once([[1,2], (3,4), "ab"])))  # [1,2,3,4,'a','b']
```

### 题5（思维题）：为什么下面代码第二次打印为空列表？

```python
it = (x*x for x in range(4))  # 生成器表达式 -> 迭代器
print(list(it))  # [0,1,4,9]
print(list(it))  # []  （已耗尽）
```

**解析**：迭代器是“一次性”；第一次 `list(it)` 已把它消费完了。

------

# 考点清单（考前3分钟速记）

- **L15**：
  - 别名、浅/深拷贝、可变默认参数陷阱。
  - 纯函数 vs 副作用；抽象屏障与不变式。
  - `nonlocal` 规则（最近非全局父帧）、环境图；闭包持久状态（计数器/账户/缓存）。
- **L16**：
  - 迭代协议：`iter`/`next`/`StopIteration`；for 的工作原理。
  - 一次性迭代器 vs 可复用容器；迭代器会被消耗。
  - 常用迭代器：`range`、字典视图、`enumerate`、`zip`、`map`、`filter`。
  - 生成器函数、生成器表达式、`yield from`；流水线思维；**不要边迭代边改底层容器**。([CS 61A](https://cs61a.org/assets/slides/16-Iterators_1pp.pdf?utm_source=chatgpt.com))

------

# 延伸阅读/官方资源（查缺补漏）

- **Mutation（可变/`nonlocal`）学习指引**：讲清抽象屏障、`nonlocal` 的环境规则与示例。([CS 61A](https://cs61a.org/study-guide/mutation/?utm_source=chatgpt.com))
- **Iterators 学习指引 & 讲义**：迭代协议、for 语句细节、常见内置迭代器和练习。([CS 61A](https://cs61a.org/study-guide/iterators/?utm_source=chatgpt.com))
- **Lab 05（Iterators & Generators）**：上手做惰性函数、流水线题。([CS 61A](https://cs61a.org/lab/lab05/?utm_source=chatgpt.com))
- **同学整理的讲义版路线**（对应 L15 可变函数、L16 迭代器与生成器）：([winstonpurnomo.github.io](https://winstonpurnomo.github.io/homepage/cs61a.html?utm_source=chatgpt.com))

------

如果你愿意，下一步我可以：

1. 按你最近卡住的题（比如 `make_averaged`、树、Hog里的策略函数）逐题用**环境图+代码走读**再过一遍；
2. 出一套**Lecture 15/16 专项练习**（5~8题），你做我批，再讲错因与替代解。
