# Recursion

## Self-Reference

```python
def print_all(x):
    print(x)
    return print_all
###由于它指向自身却没有引用自身，它不会无限循环
>>> print_all(1)(3)(5)
>>>1
>>>3
>>>5
```

> `fn.__name__`是用来访问函数或方法的名称的方式

```python
def print_sums(x):
    print(x)
    def next_sum(y):
        return print_sums(x+y)
    return next_sum

>>> print_sums(1)(3)(5)
```

----------

## Recursive Functions

Definition： A function is called *recursive* if the body of that function calls itself, either directly or indirectly. 

e.g:计算数字各位数的和

```python
def split(n):
    return n // 10, n % 10

def sum_digits(n):
    if n < 10:
        return n
    else:
        all_but_last, last = split(n)
        return sum_digits(all_but_last) + last

```

---------

## Recursion in Environment Diagrams

e.g:Factorial function

```python
def facto(n):
    if n == 0:
        return 1
    else:
        return n * fact(n-1)
```

-----

## Difference between iteration and recursion

Iteration is **a special use** of recursion

FOR EXAMPLE，when dealing with factorial, we have two solutions.

1. While

``` python
def fact_iter(n):
    total, k = 1, 1
    while k <= n:
		total, k = total * n, k + 1
    return total
```

2. Recursion

```python
def fact(n):
    if n == 0:
        return 1
    else:
        return n * fact(n-1)
```

----------

## Mutual Recursion

> 终于可以实现Luhn Algorithm了

```python
def split(n);
    return n // 10, n % 10

def sum_digits(n):
    if n < 10:
        return n
    else:
        all_but_last. last = split(n)
        return sum_digits(all_but_last) + last

def luhn_sum(n):
    if n < 10:
        return n
    else:
        all_but_last, last = split(n)
        return luhn_sum_double(all_but_last) + last

def luhn_sum_double(n):
    all_but_last, last = split(n)
    luhn_digit = sum_digits(2 * last)
    if n < 10:
        return luhn_digit
    else:
        return luhn_sum(all_but_last) + luhn_digit
```

-------------

## Recursion and Iteration

