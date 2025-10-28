# LECUTRE 7

一个lambda函数的父级是lambda表达式被评估的当前帧



```python
a = 1                      # (1) 全局 a

def f(g):
    a = 2                  # (2) f 的局部 a（会被闭包捕获）
    return lambda y : a * g(y)

f(lambda y : a + y)(a)     # (3) 这里的 a 作为实参
```







# 一步步画 Environment Diagram（词法作用域 / LEGB）

## 1) 全局帧（Global Frame, `GF`）

- `a → 1`
- `f → <function f | parent=GF>`

随后创建传给 `f` 的那个 lambda（记作 `λ_g`）：

- `λ_g = (lambda y: a + y)` **定义在全局作用域**，因此它的**父环境是 `GF`**。它体内的 `a` 会按全局名查找（不是闭包捕获局部变量，只是用到全局名）。

## 2) 调用 `f(λ_g)`

创建 `f` 的调用帧 `F1`（父环境 `GF`）：

- 形参：`g → λ_g`
- 赋值：`a → 2`（这是 `f` 的**局部** `a`）

`f` 返回一个新的 lambda（记作 `λ_ret`）：

- `λ_ret = (lambda y: a * g(y))`
- **定义在 `F1` 内部**，因此 **`λ_ret` 闭包捕获 `F1` 中的名字**，也就是：
  - 抓住了 `F1` 的 `a`（值为 2）
  - 抓住了 `F1` 的 `g`（指向 `λ_g`）

> 小结：`λ_ret` 里的 `a` 是 **2（来自 `f` 的局部）**；而 `g` 是传入的 `λ_g`。

## 3) 计算 `f(λ_g)(a)`

外层调用结果是 `λ_ret`，现在调用 `λ_ret(a)`。
 这里实参里的 `a`（右边这一坨的最后一个 `(a)`）按**全局环境**取值，因此：

- 实参 `a` = 全局 `a` = **1**

进入 `λ_ret` 的调用帧 `R1`（父环境是 `F1`）：

- 形参：`y → 1`
- 求值表达式：`a * g(y)`

逐项分解：

- `a`：按词法链在**父环境 `F1`**里找到，值是 **2**
- `g(y)`：调用的是 `λ_g(1)`。`λ_g` 的父环境是 `GF`，它体内表达式是 `a + y`
  - 这里的 `a` 按全局查找 → 全局 `a` = **1**
  - `y` 是实参 1
  - 所以 `g(1) = 1 + 1 = 2`

因此 `λ_ret(1)` 的结果是：`a * g(y) = 2 * 2 = 4`。

# 三个 `a` 各指向谁？

1. **全局 `a`**：最外层的 `a = 1`，以及最后调用 `(...)(a)` 中的这个 `a` 都是它，值 **1**。
2. **`f` 的局部 `a`**：`f` 里赋值的 `a = 2`，被返回的 `λ_ret` **闭包捕获**并在 `a * g(y)` 中使用，值 **2**。
3. **`λ_g` 里的 `a`**：因为 `λ_g` 定义在**全局作用域**，它体内的 `a` 走全局查找，值 **1**。

# 结果

最终表达式值为 **4**（`2 * (1 + 1)`）。

-----------------

## Return 语句

`return none`可以结束一个while语句

```python
def search（f）：
	x = 0
    while True:
        if f(x):
            return x
        x += 1
        
def is_three(x):
    return x == 3

def square(x):
    return x * x

def positive(x):
    return max(0, square(x) - 100)

def inverse(f):
	"""Return g(y) such that g(f(x))"""
    return lambda y : search(lambda x: f(x) == y)
"""计算反函数的常规操作"""

```

----------

## Functional Abstraction

![027b514e9c15760015ae78db6a89dca](C:\Users\ZHAOKAI\Documents\WeChat Files\wxid_tcjezcak4wsr12\FileStorage\Temp\027b514e9c15760015ae78db6a89dca.png)

起名原则：

* 名字一般不影响程序，但影响阅读。

* 名字应该反应它的作用。
* 在docstring中记录
* in a large context,用全名更好。

-----------

Error and Traceback

standardin?