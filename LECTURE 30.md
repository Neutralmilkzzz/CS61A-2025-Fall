## 异常

python里有很多的异常。

用`try`和`except`和`finally`可以处理异常

## try

对于**容易报错**的模块，可以放在try子句中，并且将处理异常的代码编写在except子句里。

```python
def test(0):
	try:
    	return 1/x
	except ZeroDivisioError :
    	return float('inf')
```



### raise

可以用raise强制“抛出”一个错误

```python
raise TypeError("hello")
```



## reduce

 Definition: **reduce** the iterable to a single value

```python
initial_missing = object()

def reduce(function, iterable, initial=initial_missing, /):
    it = iter(iterable)
    if initial is initial_missing:
        value = next(it)
    else:
        value = initial
    for element in it:
        value = function(value, element)
    return value
```

