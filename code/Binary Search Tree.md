## Binary Search Tree

```python
from collections.abc import Iterator

class BinNode:
    def __init__(self, val: int | None = None,
                 left: 'BinNode' | None = None,
                 right: 'BinNode' | None = None):
        self.val = val
        self.left = left
        self.right = right

class BinarySearchTree:
    def __init__(self):
        self.root = None
```



#### Insert

```python
    def insert(self, val: int) -> None:
        if self.root is None:
            self.root = BinNode(val)
        else:
            self.root = self._insert(self.root, val)

    def _insert(self, node: BinNode, val: int) -> BinNode:
        if val < node.val:
            node.left = BinNode(val) if node.left is None else self._insert(node.left, val)
        else:
            node.right = BinNode(val) if node.right is None else self._insert(node.right, val)
        return node
```

Time complexity: $O\left(\log n\right)$ on average, $O\left(n\right)$ in the worst case. Here $n$ is the number of nodes.

Space complexity: $O\left(\log n\right)$ on average, $O\left(n\right)$ in the worst case.



#### Delete

```python
    def delete(self, val: int) -> None:
        if self.root is not None:
            self.root = self._delete(self.root, val)

    def _delete(self, node: BinNode | None, val: int) -> BinNode | None:
        if node is None:
            return None

        if val < node.val:
            node.left = self._delete(node.left, val)
        elif val > node.val:
            node.right = self._delete(node.right, val)
        else:
            if node.left is None:
                return node.right
            if node.right is None:
                return node.left

            curr = node.right
            while curr.left:
                curr = curr.left
            node.val = curr.val
            node.right = self._delete(node.right, node.val)

        return node
```

Time complexity: $O\left(\log n\right)$ on average, $O\left(n\right)$ in the worst case.

Space complexity: $O\left(\log n\right)$ on average, $O\left(n\right)$ in the worst case.



#### Search

```python
    def search(self, val: int) -> bool:
        return self._search(self.root, val)

    def _search(self, node: BinNode | None, val: int) -> bool:
        if node is None:
            return False
        if node.val == val:
            return True
        return self._search(node.left if val < node.val else node.right, val)
```

Time complexity: $O\left(\log n\right)$ on average, $O\left(n\right)$ in the worst case.

Space complexity: $O\left(\log n\right)$ on average, $O\left(n\right)$ in the worst case.



#### Inorder traversal

```python
    def inorder(self) -> list[int]:
        return list(self._inorder(self.root))

    def _inorder(self, node: BinNode | None) -> Iterator[int]:
        if node is not None:
            yield from self._inorder(node.left)
            yield node.val
            yield from self._inorder(node.right)
```

Time complexity: $O\left(n\right)$.

Space complexity: $O\left(\log n\right)$ on average, $O\left(n\right)$ in the worst case.



#### Find minimum / maximum value

```python
    def find_min(self) -> int:
        if self.root is None:
            raise ValueError("Tree is empty")
        curr = self.root
        while curr.left:
            curr = curr.left
        return curr.val

    def find_max(self) -> int:
        if self.root is None:
            raise ValueError("Tree is empty")
        curr = self.root
        while curr.right:
            curr = curr.right
        return curr.val
```

Time complexity: $O\left(\log n\right)$ on average, $O\left(n\right)$ in the worst case.

Space complexity: $O\left(1\right)$.
