> 2025.9.9

# LECTURE 11

## LIST

```PYTHON
odds = [41, 43, 45, 47]
odds[0]
odds[1]
odds[2]
len(odds)
>>> 4
odds[3] - odds[2]
2
odds[odds[3]-odds[2]]
```

1. 使用时，我可以用`digits[3]`或者`getitem（digits, 3)`
2. 调用列表长度时，用`len`
3. 可以用add/mul/来添加列表长度

```python
>>>[2, 7] + digits * 2
[2, 7, 1, 8, 2, 8, 1, 8, 2, 8]
```

-----------

### Containers

```python
>>> digits = [1, 8, 2, 8]
>>> 1 in digits
True
>>> 5 not in digits
True
[1, 2] in [3, [1, 2], 4]
True
[1, 2] in [3, [1, 2], 4]
[1, 2] in [3. [[1, 2]], 4]
```

* 强调格式：string 1 in [1, 2, 3]是false
* 搜索不会深入，只是一个一个遍历过去。

-----------

```python
def count(s, value):
    total, index = 0, 0
    while index < len(s):
        element = s[index]

        if element == value:
            total = total + 1

        index = index + 1
    return total
```

这样不够简洁。

还好我们有for循环：

```python
def count(2, value):
    total = 0
    for element in s:### 直接遍历所有
        if element == value:
            total += 1
    	return total
```

> 以上是序列迭代的一个例子

### Sequence iteration

用for循环不会引入新的框架

整体框架：

```python
for <name> in <expression>:
    <suite>
```

1. <expression>必须是一个sequence
2. 对于在sequence里的每一个element
   1. 绑定name与element
   2. 执行套件（execute the suite)

### Sequence Unpacking in For Statement

> 你实际上可以在for语句的头部直接进行序列解包

```python
pairs = [[1,2],[2,2],[3,2],[4,4]]
same_count = 0
for x, y in pairs:
    if x == y:
        same_count = same_count + 1
```

对于`for x, y in pairs`这个玩意，有很强的限制性。但是**序列解包**是好的

-----

## Range

`list`函数：列表生成器

```python
list (range(-2,2))
[-2, -1, 0, 1]
```

-------

列表推导

```python
letters = ['a','b','c','d','e','f', 'm', 'o', 'p']
[letters[i] for i in [3, 4, 6, 8]]
```

