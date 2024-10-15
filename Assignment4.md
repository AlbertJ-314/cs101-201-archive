# Assignment #4: T-primes + 贪心

Updated 0337 GMT+8 Oct 15, 2024

2024 fall, Complied by 焦玮宸 数学科学学院



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 34B. Sale

greedy, sorting, 900, https://codeforces.com/problemset/problem/34/B



代码

```python
n, m = map(int, input().split())
nums = sorted(list(map(int, input().split())))
ans = 0
for i in range(n):
    if nums[i] >= 0 or i >= m:
        break
    ans += nums[i]
print(-ans)
```



代码运行截图

![截屏2024-10-15 19.10.08](https://raw.githubusercontent.com/AlbertJ-314/img/main/202410151911079.png)



### 160A. Twins

greedy, sortings, 900, https://codeforces.com/problemset/problem/160/A



代码

```python
n = int(input())
nums = sorted(list(map(int, input().split())), reverse=True)
cur, total = 0, sum(nums)
for i in range(n):
    cur += nums[i]
    if 2 * cur > total:
        print(i + 1)
        break
```



代码运行截图

![截屏2024-10-15 19.12.44](https://raw.githubusercontent.com/AlbertJ-314/img/main/202410151914443.png)



### 1879B. Chips on the Board

constructive algorithms, greedy, 900, https://codeforces.com/problemset/problem/1879/B



代码

```python
t = int(input())
while t:
    n = int(input())
    a, b = list(map(int, input().split())), list(map(int, input().split()))
    print(min(n * min(a) + sum(b), n * min(b) + sum(a)))
    t -= 1
```



代码运行截图

![截屏2024-10-15 19.25.58](https://raw.githubusercontent.com/AlbertJ-314/img/main/202410151934437.png)



### 158B. Taxi

*special problem, greedy, implementation, 1100, https://codeforces.com/problemset/problem/158/B



代码

```python
n = int(input())
count = [0 for _ in range(5)]
groups = list(map(int, input().split()))
for group in groups:
    count[group] += 1
print(count[4] + count[3] + (count[2] + 1) // 2 + (max(0, count[1] - count[3] - (count[2] % 2) * 2) + 3) // 4)
```



代码运行截图

![截屏2024-10-15 19.28.07](https://raw.githubusercontent.com/AlbertJ-314/img/main/202410151928332.png)



### *230B. T-primes（选做）

binary search, implementation, math, number theory, 1300, http://codeforces.com/problemset/problem/230/B



代码

```python
book = [True] * (10 ** 6 + 1)
primes, ind = [0] * 78498, 0
for i in range(2, 10 ** 6 + 1):
    if not book[i]:
        continue
    primes[ind]= i
    ind += 1
    for j in range(i, 10 ** 6 // i + 1):
        book[j * i] = False
 
n = int(input())
x = list(map(int, input().split()))
for i in range(n):
    x[i] = (x[i], i)
x.sort()
ans = ["NO"] * n
i, j = 0, 0
while j < n:
    while i < 78498 and primes[i] ** 2 < x[j][0]:
        i += 1
    if i >= 78498:
        break
    if primes[i] ** 2 == x[j][0]:
        ans[x[j][1]] = "YES"
    j += 1
for i in range(n):
    print(ans[i])
```



代码运行截图

![截屏2024-10-15 19.57.43](https://raw.githubusercontent.com/AlbertJ-314/img/main/202410151958032.png)



### *12559: 最大最小整数 （选做）

greedy, strings, sortings, http://cs101.openjudge.cn/practice/12559



代码

```python
from functools import cmp_to_key
def cmp(a, b):
    if int(a + b) > int(b + a):
        return 1
    elif int(a + b) < int(b + a):
        return -1
    else:
        return 0

_ = int(input())
nums = sorted(list(input().split()),key=cmp_to_key(cmp))
print("".join(nums[::-1]), "".join(nums))
```



代码运行截图

![截屏2024-10-15 19.50.40](https://raw.githubusercontent.com/AlbertJ-314/img/main/202410151952592.png)



## 2. 学习总结和收获

复习了一下栈的基础知识。

额外练习：每⽇选做的所有题，以及 LeetCode 上⼀些题⽬，⽐如 1047, 155, 20, 227, 739, 150, 232, 394, 32, 946, 71。
