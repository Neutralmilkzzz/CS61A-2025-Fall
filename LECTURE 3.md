2025.9.2

 ![image-20250904173457766](C:\Users\ZHAOKAI\AppData\Roaming\Typora\typora-user-images\image-20250904173457766.png)

**Name has no meaning without environment**

每一个表达式都在一个环境中被评估

![image-20250904174317724](C:\Users\ZHAOKAI\AppData\Roaming\Typora\typora-user-images\image-20250904174317724.png)

```python
from operator import mul
def sqaure(square):
    return mul(square, square)
square(4)
```

作为operator的square在global frame中，因此与平方函数绑定

作为operand的square在local frame中首先被查找为4，没有找到过平方函数，因为始终只找“最早的”那个。

## Miscellaneous Python Features

### operator

```python
truediv(2013,10)
201.3
floordiv(2013,10)
201
2013 % 10 
3
from operator import mod 
mod(2013,10)
3
5/3
1.666666666666666667
 
```

比如说

```python
def divide_exact(n,d):
	return n // d, n % d

quotient, remainder =divide_exact(2013,10)
quotient
201
```

```python
"""Our first Python source file"""

from operator import floordiv, mod

def divide_exact(n.d=10):
    """Return the quotient and the remainder of dividing N by D"""
	return floordiv(n ,d), mod(n ,d)

q, r = divide_exact(2013)
print('Quotient:', q)
print('Remainder:', r)
```

两个要点：

* 可以给d一个默认值
* 可以让函数返回两个值

## Conditional Statement(条件语句)

```python
def absolute_value(x):
    if x < 0;
    	return -x
    elif x ==0;
    	return 0
    else:
        return x
```

在python里有False value和True Value

False values：False，0，'',None （more to come)

## Iteration

用while
