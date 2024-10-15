# Assignment #1: 自主学习

Updated 0110 GMT+8 Sep 10, 2024

2024 fall, Complied by 焦玮宸 数学科学学院



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知9月19日导入选课名单后启用。**作业写好后，保留在自己手中，待9月20日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 02733: 判断闰年

http://cs101.openjudge.cn/practice/02733/



##### 代码

```python
a = int(input())
if a % 4 != 0 or (a % 100 == 0 and a % 400 != 0) or a % 3200 == 0:
    print("N")
else:
    print("Y")
```



代码运行截图

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202409101819933.png)



### 02750: 鸡兔同笼

http://cs101.openjudge.cn/practice/02750/



##### 代码

```python
a = int(input())
if a % 2:
    print("0 0")
else:
    print((a + 2) // 4, a // 2)
```



代码运行截图

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202409101823977.png)



### 50A. Domino piling

greedy, math, 800, http://codeforces.com/problemset/problem/50/A



##### 代码

```python
m, n = map(int, input().split())
print((m * n) // 2)
```



代码运行截图

![CF50A](https://raw.githubusercontent.com/AlbertJ-314/img/main/202409101824351.png)



### 1A. Theatre Square

math, 1000, https://codeforces.com/problemset/problem/1/A



##### 代码

```python
n, m, a = map(int, input().split())
print(((n + a - 1) // a) * ((m + a - 1) // a))
```



代码运行截图

![CF1A](https://raw.githubusercontent.com/AlbertJ-314/img/main/202409101826941.png)



### 112A. Petya and Strings

implementation, strings, 1000, http://codeforces.com/problemset/problem/112/A



##### 代码

```python
s1, s2 = input().lower(), input().lower()
if s1 < s2:
    print(-1)
elif s1 > s2:
    print(1)
else:
    print(0)
```



代码运行截图

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202409101833261.png)



### 231A. Team

bruteforce, greedy, 800, http://codeforces.com/problemset/problem/231/A



##### 代码

```python
n = int(input())
ans = 0
for i in range(n):
    ans += (sum(list(map(int, input().split()))) >= 2)
print(ans)
```



代码运行截图

![CF231A](https://raw.githubusercontent.com/AlbertJ-314/img/main/202409101835091.png)



## 2. 学习总结和收获

复习了一下数组基础知识和排序算法。

额外练习：每日选做的所有题，以及 LeetCode 上一些题目，比如 189, 66, 724, 485, 238, 498, 48, 73, 54, 59, 289, 283, 215, 75, 912, 506, 88, 315, 169, 1122, 220, 164, 561。


