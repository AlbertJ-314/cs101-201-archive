# Assignment #A: dp & bfs

Updated 2 GMT+8 Nov 25, 2024



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### LuoguP1255 数楼梯

dp, bfs, https://www.luogu.com.cn/problem/P1255



代码：

```python
n = int(input())
dp = [1, 1]
for _ in range(n - 1):
    dp.append(dp[-1] + dp[-2])
print(dp[-1])
```



代码运行截图

![截屏2024-12-01 20.41.48](https://raw.githubusercontent.com/AlbertJ-314/img/main/202412012042643.png)



### 27528: 跳台阶

dp, http://cs101.openjudge.cn/practice/27528/



代码：

```python
print(2 ** (int(input()) - 1))
```



代码运行截图

![截屏2024-12-01 20.43.34](https://raw.githubusercontent.com/AlbertJ-314/img/main/202412012044639.png)



### 474D. Flowers

dp, https://codeforces.com/problemset/problem/474/D



代码：

```python
Mod, dp = 10 ** 9 + 7, [0 for _ in range(10 ** 5 + 1)]

t, k = map(int, input().split())
for i in range(k):
    dp[i] = 1
for i in range(k, 10 ** 5 + 1):
    dp[i] = (dp[i - 1] + dp[i - k]) % Mod
for i in range(1, 10 ** 5 + 1):
    dp[i] = (dp[i] + dp[i - 1]) % Mod

for _ in range(t):
    a, b = map(int, input().split())
    print((dp[b] - dp[a - 1]) % Mod)
```



代码运行截图

![截屏2024-12-01 20.45.11](https://raw.githubusercontent.com/AlbertJ-314/img/main/202412012045863.png)



### LeetCode5.最长回文子串

dp, two pointers, string, https://leetcode.cn/problems/longest-palindromic-substring/



代码：

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        t = "." + ".".join(s) + "."
        n, i, r = len(t), 0, 0
        maxr = [0 for _ in range(n)]
        while i < n:
            while r < min(i, n - i - 1) and t[i - r - 1] == t[i + r + 1]:
                r += 1
            maxr[i], j = r, i + 1
            while j <= i + r:
                if maxr[2 * i - j] == r + i - j:
                    r += i - j; break
                maxr[j] = min(maxr[2 * i - j], r + i - j)
                j += 1
            if j > i + r:
                r = 0
            i = j
        ind = 0
        for i in range(n):
            if maxr[i] > maxr[ind]:
                ind = i
        return t[ind - maxr[ind]: ind + maxr[ind] + 1].replace(".", "")
```



代码运行截图

![截屏2024-12-01 20.47.06](https://raw.githubusercontent.com/AlbertJ-314/img/main/202412012047165.png)





### 12029: 水淹七军

bfs, dfs, http://cs101.openjudge.cn/practice/12029/



代码：

```python
import sys
from collections import deque
directions = [[1, 0], [0, 1], [-1, 0], [0, -1]]
data, cnt = list(map(int, sys.stdin.read().split())), 1
K = data[0]
for i in range(K):
    m, n = data[cnt], data[cnt + 1]
    cnt += 2
    h, water = [], [[0 for _ in range(n)] for _ in range(m)]
    for _ in range(m):
        h.append(data[cnt: cnt + n])
        cnt += n
    hqx, hqy = data[cnt] - 1, data[cnt + 1] - 1
    cnt += 2
    p = data[cnt]; cnt += 1
    for _ in range(p):
        wx, wy = data[cnt] - 1, data[cnt + 1] - 1
        cnt += 2
        water[wx][wy] = max(water[wx][wy], h[wx][wy])
        queue = deque([(wx, wy)])
        while queue:
            cur = queue.popleft()
            for d in directions:
                new_x, new_y = cur[0] + d[0], cur[1] + d[1]
                if 0 <= new_x < m and 0 <= new_y < n:
                    if water[cur[0]][cur[1]] > max(water[new_x][new_y], h[new_x][new_y]):
                        water[new_x][new_y] = water[cur[0]][cur[1]]
                        queue.append((new_x, new_y))
    print("Yes" if water[hqx][hqy] else "No")
```



代码运行截图

![截屏2024-12-01 20.48.42](https://raw.githubusercontent.com/AlbertJ-314/img/main/202412012049555.png)



### 02802: 小游戏

bfs, http://cs101.openjudge.cn/practice/02802/



代码：

```python
from collections import defaultdict
import heapq
from math import inf

directions = [(1, 0), (0, 1), (-1, 0), (0, -1)]
board_cnt = 1

while True:
    n, m = map(int, input().split())
    if not m and not n:
        break
    print(f"Board #{board_cnt}:"); board_cnt += 1

    board = [[True] + list(map(lambda c: c != "X", input())) + [True] for _ in range(m)]
    board = [[True for _ in range(n + 2)]] + board + [[True for _ in range(n + 2)]]

    pair_cnt = 1

    while True:
        y1, x1, y2, x2 = map(int, input().split())
        if not x1 and not y1 and not x2 and not y2:
            break
        print(f"Pair {pair_cnt}: ", end=""); pair_cnt += 1

        board[x2][y2] = True

        dis, queue, vis = defaultdict(lambda: inf), [(0, (x1, y1), -1)], set()
        dis[(x1, y1)] = 0

        while queue:
            _, cur, to = heapq.heappop(queue)
            if (cur, to) in vis:
                continue
            vis.add((cur, to))

            for i, d in enumerate(directions):
                new = (cur[0] + d[0], cur[1] + d[1])
                if 0 <= new[0] <= m + 1 and 0 <= new[1] <= n + 1 and board[new[0]][new[1]]:
                    if dis[new] > dis[cur] + (i != to):
                        dis[new] = dis[cur] + (i != to)
                        heapq.heappush(queue, (dis[new], new, i))

        board[x2][y2] = False

        print(f"{dis[(x2, y2)]} segments." if dis[(x2, y2)] != inf else "impossible.")
    print()
```



代码运行截图

![截屏2024-12-01 20.50.31](https://raw.githubusercontent.com/AlbertJ-314/img/main/202412012104140.png)



## 2. 学习总结和收获

额外练习：每⽇选做的所有题，LeetCode 上做的题不多，因为去看张学友演唱会啦啦啦⬇️

<img src="https://raw.githubusercontent.com/AlbertJ-314/img/main/202412012059397.jpeg" alt="IMG_1312" style="zoom:13%;" align="left" /><img src="https://raw.githubusercontent.com/AlbertJ-314/img/main/202412012100792.jpg" alt="WechatIMG7" style="zoom:18%;" align="right" />

