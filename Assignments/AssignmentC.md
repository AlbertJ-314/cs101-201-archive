# Assignment #C: 五味杂陈 

Updated 1148 GMT+8 Dec 10, 2024



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 1115. 取石子游戏

dfs, https://www.acwing.com/problem/content/description/1117/



代码：

```python
def game(a: int, b: int) -> bool:
    if not b:
        return False
    if a // b >= 2:
        return True
    return not game(b, a - b)


while True:
    a, b = map(int, input().split())
    if not a and not b:
        break
    print("win" if game(max(a, b), min(a, b)) else "lose")
```



代码运行截图

![截屏2024-12-14 19.53.06](https://raw.githubusercontent.com/AlbertJ-314/img/main/202412141953579.png)



### 25570: 洋葱

Matrices, http://cs101.openjudge.cn/practice/25570



代码：

```python
n = int(input())
matrix = [list(map(int, input().split())) for _ in range(n)]
s = [0 for _ in range(n)]
for i in range(n):
    for j in range(n):
        s[max(abs(2 * i - n + 1), abs(2 * j - n + 1)) // 2] += matrix[i][j]
print(max(s))
```



代码运行截图

![截屏2024-12-14 19.55.14](https://raw.githubusercontent.com/AlbertJ-314/img/main/202412141955087.png)



### 1526C1. Potions(Easy Version)

greedy, dp, data structures, brute force, *1500, https://codeforces.com/problemset/problem/1526/C1



代码：

```python
import heapq
n = int(input())
rec, h = [], 0
for potion in list(map(int,input().split())):
    h += potion
    heapq.heappush(rec, potion)
    if h < 0:
        h -= heapq.heappop(rec)
print(len(rec))
```



代码运行截图

![截屏2024-12-14 19.57.01](https://raw.githubusercontent.com/AlbertJ-314/img/main/202412141957663.png)



### 22067: 快速堆猪

辅助栈，http://cs101.openjudge.cn/practice/22067/



代码：

```python
stack, min_stack = [], []
while True:
    try:
        op = input()
        if op == "pop":
            if stack:
                stack.pop(); min_stack.pop()
        elif op == "min":
            if stack:
                print(min_stack[-1])
        else:
            w = int(op.split()[1])
            stack.append(w)
            min_stack.append(min(min_stack[-1], w) if min_stack else w)
    except EOFError:
        break
```



代码运行截图

![截屏2024-12-14 19.58.29](https://raw.githubusercontent.com/AlbertJ-314/img/main/202412141959651.png)



### 20106: 走山路

Dijkstra, http://cs101.openjudge.cn/practice/20106/



代码：

```python
from collections import defaultdict
import heapq
from math import inf

directions = [(1, 0), (0, 1), (-1, 0), (0, -1)]
def dijkstra(start: (int, int)) -> [int]:
    dis = defaultdict(lambda: inf)
    dis[start] = 0
    queue, vis = [(0, start)], set()
    while queue:
        _, u = heapq.heappop(queue)
        if u in vis:
            continue
        vis.add(u)
        for d in directions:
            v = (u[0] + d[0], u[1] + d[1])
            if 0 <= v[0] < m and 0 <= v[1] < n:
                if dis[v] > dis[u] + abs(h[u[0]][u[1]] - h[v[0]][v[1]]):
                    dis[v] = dis[u] + abs(h[u[0]][u[1]] - h[v[0]][v[1]])
                    heapq.heappush(queue, (dis[v], v))
    return dis

m, n, p = map(int, input().split())
h = [list(map(lambda x: int(x) if x != "#" else inf, input().split())) for _ in range(m)]
for _ in range(p):
    sx, sy, tx, ty = map(int, input().split())
    res = dijkstra((sx, sy))[(tx, ty)]
    print(res if res != inf else "NO")
```



代码运行截图

![截屏2024-12-14 19.59.42](https://raw.githubusercontent.com/AlbertJ-314/img/main/202412142000755.png)



### 04129: 变换的迷宫

bfs, http://cs101.openjudge.cn/practice/04129/



代码：

```python
from collections import deque
directions = [(1, 0), (0, 1), (-1, 0), (0, -1)]
T = int(input())
for _ in range(T):
    m, n, K = map(int, input().split())
    maze = [list(input()) for _ in range(m)]
    sx, sy, ex, ey = 0, 0, 0, 0
    for i in range(m):
        for j in range(n):
            if maze[i][j] == "S":
                sx, sy = i, j
            if maze[i][j] == "E":
                ex, ey = i, j
    queue, vis = deque([(0, sx, sy)]), {(0, sx, sy)}
    flag = False
    while queue:
        time, x, y = queue.popleft()
        if x == ex and y == ey:
            print(time); flag = True; break
        for dx, dy in directions:
            nx, ny = x + dx, y + dy
            if 0 <= nx < m and 0 <= ny < n and ((time + 1) % K == 0 or maze[nx][ny] != "#"):
                if ((time + 1) % K, nx, ny) not in vis:
                    vis.add(((time + 1) % K, nx, ny))
                    queue.append((time + 1, nx, ny))
    if not flag:
        print("Oop!")
```



代码运行截图

![截屏2024-12-14 20.02.05](https://raw.githubusercontent.com/AlbertJ-314/img/main/202412142003160.png)



## 2. 学习总结和收获

刷点简单的 dp。

额外练习：每⽇选做的所有题，以及 LeetCode 上⼀些题⽬，⽐如 1143, 712, 718, 583, 72, 44, 10, 97, 115, 87。
