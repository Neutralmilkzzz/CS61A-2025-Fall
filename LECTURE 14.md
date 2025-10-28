# LECTURE 14

# Tree

> 欧耶终于学到树了！！！！！！开心！！！撒花！！！

有两种表示

1. Recursive description(wooden trees):
   1. A tree has a root label and a list of branches
   2. Each branch is a tree
   3. A tree with zero branches is called a leaf

2. Relative description (family tree):
   1. Each location in a tree is called a node
   2. Each node has a label that can be any value
   3. One node can be the parent/child of another

代码实现：

```python
tree(3, [tree(1), tree(2,[tree(1),tree(1)])])
def tree(label, branches=[]):
    for branch in branches:
        assert is_tree(branch)
    return [label] + list(branches)
    return [label] + branches

def label(tree):
    return tree[0]

def branches(tree):
    return[1:]

def is_tree(tree):
    if 
```

这不对。

----------

Chatgpt：

