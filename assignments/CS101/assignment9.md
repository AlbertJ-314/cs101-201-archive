# Assignment #9: dfs, bfs, & dp

Updated 2107 GMT+8 Nov 19, 2024



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 18160: 最大连通域面积

dfs similar, http://cs101.openjudge.cn/practice/18160



代码：

```python
def dfs(x: int, y: int) -> int:
    if board[x][y] == ".":
        return 0
    res, board[x][y] = 1, "."
    for d in directions:
        if 0 <= x + d[0] < n and 0 <= y + d[1] < m:
            res += dfs(x + d[0], y + d[1])
    return res


T = int(input())
for _ in range(T):
    n, m = map(int, input().split())
    board, ans = [], 0
    for _ in range(n):
        board.append(list(input()))
    directions = [[1, 0], [1, 1], [0, 1], [-1, 1], [-1, 0], [-1, -1], [0, -1], [1, -1]]
    for i in range(n):
        for j in range(m):
            ans = max(ans, dfs(i, j))
    print(ans)
```



代码运行截图

![截屏2024-11-23 21.09.26](https://raw.githubusercontent.com/AlbertJ-314/img/main/202411232110812.png)



### 19930: 寻宝

bfs, http://cs101.openjudge.cn/practice/19930



代码：

```python
from math import inf

def dfs(x: int, y: int, vis: [[int]]) -> int:
    if graph[x][y] == 1:
        return len(vis) - 1
    res = inf
    for d in directions:
        new_x, new_y = x + d[0], y + d[1]
        if 0 <= new_x < m and 0 <= new_y < n and graph[new_x][new_y] != 2 and [new_x, new_y] not in vis:
            res = min(res, dfs(new_x, new_y, vis + [[new_x, new_y]]))
    return res


m, n = map(int, input().split())
graph = []
for _ in range(m):
    graph.append(list(map(int, input().split())))
directions = [[1, 0], [0, 1], [-1, 0], [0, -1]]
ans = dfs(0, 0, [[0, 0]])
print(ans if ans != inf else "NO")
```



代码运行截图

![截屏2024-11-23 21.11.23](https://raw.githubusercontent.com/AlbertJ-314/img/main/202411232112631.png)



### 04123: 马走日

dfs, http://cs101.openjudge.cn/practice/04123



代码：

```python
directions, cnt = [[2, 1], [1, 2], [-1, 2], [-2, 1], [-2, -1], [-1, -2], [1, -2], [2, -1]], 0

def dfs(i: int, j: int, steps: int) -> None:
    global cnt
    if steps == n * m:
        cnt += 1
    for d in directions:
        new_i, new_j = i + d[0], j + d[1]
        if 0 <= new_i < n and 0 <= new_j < m and not board[new_i][new_j]:
            board[new_i][new_j] = True
            dfs(new_i, new_j, steps + 1)
            board[new_i][new_j] = False


T = int(input())
for _ in range(T):
    n, m, x, y = map(int, input().split())
    board, cnt = [[False for _ in range(m)] for _ in range(n)], 0
    board[x][y] = True
    dfs(x, y, 1)
    print(cnt)
```



代码运行截图

![截屏2024-11-23 21.13.03](https://raw.githubusercontent.com/AlbertJ-314/img/main/202411232114431.png)



### sy316: 矩阵最大权值路径

dfs, https://sunnywhy.com/sfbj/8/1/316



代码：

```python
from math import inf

total, ans = -inf, []
directions = [[1, 0], [0, 1], [-1, 0], [0, -1]]

def dfs(x: int, y: int, vis: [[int]], cur: int) -> None:
    global total, ans
    if x == n - 1 and y == m - 1:
        if cur > total:
            total, ans = cur, vis
        return
    for d in directions:
        new_x, new_y = x + d[0], y + d[1]
        if 0 <= new_y < m and 0 <= new_x < n and [new_x, new_y] not in vis:
            dfs(new_x, new_y, vis + [[new_x, new_y]], cur + matrix[new_x][new_y])


n, m = map(int, input().split())
matrix = []
for _ in range(n):
    matrix.append(list(map(int, input().split())))
dfs(0, 0, [[0, 0]], matrix[0][0])
print("\n".join(map(lambda x: f"{x[0] + 1} {x[1] + 1}", ans)))

```



代码运行截图

![截屏2024-11-23 21.16.54](https://raw.githubusercontent.com/AlbertJ-314/img/main/202411232117957.png)



### LeetCode62.不同路径

dp, https://leetcode.cn/problems/unique-paths/



代码：

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        grid = [[1 for _ in range(n)] for _ in range(m)]
        for coord_sum in range(1, m + n - 1):
            for i in range(m):
                j = coord_sum - i
                if j < 0 or j >= n:
                    continue
                grid[i][j] = (grid[i - 1][j] if i else 0) + (grid[i][j - 1] if j else 0)
        return grid[m - 1][n - 1]
```



代码运行截图

![截屏2024-11-23 21.18.43](https://raw.githubusercontent.com/AlbertJ-314/img/main/202411232119716.png)



### sy358: 受到祝福的平方

dfs, dp, https://sunnywhy.com/sfbj/8/3/539



代码：

```python
from math import sqrt

def is_square(n: int) -> bool:
    return n > 0 and int(sqrt(n)) ** 2 == n


def check(n: int) -> bool:
    if not n:
        return False
    if is_square(n):
        return True
    k = 1
    while n > k:
        if is_square(n % k) and check(n // k):
            return True
        k *= 10
    return False


A = int(input())
print("Yes" if check(A) else "No")
```



代码运行截图

![截屏2024-11-23 21.20.17](https://raw.githubusercontent.com/AlbertJ-314/img/main/202411232120473.png)



## 2. 学习总结和收获

继续练习 dp 和记忆化搜索。

额外练习：每⽇选做的所有题，以及 LeetCode 上⼀些题⽬，⽐如 279, 322, 518, 139, 377, 638, 1137, 375, 494, 576, 1449。
