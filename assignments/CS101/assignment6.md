# Assignment #6: Recursion and DP

Updated 2201 GMT+8 Oct 29, 2024



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### sy119: 汉诺塔

recursion, https://sunnywhy.com/sfbj/4/3/119  



代码：

```python
def Hanoi(n: int, A: str, B: str, C: str) -> None:
    if not n:
        return
    Hanoi(n - 1, A, C, B)
    print(f"{A}->{C}")
    Hanoi(n - 1, B, A, C)


n = int(input())
print(2 ** n - 1)
Hanoi(n, "A", "B", "C")
```



代码运行截图

![截屏2024-10-31 20.18.30](https://raw.githubusercontent.com/AlbertJ-314/img/main/202410312042585.png)



### sy132: 全排列I

recursion, https://sunnywhy.com/sfbj/4/3/132



代码：

```python
def permutation(n: int, cur: [int]) -> None:
    if len(cur) == n:
        print(" ".join(map(str, cur)))
        return
    for i in range(1, n + 1):
        if i not in cur:
            permutation(n, cur + [i])


n = int(input())
permutation(n, [])
```



代码运行截图

![截屏2024-10-31 20.21.20](https://raw.githubusercontent.com/AlbertJ-314/img/main/202410312041876.png)



### 02945: 拦截导弹 

dp, http://cs101.openjudge.cn/2024fallroutine/02945



代码：

```python
from math import inf
k = int(input())
arr = list(map(int, input().split()))
dp = [inf] + [-inf for _ in range(k)]
for i in range(k):
    l, r = 0, k
    while l < r:
        mid = (l + r + 1) // 2
        if dp[mid] < arr[i]:
            r = mid - 1
        else:
            l = mid
    dp[l + 1] = max(dp[l + 1], arr[i])
for i in range(k, -1, -1):
    if dp[i] > -inf:
        print(i); break
```



代码运行截图

![截屏2024-10-31 20.25.48](https://raw.githubusercontent.com/AlbertJ-314/img/main/202410312026786.png)



### 23421: 小偷背包 

dp, http://cs101.openjudge.cn/practice/23421



代码：

```python
n, b = map(int, input().split())
value, weight = list(map(int, input().split())), list(map(int, input().split()))
dp = [0 for _ in range(b + 1)]
for i in range(n):
    for j in range(b, weight[i] - 1, -1):
        dp[j] = max(dp[j], dp[j - weight[i]] + value[i])
print(dp[b])
```



代码运行截图

![截屏2024-10-31 20.28.00](https://raw.githubusercontent.com/AlbertJ-314/img/main/202410312028163.png)



### 02754: 八皇后

dfs and similar, http://cs101.openjudge.cn/practice/02754



代码：

```python
from copy import deepcopy
res = []

def on_board(x: int, y: int) -> bool:
    return 0 <= x < 8 and 0 <= y < 8


def cancel(board: [[bool]], x: int, y: int) -> [[bool]]:
    tmp = deepcopy(board)
    for i in range(8):
        tmp[x][i], tmp[i][y] = False, False
        if on_board(x + i, y + i):
            tmp[x + i][y + i] = False
        if on_board(x + i, y - i):
            tmp[x + i][y - i] = False
        if on_board(x - i, y + i):
            tmp[x - i][y + i] = False
        if on_board(x - i, y - i):
            tmp[x - i][y - i] = False
    return tmp


def queens(board: [[bool]], row: int, cur: [int]) -> None:
    if row == 8:
        res.append(cur); return
    for i in range(8):
        if board[row][i]:
            queens(cancel(board, row, i), row + 1, cur + [i])


queens([[True for _ in range(8)] for _ in range(8)], 0, [])
n = int(input())
for _ in range(n):
    print("".join(map(lambda x: str(x + 1), res[int(input()) - 1])))
```



代码运行截图

![截屏2024-10-31 20.29.45](https://raw.githubusercontent.com/AlbertJ-314/img/main/202410312030521.png)



### 189A. Cut Ribbon 

brute force, dp 1300 https://codeforces.com/problemset/problem/189/A



代码：

```python
n, a, b, c = map(int, input().split())
dp = [0 for _ in range(n + 1)]
dp[0] = 1
for i in range(1, n + 1):
    tmp = max(dp[i - a] if i >= a else 0, dp[i - b] if i >= b else 0, dp[i - c] if i >= c else 0)
    dp[i] = tmp + 1 if tmp else 0
print(dp[n] - 1)
```



代码运行截图

![截屏2024-10-31 20.32.30](https://raw.githubusercontent.com/AlbertJ-314/img/main/202410312033200.png)



## 2. 学习总结和收获

复习了一下优先队列。

额外练习：每⽇选做的所有题，以及 LeetCode 上⼀些题⽬，⽐如 622, 346, 225, 703, 347, 451, 973, 1296, 239, 295, 23, 218。
