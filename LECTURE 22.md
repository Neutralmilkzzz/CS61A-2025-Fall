## 链表

![image-20250914211028659](C:\Users\ZHAOKAI\AppData\Roaming\Typora\typora-user-images\image-20250914211028659.png)

```python
class Link:
    empty = ()
    def __init__(self, first, rest=empty):
        assert rest is Link.empty or isinstance(rest, link)
        self.first = first
        self.rest = rest 
link(3, Link(7, Link(5)))
        
```

---------------------

## Link List Processing

 ```python
 def range_link(start, end):
     if start >= end:
         return Link.empty
     else:
         return Link(start, range_link(start + 1, end))
 
 
 def map_link(f, s):
     """Return a Link that contains f(x) for each x in Link s."""
     if s is Link.empty:
         return s
     else:
         return Link(f(s.first), map_link(f, s.rest))
 
 
 def filter_link(f, s):
     """Return a Link that contains only the elements x of Link s for which f(x) is true."""
     if s is Link.empty:
         return s
     filtered_rest = filter_link(f, s.rest)
     if f(s.first):
         return Link(s.first, filtered_rest)
     else:
         return filtered_rest
 ### 既然链表是递归结构，用递归处理很合理。
 ```



![image-20250914215905797](C:\Users\ZHAOKAI\AppData\Roaming\Typora\typora-user-images\image-20250914215905797.png)

小明别急，我帮你一点点拆：

------

## 📌 代码部分

```python
s = Link(1, Link(2, Link(3)))
s.first = 5        # 把链表的第一个值改成 5
t = s.rest         # t 指向 s 的第二个节点
t.rest = s         # 把第二个节点的 rest 改成指回 s（形成了环！）
```

------

## 📌 图示含义

- 原本 `s = [1 -> 2 -> 3 -> empty]`

- 改完以后：

  - `s.first = 5`，所以第一个元素变成 5。

  - `t = s.rest`，所以 `t` 指向原来的 `2 -> 3`。

  - `t.rest = s`，把 `2` 的 next 改成指回 `s`，于是形成了 **环形链表**：

    ```
    5 -> 2 -> 5 -> 2 -> 5 ...
    ```

- 这就是为什么你能写一长串 `s.rest.rest.rest.rest.first`，还真的能得到结果（因为它在环里来回跑）。

------

## 📌 输出解释

- `s.first` → 5
- `s.rest.rest.rest.rest.rest.first`
  - `s.rest` 是 2
  - 再往下绕了一圈，最后指到原来的 2
  - 所以结果是 2

------

✅ **总结**
 这一页在告诉你：链表的 `first` 和 `rest` 是可以改的，改完之后链表甚至能变成环，不是像数组那样“固定结构”。

------

要不要我帮你画一个 **环形链表的小图**（比如 5→2→5→2），这样你就能一眼看懂为什么 `rest.rest.rest` 会绕回来？

---------

## Tree Class

好的，我帮你精简总结一下图里的笔记：

------

## 🌳 Tree Abstraction 精简笔记

### 1. 基本概念

- **Tree**：有一个 *root label*（根标签）和一组 *branches*（分支）。
- **Branch**：本身也是一棵树。
- **Leaf**：没有分支的树。
- **Root**：树的起点。

------

### 2. 两种描述方式

#### 🔹 递归描述（像木头树）

- 树 = 根标签 + 分支列表
- 分支也是树
- 无分支 = 叶子
- 从根开始

#### 🔹 相对描述（像家谱）

- 每个位置 = **node（节点）**
- 每个节点有一个 **label（标签）**
- 节点间有 **parent/child（父子关系）**
- 顶部节点 = **root node**

------

### 3. 关键点

- **节点 (nodes)** = 树中的位置
- **标签 (labels)** = 节点上的值
- “父母是子女之和”是常见习惯

-----

```python
class Tree:
    def __init__(self, label, branches=[]):
        self.label = label
        for branch in branches:
            assert isinstance(branch, Tree)
           self.branches = list(branches) 
        
# Tree Class
class Tree:
    def __init__(self, label, branches=[]):
        self.label = label
        for branch in branches:
            assert isinstance(branch, Tree)
        self.branches = list(branches)


def fib_tree(n):
    if n == 0 or n == 1:
        return Tree(n)
    else:
        left = fib_tree(n - 2)
        right = fib_tree(n - 1)
        fib_n = left.label + right.label
        return Tree(fib_n, [left, right])


# Tree (functional style)
def tree(label, branches=[]):
    for branch in branches:
        assert is_tree(branch)
    return [label] + list(branches)


def label(tree):
    return tree[0]


def branches(tree):
    return tree[1:]


def fib_tree(n):
    if n == 0 or n == 1:
        return tree(n)
    else:
        left = fib_tree(n - 2)
        right = fib_tree(n - 1)
        fib_n = label(left) + label(right)
        return tree(fib_n, [left, right])

```

--------

> 咱就是说，听了这个刷leetcode一头雾水哈

## dummy节点

 好的，我帮你把我们对话中你自己领悟出来的知识点提炼出来（不包含我的理解）：

------

## 你的领悟要点

1. **`__init__` 构造函数**
   - `def __init__(self, val=0, next=None):` 是标准起手式。
   - `val` 和 `next` 是两个参数，一个默认 0，一个默认 `None`。
2. **链表遍历方式**
   - 提取子链表：`l1 = l1.next`。
   - 提取当前值：`l1.val`。
   - 遍历时普遍使用 `listnode.val`、`listnode.next` 和 `while` 循环。
3. **dummy 节点**
   - 在还没有链表时，构造时先加一个 `ListNode(0)` 作为头。
   - 结果返回时用 `dummy.next`。
4. **`ListNode` 类**
   - `ListNode` 是一个类。
   - 每次调用 `ListNode(...)` 就会创造一个实例，包含 `val` 和 `next`。
   - 可以传一个参数（`ListNode(1)`），也可以传两个参数（`ListNode(1, ListNode(2))`）。
5. **链表构造**
   - 不能直接 `curr.next = total % 10`，因为那只是整数。
   - 必须用 `ListNode(total % 10)` 包装，才能挂到链表里。
6. **`curr` 指针**
   - `curr` 是一个指针变量，不是链表。
   - 用来指向链表当前节点。
   - 通过 `curr = curr.next` 往后移动，始终指向链表尾巴。

7. 链表有引用传递。

-----------

作业：

for instance:

```python
class Solution(object):
    def reverseList(self, head):
        """
        :type head: Optional[ListNode]
        :rtype: Optional[ListNode]
        """
        l = []
        while head:
            l.append(head.val)
            head = head.next
        r = l[::-1]
        dummy = ListNode(0)
        curr = dummy
        for element in r:
            curr.next = ListNode(element)
            curr = curr.next
        return dummy.next
            
```

