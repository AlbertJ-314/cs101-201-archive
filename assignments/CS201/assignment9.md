# Assignment #9: Huffman, BST & Heap

Updated 1834 GMT+8 Apr 15, 2025



## 1. 题目

### LC222.完全二叉树的节点个数

dfs, https://leetcode.cn/problems/count-complete-tree-nodes/

代码：

```python
class Solution:
    def countNodes(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        curr, height = root, -1
        while curr:
            height += 1
            curr = curr.left
        l, r = 0, (1 << height) - 1
        while l < r:
            mid, curr = (l + r + 1) // 2, root
            for i in range(height - 1, -1, -1):
                curr = curr.right if mid & (1 << i) else curr.left
            if curr:
                l = mid
            else:
                r = mid - 1
        return (1 << height) + l
```



代码运行截图

[![2025-04-17-13-24-44.png](https://i.postimg.cc/wjG9WBhy/2025-04-17-13-24-44.png)](https://postimg.cc/tsF0Jpvp)



### LC103.二叉树的锯齿形层序遍历

bfs, https://leetcode.cn/problems/binary-tree-zigzag-level-order-traversal/

代码：

```python
class Solution:
    def zigzagLevelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []
        queue, vis = deque([root]), []
        while queue:
            level_len, level = len(queue), []
            for _ in range(level_len):
                curr = queue.popleft()
                level.append(curr.val)
                if curr.left:
                    queue.append(curr.left)
                if curr.right:
                    queue.append(curr.right)
            vis.append(level[::-1] if len(vis) % 2 else level)
        return vis
```



代码运行截图

[![2025-04-17-13-22-10.png](https://i.postimg.cc/c4GgmFsy/2025-04-17-13-22-10.png)](https://postimg.cc/NLbjG79p)



### M04080:Huffman编码树

greedy, http://cs101.openjudge.cn/practice/04080/

代码：

```python
import heapq
n, heap, s = int(input()), list(map(int, input().split())), 0
heapq.heapify(heap)
for _ in range(n - 1):
    a, b = heapq.heappop(heap), heapq.heappop(heap)
    s += a + b; heapq.heappush(heap, a + b)
print(s)
```



代码运行截图

[![2025-04-17-13-25-57.png](https://i.postimg.cc/cL2NRbSf/2025-04-17-13-25-57.png)](https://postimg.cc/DSrDF5mz)



### M05455: 二叉搜索树的层次遍历

http://cs101.openjudge.cn/practice/05455/

代码：

```python
from collections import deque
from typing import List, Optional

class BinNode:
    def __init__(self, val: Optional[int] = None, left: Optional['BinNode'] = None, right: Optional['BinNode'] = None):
        self.val = val
        self.left, self.right = left, right

class BinarySearchTree:
    def __init__(self):
        self.root = None

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

    def levelorder(self) -> List[int]:
        if not self.root:
            return []
        queue, res = deque([self.root]), []
        while queue:
            level_size = len(queue)
            for _ in range(level_size):
                node = queue.popleft()
                if node:
                    res.append(node.val)
                    queue.extend([node.left, node.right])
        return res

bst, book = BinarySearchTree(), set()
for num in map(int, input().split()):
    if num not in book:
        bst.insert(num)
        book.add(num)
print(" ".join(map(str, bst.levelorder())))
```



代码运行截图

[![2025-04-17-13-27-29.png](https://i.postimg.cc/mDGfq1hD/2025-04-17-13-27-29.png)](https://postimg.cc/941nwMd5)



### M04078: 实现堆结构

手搓实现，http://cs101.openjudge.cn/practice/04078/

类似的题目是 晴问9.7: 向下调整构建大顶堆，https://sunnywhy.com/sfbj/9/7

代码：

```python
class MyHeap:
    def __init__(self):
        self.heap = []
        self.size = 0

    def push(self, x: int) -> None:
        self.heap.append(x); self.size += 1
        i = self.size - 1
        while i > 0:
            j = (i - 1) // 2
            if self.heap[j] > self.heap[i]:
                self.heap[i], self.heap[j] = self.heap[j], self.heap[i]
                i = j
            else:
                break

    def pop(self) -> int:
        self.heap[0], self.heap[-1] = self.heap[-1], self.heap[0]
        res = self.heap.pop(); self.size -= 1
        i = 0
        while 2 * i + 1 < self.size:
            j = 2 * i + 1
            if 2 * i + 2 < self.size and self.heap[j] > self.heap[2 * i + 2]:
                j = 2 * i + 2
            if self.heap[i] > self.heap[j]:
                self.heap[i], self.heap[j] = self.heap[j], self.heap[i]
                i = j
            else:
                break
        return res

heap = MyHeap()
for _ in range(int(input())):
    op = list(map(int, input().split()))
    if op[0] == 1:
        heap.push(op[1])
    else:
        print(heap.pop())
```



代码运行截图

[![2025-04-17-13-29-02.png](https://i.postimg.cc/XvdtwNx0/2025-04-17-13-29-02.png)](https://postimg.cc/CZMv0gWv)



### T22161: 哈夫曼编码树

greedy, http://cs101.openjudge.cn/practice/22161/

代码：

```python
from collections import defaultdict
import heapq

n = int(input())
heap = []
for _ in range(n):
    char, weight = input().split()
    heapq.heappush(heap, (int(weight), char))

encode = defaultdict(str)
while len(heap) > 1:
    left, right = heapq.heappop(heap), heapq.heappop(heap)
    for char in left[1]:
        encode[char] += "0"
    for char in right[1]:
        encode[char] += "1"
    heapq.heappush(heap, (left[0] + right[0], min(left[1] + right[1], right[1] + left[1])))

decode = {}
for char, code in encode.items():
    encode[char] = code[::-1]
    decode[code[::-1]] = char

while True:
    try:
        s = input()
        if s.isalpha():
            print("".join(encode[c] for c in s))
        else:
            i, j = 0, 0
            while j <= len(s):
                if s[i:j] in decode:
                    print(decode[s[i:j]], end="")
                    i = j
                j += 1
            print()
    except EOFError:
        break
```



代码运行截图

[![2025-04-17-13-30-16.png](https://i.postimg.cc/QtqPy6R4/2025-04-17-13-30-16.png)](https://postimg.cc/ftJB3vWm)



## 2. 学习总结和收获

每日选做 + LeetCode: 208, 211, 212, 336, 421, 425, 440, 642, 648, 676, 677, 1023。
