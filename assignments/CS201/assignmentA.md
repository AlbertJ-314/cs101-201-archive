# Assignment #A: Graph starts

Updated 1830 GMT+8 Apr 22, 2025



## 1. 题目

### M19943:图的拉普拉斯矩阵

OOP, implementation, http://cs101.openjudge.cn/practice/19943/

要求创建Graph, Vertex两个类，建图实现。

代码：

```python
class Vertex:
    def __init__(self, id: int) -> None:
        self.id, self.neighbors = id, set()

    def add_neighbor(self, neighbor_id: int) -> None:
        self.neighbors.add(neighbor_id)

class Graph:
    def __init__(self, n_vertices: int) -> None:
        self.vertices = [Vertex(i) for i in range(n_vertices)]

    def add_edge(self, u: int, v: int) -> None:
        self.vertices[u].add_neighbor(v)
        self.vertices[v].add_neighbor(u)

n, m = map(int, input().split())
graph = Graph(n)
for _ in range(m):
    graph.add_edge(*map(int, input().split()))
for i in range(n):
    print(" ".join(str(len(graph.vertices[i].neighbors)) if i == j else
                   "-1" if j in graph.vertices[i].neighbors else "0" for j in range(n)))
```



代码运行截图

[![2025-04-24-16-24-05.png](https://i.postimg.cc/sfk9RnhV/2025-04-24-16-24-05.png)](https://postimg.cc/MXYj7m54)



### LC78.子集

backtracking, https://leetcode.cn/problems/subsets/

代码：

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        return [[nums[i] for i in range(len(nums)) if musk & 1 << i] for musk in range(1 << len(nums))]
```



代码运行截图

[![2025-04-24-16-28-20.png](https://i.postimg.cc/xTVHZcw8/2025-04-24-16-28-20.png)](https://postimg.cc/jCvLwddY)



### LC17.电话号码的字母组合

hash table, backtracking, https://leetcode.cn/problems/letter-combinations-of-a-phone-number/

代码：

```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        to_char = {"2": "abc", "3": "def", "4": "ghi", "5": "jkl",
                   "6": "mno", "7": "pqrs", "8": "tuv", "9": "wxyz"}
        return ["".join(s) for s in product(*[to_char[digit] for digit in digits])] if digits else []
```



代码运行截图

[![2025-04-24-16-50-32.png](https://i.postimg.cc/NjdL4M8K/2025-04-24-16-50-32.png)](https://postimg.cc/9DqWXcn2)



### M04089:电话号码

trie, http://cs101.openjudge.cn/practice/04089/

代码：

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.isTerminal = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word: str) -> bool:
        cur = self.root
        for char in word:
            if char not in cur.children:
                cur.children[char] = TrieNode()
            cur = cur.children[char]
            if cur.isTerminal:
                return False
        cur.isTerminal = True
        return not bool(cur.children)


for _ in range(int(input())):
    phone_numbers, res = Trie(), True
    for _ in range(int(input())):
        if not phone_numbers.insert(input()):
            res = False
    print("YES" if res else "NO")
```



代码运行截图

[![2025-04-24-17-16-18.png](https://i.postimg.cc/vHc6N5BW/2025-04-24-17-16-18.png)](https://postimg.cc/2VN6qbn6)



### T28046:词梯

bfs, http://cs101.openjudge.cn/practice/28046/

代码：

```python
from collections import defaultdict, deque
n = int(input())
words = [input() for _ in range(n)]
book = defaultdict(list)
for word in words:
    for i in range(4):
        book[word[:i] + "*" + word[i + 1:]].append(word)
start, end = input().split()
queue, vis = deque([start]), {start}
while queue:
    path = queue.popleft()
    cur = path[-4:]
    if cur == end:
        print(" ".join([path[i:i + 4] for i in range(0, len(path), 4)]))
        exit()
    for i in range(4):
        for word in book[cur[:i] + "*" + cur[i + 1:]]:
            if word not in vis:
                queue.append(path + word)
                vis.add(word)
print("NO")
```



代码运行截图

[![2025-04-24-16-35-59.png](https://i.postimg.cc/sfHfB8cr/2025-04-24-16-35-59.png)](https://postimg.cc/s1SCqTq6)



### T51.N皇后

backtracking, https://leetcode.cn/problems/n-queens/

代码：

```python
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        solutions = []
        def queens(row: int, cols: List[int], ldiags: Set[int], rdiags: Set[int]) -> None:
            if row == n:
                solutions.append(["".join("Q" if j == cols[i] else "." for j in range(n)) for i in range(n)])
                return
            for j in range(n):
                if j not in cols and row + j not in ldiags and row - j not in rdiags:
                    queens(row + 1, cols + [j], ldiags | {row + j}, rdiags | {row - j})

        queens(0, [], set(), set())
        return solutions
```



代码运行截图

[![2025-04-24-21-05-47.png](https://i.postimg.cc/d3gkdRXR/2025-04-24-21-05-47.png)](https://postimg.cc/GTJmnDb9)



## 2. 学习总结和收获

每日选做 + Leetcode: 128, 778, 803, 947, 959, 1202, 1631。
