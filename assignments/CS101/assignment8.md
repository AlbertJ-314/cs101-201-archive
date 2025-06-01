# Assignment #8: 田忌赛马来了

Updated 1021 GMT+8 Nov 12, 2024



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 12558: 岛屿周⻓

matices, http://cs101.openjudge.cn/practice/12558/ 



代码：

```python
n, m = map(int, input().split())
graph = [["0" for _ in range(m + 2)]] + [["0"] + list(input().split()) + ["0"] for _ in range(n)] + [["0" for _ in range(m + 2)]]
directions, ans = [(0, 1), (1, 0), (0, -1), (-1, 0)], 0
for i in range(n + 2):
    for j in range(m + 2):
        if graph[i][j] == "1":
            continue
        for drc in directions:
            x, y = i + drc[0], j + drc[1]
            if 1 <= x <= n and 1 <= y <= m:
                ans += (graph[x][y] == "1")
print(ans)
```



代码运行截图

![截屏2024-11-16 20.50.41](https://raw.githubusercontent.com/AlbertJ-314/img/main/202411162051118.png)



### LeetCode54.螺旋矩阵

matrice, https://leetcode.cn/problems/spiral-matrix/

与OJ这个题目一样的 18106: 螺旋矩阵，http://cs101.openjudge.cn/practice/18106



代码：

```python
n = int(input())
matrix = [[0 for _ in range(n)] for _ in range(n)]
i, j, cnt = 0, 0, 1
direction = [0, 1]
while cnt <= n * n:
    matrix[i][j] = cnt
    if i + direction[0] < 0 or i + direction[0] >= n or j + direction[1] < 0 or j + direction[1] >= n:
        direction = [direction[1], - direction[0]]
    elif matrix[i + direction[0]][j + direction[1]]:
            direction = [direction[1], - direction[0]]
    i += direction[0]; j += direction[1]; cnt += 1
print("\n".join(map(lambda x: " ".join(map(str, x)), matrix)))
```



代码运行截图

![截屏2024-11-16 20.55.14](https://raw.githubusercontent.com/AlbertJ-314/img/main/202411162055633.png)



### 04133:垃圾炸弹

matrices, http://cs101.openjudge.cn/practice/04133/



代码：

```python
d, n = int(input()), int(input())
city = [[0 for _ in range(1025)] for _ in range(1025)]

for _ in range(n):
    x, y, trash = map(int, input().split())
    for i in range(max(0, x - d), min(1024, x + d) + 1):
        for j in range(max(0, y - d), min(1024, y + d) + 1):
            city[i][j] += trash

cnt, ans = 1, 0
for i in range(1025):
    for j in range(1025):
        if city[i][j] > ans:
            ans = city[i][j]; cnt = 1
        elif city[i][j] == ans:
            cnt += 1

print(cnt, ans)
```



代码运行截图

![截屏2024-11-16 20.57.01](https://raw.githubusercontent.com/AlbertJ-314/img/main/202411162057511.png)



### LeetCode376.摆动序列

greedy, dp, https://leetcode.cn/problems/wiggle-subsequence/

与OJ这个题目一样的，26976:摆动序列, http://cs101.openjudge.cn/routine/26976/



代码：

```python
n = int(input())
nums = list(map(int, input().split()))
cur, ans = 0, 1
for i in range(1, n):
    if nums[i] == nums[i - 1]:
        continue
    if (nums[i] - nums[i - 1]) * cur <= 0:
        ans += 1
    cur = nums[i] - nums[i - 1]
print(ans)
```



代码运行截图

![截屏2024-11-16 20.58.36](https://raw.githubusercontent.com/AlbertJ-314/img/main/202411162059567.png)



### CF455A: Boredom

dp, 1500, https://codeforces.com/contest/455/problem/A



代码：

```python
input()
count = {}
for num in list(map(int, input().split())):
    count[num] = count[num] + 1 if num in count else 1
arr = sorted([(k, v) for i, (k, v) in enumerate(count.items())])
n = len(arr)
dp = [0 for _ in range(n)]
for i in range(n):
    if not i or arr[i - 1][0] + 1 != arr[i][0]:
        dp[i] = dp[i - 1] + arr[i][0] * arr[i][1]
    else:
        dp[i] = max((dp[i - 2] if i >= 2 else 0) + arr[i][0] * arr[i][1], dp[i - 1])
print(dp[-1])
```



代码运行截图

![截屏2024-11-16 21.00.28](https://raw.githubusercontent.com/AlbertJ-314/img/main/202411162101159.png)



### 02287: Tian Ji -- The Horse Racing

greedy, dfs http://cs101.openjudge.cn/practice/02287



代码：

```python
while True:
    n = int(input())
    if n == 0:
        break
    tian = sorted(list(map(int, input().split())))
    king = sorted(list(map(int, input().split())))
    lt, rt, lk, rk, ans = 0, n - 1, 0, n - 1, 0
    while lt <= rt:
        if tian[lt] > king[lk]:
            lt += 1; lk += 1; ans += 1
        elif tian[rt] > king[rk]:
            rt -= 1; rk -= 1; ans += 1
        elif tian[lt] < king[lk] or tian[rt] < king[rk]:
            lt += 1; rk -= 1; ans -= 1
        else:
            ans -= (tian[lt] < king[rk]); lt += 1; rk -= 1
    print(ans * 200)
```



代码运行截图

![截屏2024-11-16 21.02.00](https://raw.githubusercontent.com/AlbertJ-314/img/main/202411162102560.png)



## 2. 学习总结和收获

继续练习 dp。

额外练习：每⽇选做的所有题，以及 LeetCode 上⼀些题⽬，⽐如 118, 119, 120, 64, 174, 221, 931, 576, 85, 363, 144。
