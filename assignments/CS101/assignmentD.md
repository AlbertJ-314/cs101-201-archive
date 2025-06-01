# Assignment #D: 十全十美 

Updated 1254 GMT+8 Dec 17, 2024



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 02692: 假币问题

brute force, http://cs101.openjudge.cn/practice/02692



代码：

```python
n = int(input())
for _ in range(n):
    left, right, stat = ["" for _ in range(3)], ["" for _ in range(3)], ["" for _ in range(3)]
    flag = [6 for _ in range(12)]
    for i in range(3):
        left[i], right[i], stat[i] = input().split()
        if stat[i] == "even":
            for coin in left[i] + right[i]:
                flag[ord(coin) - ord("A")] = 0
        else:
            for coin in left[i]:
                if stat[i] == "up":
                    flag[ord(coin) - ord("A")] = -1 if flag[ord(coin) - ord("A")] in [-1, 6] else 0
                else:
                    flag[ord(coin) - ord("A")] = 1 if flag[ord(coin) - ord("A")] in [1, 6] else 0
            for coin in right[i]:
                if stat[i] == "up":
                    flag[ord(coin) - ord("A")] = 1 if flag[ord(coin) - ord("A")] in [1, 6] else 0
                else:
                    flag[ord(coin) - ord("A")] = -1 if flag[ord(coin) - ord("A")] in [-1, 6] else 0
    fake, weight = "", ""
    for i in range(12):
        if flag[i]:
            fake = chr(i + ord("A"))
            weight = "heavy" if flag[i] == -1 else "light"
            break
    print(f"{fake} is the counterfeit coin and it is {weight}.")
```



代码运行截图

![截屏2024-12-21 15.16.04](https://raw.githubusercontent.com/AlbertJ-314/img/main/202412211517183.png)



### 01088: 滑雪

dp, dfs similar, http://cs101.openjudge.cn/practice/01088



代码：

```python
from functools import lru_cache

@lru_cache(maxsize=None)
def dfs(x: int, y: int) -> int:
    length = 1
    for dx, dy in [(1, 0), (-1, 0), (0, 1), (0, -1)]:
        if 0 <= x + dx < m and 0 <= y + dy < n and h[x + dx][y + dy] < h[x][y]:
            length = max(length, dfs(x + dx, y + dy) + 1)
    return length

m, n = map(int, input().split())
h = [list(map(int, input().split())) for _ in range(m)]
print(max(dfs(i, j) for i in range(m) for j in range(n)))
```



代码运行截图

![截屏2024-12-21 15.18.32](https://raw.githubusercontent.com/AlbertJ-314/img/main/202412211519097.png)



### 25572: 螃蟹采蘑菇

bfs, dfs, http://cs101.openjudge.cn/practice/25572/



代码：

```python
n = int(input())
maze = [[1] + list(map(int, input().split())) + [1] for _ in range(n)]
maze = [[1 for _ in range(n + 2)]] + maze + [[1 for _ in range(n + 2)]]
sx, sy, dx, dy, tx, ty = 0, 0, 0, 0, 0, 0
for i in range(1, n + 1):
    for j in range(1, n + 1):
        if maze[i][j] == 5:
            sx, sy = i, j
            dx, dy = (1, 0) if maze[i - 1][j] == 5 else (0, 1)
        if maze[i][j] == 9:
            tx, ty = i, j
graph = [[0 for _ in range(n + 2)] for _ in range(n + 2)]
for i in range(n + 2):
    for j in range(n + 2):
        if maze[i][j] == 1:
            graph[i][j] = 1
            if i + dx < n + 2 and j + dy < n + 2:
                graph[i + dx][j + dy] = 1
graph[sx][sy] = 5
def dfs(x: int, y: int):
    for mx, my in [(1, 0), (-1, 0), (0, 1), (0, -1)]:
        if graph[x + mx][y + my] not in [1, 5]:
            graph[x + mx][y + my] = 5
            dfs(x + mx, y + my)
dfs(sx, sy)
print("yes" if graph[tx][ty] == 5 or graph[tx + dx][ty + dy] == 5 else "no")
```



代码运行截图

![截屏2024-12-21 15.20.19](https://raw.githubusercontent.com/AlbertJ-314/img/main/202412211520491.png)



### 27373: 最大整数

dp, http://cs101.openjudge.cn/practice/27373/



代码：

```python
m, n = int(input()), int(input())
nums = sorted(list(input().split()), key=lambda x: x * 20, reverse=True)
dp = ["" for _ in range(m + 1)]
for num in nums:
    for j in range(m, len(num) - 1, -1):
        if int(dp[j - len(num)] + num) > (int(dp[j]) if dp[j] else 0):
            dp[j] = dp[j - len(num)] + num
print(dp[m])
```



代码运行截图

![截屏2024-12-21 15.21.39](https://raw.githubusercontent.com/AlbertJ-314/img/main/202412211522444.png)



### 02811: 熄灯问题

brute force, http://cs101.openjudge.cn/practice/02811



代码：

```python
lights = [list(map(int, input().split())) for _ in range(5)]
for i in range(5):
    lights[i] = sum(lights[i][j] * 2 ** j for j in range(6))
ans = [0 for _ in range(5)]
for mask in range(64):
    ans[0] = mask
    for j in range(1, 5):
        ans[j] = ((ans[j - 1] % 32) << 1) ^ ans[j - 1] ^ (ans[j - 1] >> 1) ^ (ans[j - 2] if j > 1 else 0) ^ lights[j - 1]
    if ((ans[4] % 32) << 1) ^ ans[4] ^ (ans[4] >> 1) ^ ans[3] == lights[4]:
        break
for i in range(5):
    print(" ".join(bin(ans[i])[2:].zfill(6)[::-1]))
```



代码运行截图

![截屏2024-12-21 15.22.47](https://raw.githubusercontent.com/AlbertJ-314/img/main/202412211523219.png)



### 08210: 河中跳房子

binary search, greedy, http://cs101.openjudge.cn/practice/08210/



代码：

```python
L, N, M = map(int, input().split())
stones = [int(input()) for _ in range(N)] + [L]
l, r = 0, L
while l < r:
    mid = (l + r + 1) // 2
    cur, cnt = 0, 0
    for i in range(N + 1):
        if stones[i] - cur < mid:
            cnt += 1
        else:
            cur = stones[i]
    if cnt <= M:
        l = mid
    else:
        r = mid - 1
print(l)
```



代码运行截图

![截屏2024-12-21 15.24.30](https://raw.githubusercontent.com/AlbertJ-314/img/main/202412211525732.png)



## 2. 学习总结和收获

考前刷题。

额外练习：每⽇选做的所有题，以及 LeetCode 上⼀些题⽬，⽐如 918, 198, 213, 740, 1388, 589, 590, 124, 199, 733, 542, 322。
