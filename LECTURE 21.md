## Polymorphic Functions

多态函数是A function that applies to many different forms of data.

`str` and `repr` are both polymorphic , they apply to every object.

`repr` invokes a zero-argument method __repr__ on its argument

```python
>>> helf.__repr__()
'Fraction(1, 2)'
```

`str` invokes a zero-argument method __str__ on its argument

```python
>>> helf.__str__()
'1/2'
```



![image-20251007145038464](C:\Users\ZHAOKAI\AppData\Roaming\Typora\typora-user-images\image-20251007145038464.png)

repr只在类属性里。

str在实例属性中会被忽略，如果类属性没有str，也会用repr

![image-20251007150546202](C:\Users\ZHAOKAI\AppData\Roaming\Typora\typora-user-images\image-20251007150546202.png)

简单的说，str用来打印print，repr直接用。

Simply put: repr is a method invoked to display an object as a python expression
