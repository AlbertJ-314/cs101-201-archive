## Tree traversal

#### Recursive

```python
from typing import List, Optional

class Node:
    def __init__(self, val: Optional[int] = None, children: Optional[List['Node']] = None):
        self.val = val
        self.children = children

def preorder(root: 'Node') -> List[int]:
    if not root:
        return []
    res = [root.val]
    for child in root.children:
        res.extend(preorder(child))
    return res

def postorder(root: 'Node') -> List[int]:
    if not root:
        return []
    res = []
    for child in root.children:
        res.extend(postorder(child))
    return res + [root.val]
```



#### Stack

```python
from typing import List, Optional

class Node:
    def __init__(self, val: Optional[int] = None, children: Optional[List['Node']] = None):
        self.val = val
        self.children = children

def preorder(root: 'Node') -> List[int]:
    if not root:
        return []
    res, stack = [], [root]
    while stack:
        node = stack.pop()
        res.append(node.val)
        stack.extend(node.children[::-1])
    return res

def postorder(root: 'Node') -> List[int]:
    if not root:
        return []
    # Hold the nodes in reverse order
    res, stack = [], [root]
    while stack:
        node = stack.pop()
        res.append(node.val)
        stack.extend(node.children)
    return res[::-1]
```



## Binary tree inorder traversal

#### Recursive

```python
from typing import List, Optional

class BinNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def inorder(root: Optional[BinNode]) -> List[int]:
    return inorder(root.left) + [root.val] + inorder(root.right) if root else []
```



#### Stack

```python
from typing import List, Optional

class BinNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def inorder(root: Optional[BinNode]) -> List[int]:
    if not root:
        return []
    res, stack, node = [], [], root
    while node or stack:
      	# Go as left as possible
        while node:
            stack.append(node)
            node = node.left
        node = stack.pop()
        res.append(node.val)
        node = node.right
    return res
```



#### Level order

TBC