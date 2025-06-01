# Assignment #D: 图 & 散列表

Updated 2042 GMT+8 May 20, 2025



> **说明：**
>
> 1. **解题与记录：**
>
>    对于每一个题目，请提供其解题思路（可选），并附上使用Python或C++编写的源代码（确保已在OpenJudge， Codeforces，LeetCode等平台上获得Accepted）。请将这些信息连同显示“Accepted”的截图一起填写到下方的作业模板中。（推荐使用Typora https://typoraio.cn 进行编辑，当然你也可以选择Word。）无论题目是否已通过，请标明每个题目大致花费的时间。
>
> 2. **提交安排：**提交时，请首先上传PDF格式的文件，并将.md或.doc格式的文件作为附件上传至右侧的“作业评论”区。确保你的Canvas账户有一个清晰可见的头像，提交的文件为PDF格式，并且“作业评论”区包含上传的.md或.doc附件。
>
> 3. **延迟提交：**如果你预计无法在截止日期前提交作业，请提前告知具体原因。这有助于我们了解情况并可能为你提供适当的延期或其他帮助。 
>
> 请按照上述指导认真准备和提交作业，以保证顺利完成课程要求。



## 1. 题目

### M17975: 用二次探查法建立散列表

http://cs101.openjudge.cn/practice/17975/

代码：

```python
import sys
data = sys.stdin.read().split()
n, m = int(data[0]), int(data[1])
keys, values, hashmap = list(map(int, data[2:n + 2])), [], {}
for key in keys:
    val = key % m
    if val in hashmap and hashmap[val] != key:
        i, new_val = 1, val
        while new_val in hashmap and hashmap[new_val] != key:
            new_val = val + i * abs(i)
            i = 1 - i if i < 0 else -i
        hashmap[new_val] = key
        values.append(new_val)
    else:
        hashmap[val] = key
        values.append(val)
print(*values)
```



代码运行截图

[![2025-05-22-10-46-47.png](https://i.postimg.cc/HkrN4HKT/2025-05-22-10-46-47.png)](https://postimg.cc/pmtkPNRS)



### M01258: Agri-Net

MST, http://cs101.openjudge.cn/practice/01258/

代码：

```python
from math import inf
while True:
    try:
        n = int(input())
        graph = [list(map(int, input().split())) for _ in range(n)]
        key, parent, mst_set = [0] + [inf] * (n - 1), [None] * n, [False] * n
        for _ in range(n):
            u = min(range(n), default=-1, key=lambda v: key[v] if not mst_set[v] else inf)
            if u == -1:
                break
            mst_set[u] = True
            for v in range(n):
                if not mst_set[v] and graph[u][v] < key[v]:
                    key[v], parent[v] = graph[u][v], u
        print(sum(graph[i][parent[i]] for i in range(1, n) if parent[i] is not None))
    except EOFError:
        break
```



代码运行截图

[![2025-05-22-14-04-42.png](https://i.postimg.cc/ncnYqQTv/2025-05-22-14-04-42.png)](https://postimg.cc/6T1vXqk3)



### M3552.网络传送门旅游

bfs, https://leetcode.cn/problems/grid-teleportation-traversal/

代码：

```python
class Solution:
    def minMoves(self, matrix: List[str]) -> int:
        m, n = len(matrix), len(matrix[0])
        tele = {chr(i) : [] for i in range(ord("A"), ord("Z") + 1)}
        for i in range(m):
            for j in range(n):
                if matrix[i][j] != "." and matrix[i][j] != "#":
                    tele[matrix[i][j]].append((i, j))
        queue, vis = deque([(0, 0, 0)]), {(0, 0)}
        if matrix[0][0] in tele:
            for tx, ty in tele[matrix[0][0]]:
                if tx or ty:
                    queue.append((0, tx, ty)); vis.add((tx, ty))
            del tele[matrix[0][0]]
        while queue:
            steps, x, y = queue.popleft()
            if x == m - 1 and y == n - 1:
                return steps
            for dx, dy in ((1, 0), (0, 1), (-1, 0), (0, -1)):
                nx, ny = x + dx, y + dy
                if 0 <= nx < m and 0 <= ny < n and matrix[nx][ny] != "#" and (nx, ny) not in vis:
                    queue.append((steps + 1, nx, ny)); vis.add((nx, ny))
                    if matrix[nx][ny] in tele:
                        for tx, ty in tele[matrix[nx][ny]]:
                            if tx != nx or ty != ny:
                                queue.append((steps + 1, tx, ty)); vis.add((tx, ty))
                        del tele[matrix[nx][ny]]
        return -1
```



代码运行截图

[![2025-05-22-11-40-48.png](https://i.postimg.cc/BQhSnxRt/2025-05-22-11-40-48.png)](https://postimg.cc/rKrXhtVL)



### M787.K站中转内最便宜的航班

Bellman Ford, https://leetcode.cn/problems/cheapest-flights-within-k-stops/

代码：

```python
class Solution:
    def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, k: int) -> int:
        graph = [[] for _ in range(n)]
        for u, v, w in flights:
            graph[u].append((v, w))
        queue, vis = [(0, 0, src)], [inf] * n
        while queue:
            price, length, u = heapq.heappop(queue)
            if u == dst:
                return price
            if length >= vis[u]:
                continue
            vis[u] = length
            if length <= k:
                for v, w in graph[u]:
                    heapq.heappush(queue, (price + w, length + 1, v))
        return -1
```



代码运行截图

[![2025-05-22-11-47-03.png](https://i.postimg.cc/BvN9mssb/2025-05-22-11-47-03.png)](https://postimg.cc/t11MgKsG)



### M03424: Candies

Dijkstra, http://cs101.openjudge.cn/practice/03424/

代码：

```python
from math import inf
import heapq
n, m = map(int, input().split())
graph = [[] for _ in range(n)]
for _ in range(m):
    a, b, c = map(int, input().split())
    graph[a - 1].append((b - 1, c))
dis = [inf if i else 0 for i in range(n)]
queue, vis = [(0, 0)], set()
while queue:
    _, u = heapq.heappop(queue)
    if u in vis:
        continue
    vis.add(u)
    for v, w in graph[u]:
        if dis[v] > dis[u] + w:
            dis[v] = dis[u] + w
            heapq.heappush(queue, (dis[v], v))
print(dis[n - 1])
```



代码运行截图

[![2025-05-22-13-47-44.png](https://i.postimg.cc/c414Rs3K/2025-05-22-13-47-44.png)](https://postimg.cc/c6jSZGXZ)



### M22508:最小奖金方案

topological order, http://cs101.openjudge.cn/practice/22508/

代码：

```python
from collections import deque
from typing import List, Optional

def toposort(graph: List[List[int]]) -> Optional[List[int]]:
    indegrees = [0] * (n := len(graph))
    for u in range(n):
        for v in graph[u]:
            indegrees[v] += 1
    S, order = deque([x for x in range(n) if indegrees[x] == 0]), []
    while S:
        u = S.popleft()
        order.append(u)
        for v in graph[u]:
            indegrees[v] -= 1
            if indegrees[v] == 0:
                S.append(v)
    return order

n, m = map(int, input().split())
graph = [[] for _ in range(n)]
for _ in range(m):
    a, b = map(int, input().split())
    graph[a].append(b)
bonus = [100] * n
for u in toposort(graph)[::-1]:
    for v in graph[u]:
        bonus[u] = max(bonus[u], bonus[v] + 1)
print(sum(bonus))
```



代码运行截图

[![2025-05-22-13-01-38.png](https://i.postimg.cc/Ss6H5hcH/2025-05-22-13-01-38.png)](https://postimg.cc/zbfdb4tk)



## 2. 学习总结和收获

每日选做 ✅

寒假每日选做还剩 $3$ 道，争取期末前搞定。
