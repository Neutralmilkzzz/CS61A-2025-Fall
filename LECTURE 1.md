# Lecture 1

> 我们只需要*函数*（function）

 教授的观点：**所有的表达式（expression）都可以用函数调用符号（function call notation）**。

> 教授打开了一个解释器，提供了一个提示（prompt）来等待他输入表达式并显示它的值。

## 调用表达式

调用表达式有一个特殊的形式。（call expression）

* name a function you want to call
* write down expression

```python
from operator import add , mul
add(2,3)
mul(1,2,3,4,5)
```

### 1.Atanomy of a call expression

![f4a2298f73620ac751b5761c8095ad9](C:\Users\ZHAOKAI\Documents\WeChat Files\wxid_tcjezcak4wsr12\FileStorage\Temp\f4a2298f73620ac751b5761c8095ad9.png)

> 操作符和操作子都是表达式（operator and operand)
>
> 所以这个表达式会被评估为一个值
>
> 这是底层逻辑？

Evaluation procedure for call expressions:

1. Evaluate the operator and the operand subexpressions
2. Apply the function to the argument(参数)

 ### 2. Expression Tree(表达树)

![image-20250830161119922](C:\Users\ZHAOKAI\AppData\Roaming\Typora\typora-user-images\image-20250830161119922.png)

电脑的evaluation procedure：

1. 看到parentheses，这是个call expression
   1. 提取operator，是mul
   2. 提取operand，是个call expression
      1. 提取operator，是add
      2. 提取operand，是2
      3. 提取operand，是个call expression
         1. 提取operator，是mul
         2. 提取operand，是4
         3. 提取operand，是6
         4. 计算出24
      4. 计算出26
   3. 提取operand，是个call expression
      1. 提取operator，是add
      2. 提取operand，是3
      3. 提取operand，是5
      4. 计算出8
   4. 计算出208

2. 输出208

> 这就是表达树
