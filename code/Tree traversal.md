## Tree traversal

```python
from collections.abc import Iterator

class TreeNode:
    def __init__(self, val: int | None = None, children: list['TreeNode'] | None = None ):
        self.val = val
        self.children = children or []
```



#### Preorder and postorder

###### Recursive

```python
def preorder(root: TreeNode | None) -> Iterator[int]:
    if root is None:
        return
    yield root.val
    for child in root.children:
        yield from preorder(child)

def postorder(root: TreeNode | None) -> Iterator[int]:
    if root is None:
        return
    for child in root.children:
        yield from postorder(child)
    yield root.val
```



#### Preorder and postorder

###### Iterative

```python
def preorder(root: TreeNode | None) -> list[int]:
    if root is None:
        return []
    res, stack = [], [root]
    while stack:
        node = stack.pop()
        res.append(node.val)
        # Reverse children to maintain left-to-right order
        stack.extend(node.children[::-1])
    return res

def postorder(root: TreeNode | None) -> list[int]:
    if root is None:
        return []
    res, stack = [], [root]
    while stack:
        node = stack.pop()
        res.append(node.val)
        # Children are added left to right; reversal happens at the end
        stack.extend(node.children)
    return res[::-1]
```



#### Level order

```python
from collections import deque

def levelorder(root: TreeNode | None) -> list[list[int]]:
    if root is None:
        return []
    queue, res = deque([root]), []
    while queue:
        level_size, level = len(queue), []
        for _ in range(level_size):
            node = queue.popleft()
            level.append(node.val)
            queue.extend(node.children)
        res.append(level)
    return res
```



#### Inorder traversal for binary trees

```python
from collections.abc import Iterator

class BinNode:
    def __init__(self, val: int | None = None,
                 left: 'BinNode' | None = None,
                 right: 'BinNode' | None = None):
        self.val = val
        self.left = left
        self.right = right
```



###### Recursive

```python
def inorder(root: BinNode | None) -> Iterator[int]:
    if root is None:
        return
    yield from inorder(root.left)
    yield root.val
    yield from inorder(root.right)
```



###### Iterative

```python
def inorder(root: BinNode | None) -> list[int]:
    if root is None:
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
