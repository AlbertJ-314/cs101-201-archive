# Assignment #5: Greedy穷举Implementation

Updated 1939 GMT+8 Oct 21, 2024



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 04148: 生理周期

brute force, http://cs101.openjudge.cn/practice/04148



代码：

```python
cnt = 0
while True:
    p, e, i, d = map(int, input().split())
    if p == -1 and e == -1 and i == -1 and d == -1:
        break
    cnt += 1
    print(f"Case {cnt}: the next triple peak occurs in ", end="")
    print((p * 6 * 28 * 33 + e * 19 * 33 * 23 + i * 2 * 23 * 28 - d - 1) % 21252 + 1, end="")
    print(" days.")
```



代码运行截图

![截屏2024-10-22 08.29.09](https://raw.githubusercontent.com/AlbertJ-314/img/main/202410220829936.png)



### 18211: 军备竞赛

greedy, two pointers, http://cs101.openjudge.cn/practice/18211



代码：

```python
p = int(input())
weapon = sorted(list(map(int, input().split())))
n, cur, ans = len(weapon), 0, 0
l, r = 0, n - 1
while l <= r:
    if p >= weapon[l]:
        cur += 1; ans = max(ans, cur)
        p -= weapon[l]; l += 1
    elif not cur:
        break
    else:
        cur -= 1
        p += weapon[r]; r -= 1
print(ans)
```



代码运行截图

![截屏2024-10-22 08.30.47](https://raw.githubusercontent.com/AlbertJ-314/img/main/202410220831765.png)



### 21554: 排队做实验

greedy, http://cs101.openjudge.cn/practice/21554



代码：

```python
n = int(input())
time = list(map(int, input().split()))
students = sorted([(time[i], i + 1) for i in range(n)])
print(" ".join(map(lambda stu: str(stu[1]), students)))
pre_sum, total = 0, 0
for stu in students:
    total += pre_sum
    pre_sum += stu[0]
print(f"{total/n:.2f}")
```



代码运行截图

![截屏2024-10-22 08.32.06](https://raw.githubusercontent.com/AlbertJ-314/img/main/202410220832596.png)



### 01008: Maya Calendar

implementation, http://cs101.openjudge.cn/practice/01008/



代码：

```python
Haab = {"pop": 0, "no": 1, "zip": 2, "zotz": 3, "tzec": 4, "xul": 5, "yoxkin": 6, "mol": 7, "chen": 8, "yax": 9, "zac": 10, "ceh": 11, "mac": 12, "kankin": 13, "muan": 14, "pax": 15, "koyab": 16, "cumhu": 17, "uayet": 18}
Tzolkin = {0: "imix", 1: "ik", 2: "akbal", 3: "kan", 4: "chicchan", 5: "cimi", 6: "manik", 7: "lamat", 8: "muluk", 9: "ok", 10: "chuen", 11: "eb", 12: "ben", 13: "ix", 14: "mem", 15: "cib", 16: "caban", 17: "eznab", 18: "canac", 19: "ahau"}
n = int(input())
print(n)
for _ in range(n):
    day, month, year = input().split()
    day = int(day.rstrip("."))
    total = int(year) * 365 + Haab[month] * 20 + day
    print(total % 13 + 1, Tzolkin[total % 20], total // 260)
```



代码运行截图

![截屏2024-10-22 08.33.10](https://raw.githubusercontent.com/AlbertJ-314/img/main/202410220833937.png)



### 545C. Woodcutters

dp, greedy, 1500, https://codeforces.com/problemset/problem/545/C



代码：

```python
from math import inf
n = int(input())
trees = [list(map(int, input().split())) for _ in range(n)] + [[inf, 0]]
cur, ans = -inf, 0
for i in range(n):
    if trees[i][1] < trees[i][0] - cur:
        cur = trees[i][0]; ans += 1
    elif trees[i][1] < trees[i + 1][0] - trees[i][0]:
        cur = trees[i][0] + trees[i][1]; ans += 1
    else:
        cur = trees[i][0]
print(ans)
```



代码运行截图

![截屏2024-10-22 08.24.03](https://raw.githubusercontent.com/AlbertJ-314/img/main/202410220839830.png)



### 01328: Radar Installation

greedy, http://cs101.openjudge.cn/practice/01328/



代码：

```python
from math import sqrt
case_cnt = 0
while True:
    n, d = map(int, input().split())
    if not n and not d:
        break
    case_cnt += 1
    flag = True
    isl = [list(map(int, input().split())) for _ in range(n)]
    for i in range(n):
        if isl[i][1] > d:
            flag = False; break
        isl[i] = [isl[i][0] - sqrt(d ** 2 - isl[i][1] ** 2), isl[i][0] + sqrt(d ** 2 - isl[i][1] ** 2)]
    if not flag:
        print(f"Case {case_cnt}: -1")
        input(); continue
    isl = sorted(isl, key=lambda x: x[1])
    cur, ans = isl[0][1], 1
    for i in range(n):
        if isl[i][0] > cur:
            cur = isl[i][1]; ans += 1
    print(f"Case {case_cnt}: {ans}")
    input()
```



代码运行截图

![截屏2024-10-22 08.26.54](https://raw.githubusercontent.com/AlbertJ-314/img/main/202410220827618.png)



## 2. 学习总结和收获

复习了一下单调栈。

额外练习：每⽇选做的所有题，以及 LeetCode 上⼀些题⽬，⽐如 739, 496, 503, 901, 84, 316, 42, 85, 862。
