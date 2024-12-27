# Assignment #B: Dec Mock Exam大雪前一天

Updated 1649 GMT+8 Dec 5, 2024



**说明：**

1）⽉考： AC5。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### E22548: 机智的股民老张

http://cs101.openjudge.cn/practice/22548/



代码：

```python
a = list(map(int, input().split()))
n = len(a)
min_a, max_a = [a[0] for _ in range(n)], [a[-1] for _ in range(n)]
for i in range(1, n):
    min_a[i] = min(a[i], min_a[i - 1])
for i in range(n - 2, -1, -1):
    max_a[i] = max(a[i], max_a[i + 1])
ans = 0
for i in range(n):
    ans = max(ans, max_a[i] - min_a[i])
print(ans)
```



代码运行截图

![截屏2024-12-08 21.13.57](https://raw.githubusercontent.com/AlbertJ-314/img/main/202412082114726.png)



### M28701: 炸鸡排

greedy, http://cs101.openjudge.cn/practice/28701/



代码：

```python
n, k = map(int, input().split())
t = sorted(list(map(int, input().split())))
ans, s = sum(t)/k, sum(t[:n - k])
for i in range(n - k, n):
    if t[i] * (k + i - n) > s:
        ans = s/(k + i - n); break
    s += t[i]
print(f"{ans:.3f}")
```



代码运行截图

![截屏2024-12-08 21.15.29](https://raw.githubusercontent.com/AlbertJ-314/img/main/202412082116747.png)



### M20744: 土豪购物

dp, http://cs101.openjudge.cn/practice/20744/



代码：

```python
from math import inf
val = list(map(int, input().split(",")))
n = len(val)
if n == 1:
    print(val[0])
else:
    dp = [[-inf, -inf] for _ in range(n)]
    dp[0][0], dp[1][1] = val[0], val[1]
    for i in range(1, n):
        dp[i][0] = max(dp[i - 1][0], 0) + val[i]
        if i >= 2:
            dp[i][1] = max(dp[i - 1][1], dp[i - 2][0]) + val[i]
    ans = -inf
    for i in range(n):
        ans = max(ans, max(dp[i]))
    print(ans)
```



代码运行截图

![截屏2024-12-08 21.16.57](https://raw.githubusercontent.com/AlbertJ-314/img/main/202412082117598.png)



### T25561: 2022决战双十一

brute force, dfs, http://cs101.openjudge.cn/practice/25561/



代码：

```python
from math import inf

def calc(price: [int]) -> int:
    disc = 0
    for i in range(1, m + 1):
        for cp in coupons[i]:
            if price[i] >= cp[0]:
                disc += cp[1]; break
    return sum(price) - disc - (sum(price) // 300) * 50


def dfs(ind: int, rec: [int]) -> int:
    if ind >= n:
        return calc(rec)
    res = inf
    for shop in item[ind]:
        rec[shop[0]] += shop[1]
        res = min(res, dfs(ind + 1, rec))
        rec[shop[0]] -= shop[1]
    return res


n, m = map(int, input().split())

item = [[] for _ in range(n)]
for i in range(n):
    for tmp in list(input().split()):
        item[i].append(tuple(map(int, tmp.split(":"))))

coupons = [[] for _ in range(m + 1)]
for i in range(1, m + 1):
    for tmp in list(input().split()):
        coupons[i].append(tuple(map(int, tmp.split("-"))))
    coupons[i].sort(key=lambda x: x[1], reverse=True)

print(dfs(0, [0 for _ in range(m + 1)]))
```



代码运行截图

![截屏2024-12-08 21.18.59](https://raw.githubusercontent.com/AlbertJ-314/img/main/202412082119098.png)



### T20741: 两座孤岛最短距离

dfs, bfs, http://cs101.openjudge.cn/practice/20741/



代码：

```python
import heapq
from math import inf
directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]

def dfs(x: int, y:int):
    graph[x][y] = "2"
    for dx, dy in directions:
        nx, ny = x + dx, y + dy
        if 0 <= nx < n and 0 <= ny < n and graph[nx][ny] == "1":
            dfs(nx, ny)


n = int(input())
graph = [list(input()) for _ in range(n)]

flag = False
for i in range(n):
    for j in range(n):
        if graph[i][j] == "1":
            dfs(i, j)
            flag = True; break
    if flag:
        break

dis = [[inf for _ in range(n)] for _ in range(n)]
queue, vis = [], set()

for i in range(n):
    for j in range(n):
        if graph[i][j] == "1":
            dis[i][j] = 0; heapq.heappush(queue, (0, i, j))

while queue:
    _, x, y = heapq.heappop(queue)
    if (x, y) in vis:
        continue
    vis.add((x, y))
    if graph[x][y] == "2":
        print(dis[x][y] - 1); break
    for dx, dy in directions:
        nx, ny = x + dx, y + dy
        if 0 <= nx < n and 0 <= ny < n and dis[nx][ny] > dis[x][y] + 1:
            dis[nx][ny] = dis[x][y] + 1
            heapq.heappush(queue, (dis[nx][ny], nx, ny))
```



代码运行截图

![截屏2024-12-08 21.20.41](https://raw.githubusercontent.com/AlbertJ-314/img/main/202412082121450.png)



### T28776: 国王游戏

greedy, http://cs101.openjudge.cn/practice/28776



代码：

```python
n = int(input())
a, b = map(int, input().split())
nums = [tuple(map(int, input().split())) for i in range(n)]
nums.sort(key=lambda x: x[0] * x[1])
cur, ans = a, 0
for num in nums:
    ans = max(ans, cur // num[1])
    cur *= num[0]
print(ans)
```



代码运行截图

![截屏2024-12-08 21.22.08](https://raw.githubusercontent.com/AlbertJ-314/img/main/202412082122088.png)



## 2. 学习总结和收获

做了一点记忆化搜索和最短路。

额外练习：每⽇选做的所有题，以及 LeetCode 上⼀些题⽬，⽐如 87, 403, 552, 913, 329, 407, 743, 787, 1631, 1786。
