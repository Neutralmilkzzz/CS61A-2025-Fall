# LECTURE 5

## Environments for Higher-order Functions

Higher-order function：A function 接收一个function并且返回一个function

> 讲环境和环境示意图完全是为了准确解释高阶函数

![image-20250905112822325](C:\Users\ZHAOKAI\AppData\Roaming\Typora\typora-user-images\image-20250905112822325.png)

```python
def make_adder(n):
    def adder(k):
        return k + n
    return adder
```

我们是怎么在函数里嵌入数据的？

下面用 CS61A 风格把这段代码的**环境图（environment diagram）**讲清楚。

## 代码

```python
def make_adder(n):
    def adder(k):
        return k + n
    return adder

add_three = make_adder(3)
add_three(4)
```

## 执行过程与环境图

### 1) 定义阶段（加载脚本）

- 创建**全局帧** `Global`。
- 绑定 `make_adder -> <function make_adder>`，其 **parent/env** 指向 `Global`。

```
Global:
  make_adder ──► <func make_adder | parent=Global>
```

------

### 2) 调用 `make_adder(3)`

- 新建调用帧 `f1: make_adder`：
  - 形参绑定：`n = 3`
  - 在 `f1` 内**创建函数对象** `adder`，其 **parent/env 指向 f1**（这一步形成闭包）。
- `return adder` 把这个函数对象返回到全局，并绑定给 `add_three`。

```
Global:
  make_adder ──► <func make_adder | parent=Global>
  add_three ───► <func adder | parent=f1>

f1: make_adder
  n = 3
```

> 关键点：由于 `adder` 的 **parent** 指向 `f1`，所以 `f1` 不会被回收；`n=3` 被“闭包”起来。

------

### 3) 调用 `add_three(4)`（实际是调用 `adder(4)`）

- 新建调用帧 `f2: adder`，其 **parent/env= f1**：
  - 形参绑定：`k = 4`
  - 执行 `return k + n`
    - 查找 `k`：在 `f2` 找到 `k=4`
    - 查找 `n`：`f2` 没有，沿着 **parent** 到 `f1` 找到 `n=3`
  - 计算得到 `7` 并返回。

```
Global:
  make_adder ──► <func make_adder | parent=Global>
  add_three ───► <func adder | parent=f1>

f1: make_adder
  n = 3

f2: adder (parent = f1)
  k = 4
  result = 7
```

## 小结

- `adder` 是一个**闭包**，它“携带”着创建时所在环境 `f1`，因此能在之后的调用里继续访问 `n=3`。
- `add_three(4)` 的值是 `7`，因为 `4 + 3 = 7`。
- 多次调用 `make_adder` 会得到不同的闭包（不同的 `f1`，各自的 `n` 不同）。

## 怎么画环境图

![image-20250905120117532](C:\Users\ZHAOKAI\AppData\Roaming\Typora\typora-user-images\image-20250905120117532.png)

## Local Names

![image-20250905132750923](C:\Users\ZHAOKAI\AppData\Roaming\Typora\typora-user-images\image-20250905132750923.png)

会报错：找不到y



因为parent frame是global frame



## Function composition

```python
def make_adder(n):
    def adder(k):
        return k + n
    return adder

def square(x):
    return x * x

def triple(x):
    return x * 3

def compose1(f, g):
    def h(x):
        return f(g(x))
    return h

```

![image-20250905134029988](C:\Users\ZHAOKAI\AppData\Roaming\Typora\typora-user-images\image-20250905134029988.png)

## Lambda Expression

Lambda表达式是评估为函数的表达式

![image-20250905140615937](C:\Users\ZHAOKAI\AppData\Roaming\Typora\typora-user-images\image-20250905140615937.png)

* 没有return function
* 必须是一个single expression
  * 比如说，你想用while，就不能用lambda
* lambda并不常见，但它们非常重要，尤其在一些其他语言里。

与def的最大差异：不会给新函数**命名**



>>> max(a, key=lambda x: -x)
>>> 1
>>>
>>> 一个常见的例子：key里面填的常常就是lambda函数

![image-20250905141606527](C:\Users\ZHAOKAI\AppData\Roaming\Typora\typora-user-images\image-20250905141606527.png)

---------

## 库里库里库里库里

```python
def make_adder(n):
	return lambda k: n + k

make_adder(2)(3)
add(2,3)
###
there's a general relationship between these functions
###
```

我们可以用代码表达这个普遍的关系

柯里化很重要，但是他讲得不太好。

作业：

for i in range（n)

指的是0到n-1

辗转相除法获得

```python
c, d = a, b
    while a != b:
        if a > b:
            a = a - b(a-=b)
        elif b > a:
            b = b - a
    return (c / a) * (d / a) * a
```

