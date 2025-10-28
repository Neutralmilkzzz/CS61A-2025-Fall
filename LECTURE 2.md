# LECUTRE 2

## DEFINING FUNCTION

> Binds names to *expression*

定义**语句**的方式：**def**

![image-20250831100357734](C:\Users\ZHAOKAI\AppData\Roaming\Typora\typora-user-images\image-20250831100357734.png)

* formal parameter: 形式参数
* function signature：函数签名，指**def**和**：**中间的部分。用来确定argument的数量
* function body：函数体。冒号之后的部分

> 当你定义一个函数时，他并没有真正被调用

**调用自定义函数的方法**

1. Add a local frame
2. Bing the *formal parameter* to its **arguments** in that frame
3. Execute the body of the function in that new environment.

```python
from operator import mul
def square(x):
    return mul(x, x)
square(-2)
```

> 在Python中，local frame（局部帧）是指函数调用时创建的一个新的、独立的命名空间，用于存储该函数内部定义的变量和参数。每个函数调用都会生成一个新的局部帧，其父帧通常是全局帧或另一个局部帧。
>
> global frame（全局帧）则是程序主体中定义的所有变量和函数所在的环境。全局帧中的变量可以在整个模块中访问，除非被局部帧中的同名变量遮蔽。

## Looking up names  in environments

> 每个被evaluate的expression都是在某个environment的context里被evaluated的

***今天最重要的！！！***

1. 一个环境是一系列的帧

> 举个例子：寻找square function的一些名字时：
>
> * 先在local frame里面寻找
> * 如果找不到，去global frame寻找

```python
from operator import mul
mul(3,4)

def square(square):
    return mul(square, square)
"""
此时square还是个函数
"""
square（4）
#这个时候会生成16，为啥呢
#因为先找到的是作为function parameter的square。
```

## Environment diagram

**环境图**是用来**跟踪程序**的方式

> 模拟解释器

* import statement: 导入语句
* assignment statement：赋值语句
* 灰色箭头：执行完了
* 红色箭头：还没执行（即将执行）

![image-20250831112129131](C:\Users\ZHAOKAI\AppData\Roaming\Typora\typora-user-images\image-20250831112129131.png)

> 在 Python 中，`max` 是一个内置函数，用于返回一组值中的最大值。如果你将 `max` 重新赋值为一个函数，那么你将覆盖内置的 `max` 函数，导致无法再使用内置的 `max` 函数。例如：
>
> ```python
> def max(x):
>  return x
> ```
>
>
> 此时，`max` 变成了一个自定义函数，而不是内置函数。因此，如果你尝试使用 `max(1, 2)`，它将调用你定义的函数，而不是内置的 `max` 函数 。
>
> 为了避免这种情况，建议避免使用与内置函数同名的变量或函数名。如果必须使用，可以考虑使用别名或前缀来避免冲突 。

## Print and None

### None是啥？

定义：

# pure function& non-pure function

* 纯函数（pow ,abs):一个输入一个输出（return）
* 非纯函数（print）：输入一个数字输出**none**，但display这个数字















## 作业

### lab01



赋值操作符 (=): 这个操作符将右侧的计算结果赋值给左侧的变量。所以 n //= 10 的意思是将 n 除以 10 的整数结果重新赋值给 n。