# Assignment #2: 语法练习

Updated 0126 GMT+8 Sep 24, 2024

2024 fall, Complied by 焦玮宸 数学科学学院



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知9月19日导入选课名单后启用。**作业写好后，保留在自己手中，待9月20日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 263A. Beautiful Matrix

https://codeforces.com/problemset/problem/263/A



##### 代码

```python
ans = 0
for row in range(5):
    col = input().find("1")
    if col != -1:
        ans = abs(2 - row) + abs(2 - col // 2)
print(ans)
```



代码运行截图

![截屏2024-09-24 18.49.19](https://raw.githubusercontent.com/AlbertJ-314/img/main/202409241850901.png)



### 1328A. Divisibility Problem

https://codeforces.com/problemset/problem/1328/A



##### 代码

```python
t = int(input())
for i in range(t):
    a, b = map(int, input().split())
    print(b - (a - 1) % b - 1)
```



代码运行截图

![截屏2024-09-24 19.00.59](https://raw.githubusercontent.com/AlbertJ-314/img/main/202409241903564.png)



### 427A. Police Recruits

https://codeforces.com/problemset/problem/427/A



##### 代码

```python
n = int(input())
arr = list(map(int, input().split()))
cnt = ans = 0
for num in arr:
    if num == -1:
        if cnt:
            cnt -= 1
        else:
            ans += 1
    else:
        cnt += num
print(ans)
```



代码运行截图

![截屏2024-09-24 19.06.22](https://raw.githubusercontent.com/AlbertJ-314/img/main/202409241908918.png)



### 02808: 校门外的树

http://cs101.openjudge.cn/practice/02808/



##### 代码

```python
l, m = map(int, input().split())
tree, ans = [True for _ in range(l + 1)], l + 1
while m:
    start, end = map(int, input().split())
    for i in range(start, end + 1):
        ans -= tree[i]
        tree[i] = False
    m -= 1
print(ans)
```



代码运行截图

![截屏2024-09-24 19.15.13](https://raw.githubusercontent.com/AlbertJ-314/img/main/202409241916084.png)



### sy60: 水仙花数II

https://sunnywhy.com/sfbj/3/1/60



##### 代码

```python
a, b = map(int, input().split())
flag = False
for i in range(a, b + 1):
    if i == (i // 100) ** 3 + (i // 10 % 10) ** 3 + (i % 10) ** 3:
        print(f" {i}" if flag else i, end=""); flag = True
if not flag:
    print("NO")
```



代码运行截图

![截屏2024-09-24 19.10.51](https://raw.githubusercontent.com/AlbertJ-314/img/main/202409241912511.png)



### 01922: Ride to School

http://cs101.openjudge.cn/practice/01922/



##### 代码

```python
import math
while True:
    n = int(input())
    if not n:
        break
    ans = math.inf
    for _ in range(n):
        v, t = map(int, input().split())
        if t >= 0:
            ans = min(ans, t + 4500 * 3.6 / v)
    print(math.ceil(ans))
```



代码运行截图

![截屏2024-09-24 21.46.27](https://raw.githubusercontent.com/AlbertJ-314/img/main/202409242148988.png)



## 2. 学习总结和收获

复习了一下二分算法。

额外练习：每日选做的所有题，以及 LeetCode 上一些题目，比如 4, 33, 34, 35, 50, 69, 74, 81, 153, 154, 162, 167, 240, 278, 287, 367, 374, 400, 704, 744, 852, 1095, 1300。
