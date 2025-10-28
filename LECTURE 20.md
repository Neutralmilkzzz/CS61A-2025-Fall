

# 继承 (Inheritance)

- **继承是一种将类联系在一起的方法。**
- **常见用法**：两个相似的类，只是在“专门化程度”上有所不同。
- 专门化的类（子类）可能拥有和通用类（父类）相同的属性，同时还会包含一些特殊的行为。

------

## 语法

```python
class <子类名>(<父类名>):
    <代码块>
```

------

## 概念

- 新的 **子类** 会“共享”父类的属性。
- 子类可以 **覆盖 (override)** 某些继承的属性。
- 使用继承时，我们通过定义子类与父类的差异来实现子类。

------

对，小明，你理解得对，但我帮你更精确地捋一下：

------

## 1. 子类继承父类

```python
class CheckingAccount(Account):
    withdraw_fee = 1
    interest = 0.01
    def withdraw(self, amount):
        return Account.withdraw(self, amount + self.withdraw_fee)
```

- `CheckingAccount(Account)` → `CheckingAccount` 是 `Account` 的子类。
- 子类自动继承父类里定义的所有 **方法** 和 **属性**（除非覆盖）。

------

## 2. 构造方法（`__init__`）的继承

- 在 `CheckingAccount` 里并没有自己写 `__init__`。

- 所以它会直接用父类 `Account.__init__`。

- 父类的 `__init__` 定义是：

  ```python
  def __init__(self, holder):
      self.balance = 0
      self.holder = holder
  ```

- 这就是为什么 `CheckingAccount('Tom')` 会顺理成章地调用 `Account.__init__`，把 `'Tom'` 存到 `self.holder`。

------

## 3. 子类覆盖（override）

- `CheckingAccount` 自己定义了新的 `interest`（0.01），覆盖了 `Account` 的 0.02。
- 它还重写了 `withdraw` 方法，加了 $1 手续费。
- 但 `deposit` 没重写，所以继承父类版本，保持一样。

------

## 4. 总结

- **继承**：子类没写的东西，默认用父类的。
- **构造器继承**：因为没写 `__init__`，所以直接继承 `Account.__init__`，能传 `'Tom'`。
- **覆盖**：子类里重写同名属性/方法，就会替换掉父类的。

------

要不要我帮你画一个「继承树图」，把 `Account` → `CheckingAccount` 的属性分成 **继承来的** 和 **覆盖/新增的** 两类，直观展示？

OOP的三项原则:

1. 不要重复自己，用已存在的implementations
2. 被覆写的属性在类中应该可以访问
3. 如有可能，在实例中找属性。

-------

> 何时继承？何时组合？

按照老师的说法，inheritance适合**is a**的情况

* 小明的银行账户**是**银行账户

composition适合**has a**的情况

* 一个银行**有**很多银行账户

-------

```python
def __init__(self):
    self.accounts = []

def open_account(self, holder, amount, kind=Account)：
	account = kind(holder)
    account.deposit(amount)
    self.accounts.append(account)
    return account

def pay_interest(self):
	for a in self.accounts:
        a.deposit(a.balance * a.interest)

def too_big_to_fail(self):
    return len(self)
```

--------

```python
class A:
    z = -1
    def f(self, x):
        return B(x-1)

class B(A):
    n = 4
    def __init__(self, y):
        if y:
            self.z = y
        else:
            self.z = C(y+1)

class C(B):
    def f(self, x):
        return x

```

继承与覆写。

## 多态性

多态性是一种功能（在OOP中），可以将公共接口用于多种形式（数据类型）。

假设我们需要给一个形状上色，有多个形状选项（矩形，正方形，圆形）。但是，我们可以使用相同的方法为任何形状着色。这个概念称为多态。

### 示例5：在Python中使用多态

示例

```
class Parrot:

    def fly(self):
        print("鹦鹉会飞")
    
    def swim(self):
        print("鹦鹉不会游泳")

class Penguin:

    def fly(self):
        print("企鹅不会飞")
    
    def swim(self):
        print("企鹅会游泳")

# 通用接口
def flying_test(bird):
    bird.fly()

#示例化对象
blu = Parrot()
peggy = Penguin()

# 传递对象
flying_test(blu)
flying_test(peggy)
```

当我们运行上面的程序时，输出将是：

```
鹦鹉会飞
企鹅不会飞
```

在上面的程序中，我们定义了两个类Parrot和Penguin。它们每个都有通用的fly()方法。但是，它们的功能不同。为了允许多态，我们创建了通用接口，即flying_test()可以接受任何对象的函数。然后，我们在flying_test()函数中传递了blu和peggy对象，它有效地运行了。