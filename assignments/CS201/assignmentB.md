# Assignment #B: 图为主

Updated 2223 GMT+8 Apr 29, 2025



## 1. 题目

### E07218:献给阿尔吉侬的花束

bfs, http://cs101.openjudge.cn/practice/07218/

代码：

```python
from collections import deque
for _ in range(int(input())):
    r, c = map(int, input().split())
    maze, start, end = [input() for _ in range(r)], None, None
    for i in range(r):
        for j in range(c):
            if maze[i][j] == "S":
                start = (i, j)
            elif maze[i][j] == "E":
                end = (i, j)
    queue, vis, flag = deque([(start, 0)]), {start}, False
    while queue:
        (x, y), steps = queue.popleft()
        if (x, y) == end:
            print(steps); flag = True
        for dx, dy in ((0, 1), (1, 0), (0, -1), (-1, 0)):
            nx, ny = x + dx, y + dy
            if 0 <= nx < r and 0 <= ny < c and maze[nx][ny] != "#" and (nx, ny) not in vis:
                queue.append(((nx, ny), steps + 1))
                vis.add((nx, ny))
    if not flag:
        print("oop!")
```



代码运行截图

[![2025-05-01-16-29-03.png](https://i.postimg.cc/8kWQN8ww/2025-05-01-16-29-03.png)](https://postimg.cc/NLgPkz3r)



### M3532.针对图的路径存在性查询I

disjoint set, https://leetcode.cn/problems/path-existence-queries-in-a-graph-i/

代码：

```python
class Solution:
    def pathExistenceQueries(self, n: int, nums: List[int], maxDiff: int, queries: List[List[int]]) -> List[bool]:
        l, r, idx = 0, 0, []
        while r < n:
            if r < n - 1 and nums[r + 1] - nums[r] <= maxDiff:
                r += 1
            else:
                idx.extend([r] * (r - l + 1))
                l = r = r + 1
        for index, (i, j) in enumerate(queries):
            queries[index] = idx[min(i, j)] >= max(i, j)
        return queries
```



代码运行截图

[![2025-05-01-16-40-58.png](https://i.postimg.cc/tCKs7HHT/2025-05-01-16-40-58.png)](https://postimg.cc/56m4kTyd)



### M22528:厚道的调分方法

binary search, http://cs101.openjudge.cn/practice/22528/

代码：

```python
from math import ceil
scores = list(map(float, input().split()))
x, l, r = sorted(scores, reverse=True)[ceil(len(scores) * 0.6) - 1], 1, 1000000000
while l < r:
    b = (l + r) // 2
    if b * x / 1000000000 + 1.1 ** (b * x / 1000000000) >= 85:
        r = b
    else:
        l = b + 1
print(l)
```



代码运行截图

[![2025-05-01-16-49-32.png](https://i.postimg.cc/3Ngxvkhx/2025-05-01-16-49-32.png)](https://postimg.cc/K38bXv46)



### Msy382: 有向图判环 

dfs, https://sunnywhy.com/sfbj/10/3/382

代码：

```python
n, m = map(int, input().split())
graph = [[] for _ in range(n)]
for _ in range(m):
    u, v = map(int, input().split())
    graph[u].append(v)
vis, curr, cycle = set(), set(), False
def dfs(u: int) -> None:
    global cycle
    if u in curr:
        cycle = True
    if cycle or u in vis:
        return
    vis.add(u); curr.add(u)
    for v in graph[u]:
        dfs(v)
    curr.remove(u)

for u in range(n):
    if u not in vis:
        dfs(u)
print("Yes" if cycle else "No")
```



代码运行截图

[![2025-05-01-17-19-22.png](https://i.postimg.cc/5tYgMJ1m/2025-05-01-17-19-22.png)](https://postimg.cc/t1bWhLs1)



### M05443:兔子与樱花

Dijkstra, http://cs101.openjudge.cn/practice/05443/

代码：

```python
from copy import deepcopy
from math import inf
n = int(input())
names = [input() for _ in range(n)]
index = {name : i for i, name in enumerate(names)}
graph = [[inf if i != j else 0 for j in range(n)] for i in range(n)]
for _ in range(int(input())):
    a, b, l = input().split()
    graph[index[a]][index[b]] = int(l)
    graph[index[b]][index[a]] = int(l)
dis, paths = deepcopy(graph), [[[] for _ in range(n)] for _ in range(n)]
for i in range(n):
    for j in range(n):
        if graph[i][j] != inf:
            paths[i][j] = [i, j] if i != j else [i]
for k in range(n):
    for x in range(n):
        for y in range(n):
            if dis[x][k] + dis[k][y] < dis[x][y]:
                dis[x][y] = dis[x][k] + dis[k][y]
                paths[x][y] = paths[x][k][:-1] + paths[k][y]
for _ in range(int(input())):
    a, b = input().split()
    path, res = paths[index[a]][index[b]], [a]
    for i in range(1, len(path)):
        res.extend([f"({graph[path[i - 1]][path[i]]})", names[path[i]]])
    print("->".join(res))
```



代码运行截图

[![2025-05-01-12-51-45.png](https://i.postimg.cc/HLWs8vJF/2025-05-01-12-51-45.png)](https://postimg.cc/bZMjKT9R)



### T28050: 骑士周游

dfs, http://cs101.openjudge.cn/practice/28050/

代码：

```python
directions = [(-2, -1), (-2, 1), (-1, -2), (-1, 2), (1, -2), (1, 2), (2, -1), (2, 1)]

def knight_tour(n: int, sx: int, sy: int) -> bool:
    vis = [[False for _ in range(n)] for _ in range(n)]
    vis[sx][sy] = True

    def degree(x: int, y: int) -> int:
        cnt = 0
        for dx, dy in directions:
            nx, ny = x + dx, y + dy
            if 0 <= nx < n and 0 <= ny < n and not vis[nx][ny]:
                cnt += 1
        return cnt

    def Warnsdorf(x: int, y: int, steps: int) -> bool:
        if steps == n * n:
            return True
        moves = []
        for dx, dy in directions:
            nx, ny = x + dx, y + dy
            if 0 <= nx < n and 0 <= ny < n and not vis[nx][ny]:
                moves.append((degree(nx, ny), nx, ny))
        for _, nx, ny in sorted(moves):
            vis[nx][ny] = True
            if Warnsdorf(nx, ny, steps + 1):
                return True
            vis[nx][ny] = False
        return False

    return Warnsdorf(sx, sy, 1)

N = int(input())
SR, SC = map(int, input().split())
print("success" if knight_tour(N, SR, SC) else "fail")
```



代码运行截图

[![2025-05-01-16-51-36.png](https://i.postimg.cc/VNdsFGwT/2025-05-01-16-51-36.png)](https://postimg.cc/mcWGLStN)



## 2. 学习总结和收获

完成每日选做，即将开始做陈斌老师数算B的[阿瓦隆大作业](https://github.com/pkulab409/pkudsa.avalon)（因为选了陈老师另一门课，大作业也是这个🤪）。
