# Assignment #6: 回溯、树、双向链表和哈希表

Updated 1526 GMT+8 Mar 22, 2025



## 1. 题目

### LC46.全排列

backtracking, https://leetcode.cn/problems/permutations/

代码：

```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        return list(permutations(nums))
```



代码运行截图

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202503271248142.png)



### LC79: 单词搜索

backtracking, https://leetcode.cn/problems/word-search/

代码：

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        m, n = len(board), len(board[0])
        vis = [[False] * n for _ in range(m)]
        def dfs(x: int, y: int, idx: int) -> bool:
            if board[x][y] != word[idx]:
                return False
            if idx == len(word) - 1:
                return True
            vis[x][y] = True
            for dx, dy in ((1, 0), (0, 1), (-1, 0), (0, -1)):
                nx, ny = x + dx, y + dy
                if 0 <= nx < m and 0 <= ny < n and not vis[nx][ny]:
                    if dfs(nx, ny, idx + 1):
                        return True
            vis[x][y] = False
            return False

        for i in range(m):
            for j in range(n):
                if dfs(i, j, 0):
                    return True
        return False
```



代码运行截图

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202503271308754.png)



### LC94.二叉树的中序遍历

dfs, https://leetcode.cn/problems/binary-tree-inorder-traversal/

代码：

```python
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []
        return self.inorderTraversal(root.left) + [root.val] + self.inorderTraversal(root.right)
```



代码运行截图

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202503271252079.png)



### LC102.二叉树的层序遍历

bfs, https://leetcode.cn/problems/binary-tree-level-order-traversal/

代码：

```python
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []
        queue = deque([root])
        vis = []
        while queue:
            level_len = len(queue)
            level = []
            for _ in range(level_len):
                cur_node = queue.popleft()
                level.append(cur_node.val)
                if cur_node.left:
                    queue.append(cur_node.left)
                if cur_node.right:
                    queue.append(cur_node.right)
            vis.append(level)
        return vis
```



代码运行截图

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202503271253637.png)



### LC131.分割回文串

dp, backtracking, https://leetcode.cn/problems/palindrome-partitioning/

代码：

```python
class Solution:
    @cache
    def partition(self, s: str) -> List[List[str]]:
        if s == "":
            return [[]]
        res = []
        for i in range(len(s)):
            if s[:i + 1] == s[:i + 1][::-1]:
                for par in self.partition(s[i + 1:]):
                    res.append([s[:i + 1]] + par)
        return res
```



代码运行截图

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202503271323146.png)



### LC146.LRU缓存

hash table, doubly-linked list, https://leetcode.cn/problems/lru-cache/

代码：

```python
class Node:
    def __init__(self, key: int, val: int, prev=None, next=None):
        self.key, self.val = key, val
        self.prev, self.next = prev, next

class LRUCache:
    def __init__(self, capacity: int):
        self.head, self.tail = Node(0, 0), Node(0, 0)
        self.head.next = self.tail
        self.tail.prev = self.head
        self.cache = {}
        self.capacity = capacity

    def get(self, key: int) -> int:
        if key in self.cache:
            self.put(key, self.cache[key].val)
            return self.cache[key].val
        return -1

    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            node = self.cache[key].prev
            node.next = node.next.next
            node.next.prev = node
        node = Node(key, value, self.tail.prev, self.tail)
        self.tail.prev.next = self.tail.prev = node
        self.cache[key] = node
        if len(self.cache) > self.capacity:
            del self.cache[self.head.next.key]
            self.head.next = self.head.next.next
            self.head.next.prev = self.head
```



代码运行截图

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202503271255651.png)



## 2. 学习总结和收获

每日选做按进度完成，正在补寒假的选做。
