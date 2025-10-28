### 点表达式（属性查找）

- 先在 **实例对象的属性字典**里找；
- 找不到就去 **类的属性字典**里找；
- 如果类继承别的类，就会沿着 **继承链**往上查；
- 如果找到的是函数，就会绑定成方法。

> 和闭包一样啊

## Method Call

----

## Bound Method

好的，小明，我帮你把这页内容整理成简洁笔记：

------

# 📌 方法与函数 (Methods and Functions)

## 1. 区别

- **函数 (Function)**
  - 普通定义的函数。
  - 例：`Account.deposit` → `<class 'function'>`
- **绑定方法 (Bound Method)**
  - 函数 + 对象 绑定在一起。
  - 例：`tom_account.deposit` → `<class 'method'>`

------

## 2. 原理

- **对象 + 函数 = 绑定方法**
- 调用时，方法会自动把对象本身 (`self`) 作为第一个参数传入。

------

## 3. 调用方式对比

```python
# 直接调用函数，需要手动传入对象
Account.deposit(tom_account, 1001)

# 调用绑定方法，自动传入对象
tom_account.deposit(1007)
```

------

## 4. 输出结果

- `Account.deposit(tom_account, 1001)` → 1011
- `tom_account.deposit(1007)` → 2018

------

## 🎯 考点总结

1. 类中的函数本质是 **函数**；
2. 通过实例调用时，变成 **绑定方法**；
3. 方法调用时，`self` 自动传入，不需要手动写。

