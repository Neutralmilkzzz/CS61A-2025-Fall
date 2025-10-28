# lecture 18

## OOP

好嘞，我帮你整理成极简+ nerd 学霸风格笔记 👇

------

# OOP 基础笔记

## 定义与作用

- **OOP**: 一种组织程序的方法
- **核心**：数据抽象升级版，把**信息+行为**捆绑在一起

## 核心隐喻

- 每个 **对象 object** 有自己本地状态
- 对象能管理自己的状态
- 与对象交互靠 **方法 method**
- 多个对象可以是同一个 **类 class** 的实例
- 不同类之间也能有关系

## 基础概念

- **class**: 定义某类对象的行为方式
- **object**: 类的实例，类是它的类型
- **method**: 定义在类里的函数，用 `.` 调用，比如 `s.append(5)`

------

# 列表（List）举例

## 基础

- Python 内置 `list` 是一个 **类 class**
- 调用 `list(iterable)` 会创建一个 **list 对象**

```python
s = list(range(3))   # [0,1,2]
type(s)              # <class 'list'>
```

## list 类的能力

- **方法**：`append, extend, insert, pop, remove, …`
- **运算**：加法、乘法 → `s + t`, `s * n`
- **索引/赋值**：`s[i]`, `s[i] = x`

------

📌 一句话总结（学霸腔）：
 **类是模板 → 对象是产物 → 方法是工具**
 List 就是 Python 自带的“工厂”，造出来的对象能存数据，还能调用一堆方法玩。

------

我的总结：

# OOP 核心框架

- **类 class**：规则/模板，规定对象该有的属性和行为
- **对象 object**：类的实例，真正的“个体”，遵循类的规则
- **属性 attributes**：对象自带的数据（状态），比如账户余额、用户名
- **方法 methods**：类中定义的函数，用来操作对象，比如存款、取款

如何构造一个类：

好嘞 👍 我给你一个 **构造类（class）复习大纲**，简洁又全面，适合速背：

------

# 构造类（Class）复习大纲

## 1. 基本概念

- **类（class）**：对象的模板，定义规则（属性+方法）
- **对象（object）**：类的实例，每个对象都有独立数据
- **属性（attributes）**：对象自带的数据（状态）
- **方法（methods）**：类中定义的函数，用于操作对象

------

## 2. 定义类

```python
class ClassName:
    def __init__(self, 参数…):
        self.属性 = 值   # 初始化对象
    def 方法(self, 参数…):
        # 定义行为
```

------

## 3. `__init__` 方法

- 构造方法，创建对象时自动调用
- `self` → 当前对象本身
- 用来初始化对象的属性

------

## 4. 方法与属性

- **方法**：必须带 `self` 作为第一个参数
- **属性**：通过 `self.属性名` 绑定在对象上
- 调用方法：`对象.方法(参数)`
- 访问属性：`对象.属性`

------

## 5. 示例：银行账户

```python
class Account:
    def __init__(self, holder):
        self.holder = holder
        self.balance = 0

    def deposit(self, amount):
        self.balance += amount
        return self.balance

    def withdraw(self, amount):
        if amount > self.balance:
            return 'Insufficient funds'
        self.balance -= amount
        return self.balance
```

------

## 6. 使用

```python
a = Account('John')  # 创建对象
a.holder    # 'John'
a.deposit(100)  # 存钱
a.withdraw(30)  # 取钱
```

------

📌 **一句话速记**：
 **类定规矩 → `__init__` 定出生属性 → self 指自己 → 方法改状态 → 对象照规矩**

------

要不要我帮你再加一栏 **高频考点坑点（如 self 必须写、方法必须有 self、属性只能在 init 初始化等）**，方便你考前秒扫？

👌 我帮你把这一页也整理成极简复习笔记：

------

# 对象标识（Object Identity）

## 1. 唯一性

- 每次调用类都会生成一个 **新对象实例**
- 类只有一个，但对象可以有多个

```python
a = Account('John')
b = Account('Jack')
a is b   # False
```

------

## 2. 身份运算符

- **`is`**：判断两个变量是否引用同一个对象
- **`is not`**：判断是否不是同一个对象

```python
a is a       # True
a is not b   # True
```

------

## 3. 变量绑定

- 给对象绑定新名字 **不会创建新对象**
- 只是多了个引用

```python
c = a
c is a   # True
```

------

## 速记口诀

- **类唯一，对象多**
- **`is` 看身份，`==` 看值**
- **赋新名 ≠ 新对象**

------

要不要我帮你把 **对象标识 vs 对象相等** (`is` vs `==`) 的区别画成一个小对照表？