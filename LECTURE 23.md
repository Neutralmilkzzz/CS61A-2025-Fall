> 为什么先上23呢，还不是为了刷力扣算时间复杂度（（

```python

def fib(n):
    if n == 0 or n == 1:
        return n
    else:
        return fib(n-2) + fib(n-1)

def count(f):
    def counted(n):
        counted.call_count += 1
        return f(n)
    counted.call_count = 0
    return counted

结果：
fib(25)
75025
fib.call_count
242794
```

--------

## Memoization

```python
def memo(f):
    cache = {}
    def memoized(n):
        if n not in cache:
            cache[n] = f(n)
           return cache[n]
    return memoized
```

![image-20250914200759118](C:\Users\ZHAOKAI\AppData\Roaming\Typora\typora-user-images\image-20250914200759118.png)

------

## Exponentiation

```python
def exp(b, n):
    if n == 0:
        return 1
    else:
        return b * exp(b, n-1)
### 普通幂
def exp_fast(b, n):
    if n == 0:
        return 1
    elif n % 2 == 0:
        return exp_fast(b, n//2) * exp_fast(b, n//2)   # 或用 square(exp_fast(b, n/2))
    else:
        return b * exp_fast(b, n-1)
### 快速幂
```

> 老师用了jupyter

![image-20250914202327933](C:\Users\ZHAOKAI\AppData\Roaming\Typora\typora-user-images\image-20250914202327933.png)

# 

## 1. Exponential growth
- **Example**: recursive `fib`
- 特点：Incrementing n multiplies *time* by a constant
- 时间复杂度：  
  \[
  a \cdot b^{n+1} = (a \cdot b^n) \cdot b
  \]

---

## 2. Quadratic growth
- **Example**: `overlap`
- 特点：Incrementing n increases *time* by n times a constant
- 时间复杂度：  
  \[
  a \cdot (n+1)^2 = (a \cdot n^2) + a \cdot (2n+1)
  \]

---

## 3. Linear growth
- **Example**: slow `exp`
- 特点：Incrementing n increases *time* by a constant
- 时间复杂度：  
  \[
  a \cdot (n+1) = (a \cdot n) + a
  \]

---

## 4. Logarithmic growth
- **Example**: `exp_fast`
- 特点：Doubling n only increments *time* by a constant
- 时间复杂度：  
  \[
  a \cdot \ln(2n) = (a \cdot \ln n) + a \cdot \ln 2
  \]

---

## 5. Constant growth
- **Example**: array access
- 特点：Increasing n doesn’t affect *time*  
- 时间复杂度：O(1)

----------

## SPACE

空间的消耗是被value占用的。

哪些环境帧是我们evaluate时需要的？

这张 slide 讲的是 **空间消耗（The Consumption of Space）**，也就是程序执行时内存是怎么被用掉的。

------

## 📌 关键点

1. **哪些环境框架需要保留？**
   - 在求值过程中，有一组 *active environments*（活动环境）。
   - 这些活动环境里的值和 frame 会占用内存。
   - 其他已经不用的值和 frame 可以被回收（比如 Python 的垃圾回收）。
2. **Active environments（活动环境）包含什么？**
   - **正在执行的函数调用的环境**（比如你调用了一个函数 f，它的局部变量存在 f 的环境里）。
   - **这些函数的父环境**（如果 f 定义在 g 里面，g 的环境也要保留，因为 f 可能会访问 g 的变量）。
3. **Python Tutor** 可以可视化显示当前哪些环境框架是“活着的”。

------

## 🎯 总结

这部分其实是告诉你：

- 内存里不可能把所有历史环境都保存，只保留“正在执行的调用链”和它的父作用域。
- 这就是为什么递归调用会消耗很多内存（每一层递归都会有一个新的环境 frame）。

------

要不要我帮你整理一个 **递归调用栈与环境 frame 的对照图**（用 Markdown 画出来），这样你能更直观地理解“哪些 frame 活着”？

 