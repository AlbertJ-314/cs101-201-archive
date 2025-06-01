# Assignment #7: Nov Mock Exam立冬

Updated 1646 GMT+8 Nov 7, 2024



**说明：**

1）⽉考： AC6。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### E07618: 病人排队

sorttings, http://cs101.openjudge.cn/practice/07618/



代码：

```python
n = int(input())
elder, younger = [], []
for i in range(n):
    ID, age = input().split()
    if int(age) >= 60:
        elder.append([-int(age), i, ID])
    else:
        younger.append(ID)
for patient in sorted(elder):
    print(patient[2])
print("\n".join(younger))
```



代码运行截图

![截屏2024-11-09 20.48.35](https://raw.githubusercontent.com/AlbertJ-314/img/main/202411092105080.png)



### E23555: 节省存储的矩阵乘法

implementation, matrices, http://cs101.openjudge.cn/practice/23555/



代码：

```python
n, m1, m2 = map(int, input().split())
X, Y = [{} for i in range(n)], [{} for i in range(n)]
for _ in range(m1):
    i, j, val = map(int, input().split())
    X[i][j] = val
for _ in range(m2):
    i, j, val = map(int, input().split())
    Y[j][i] = val
for i in range(n):
    for j in range(n):
        res = 0
        for ind in range(n):
            if ind in X[i] and ind in Y[j]:
                res += X[i][ind] * Y[j][ind]
        if res:
            print(i, j, res)
```



代码运行截图

![截屏2024-11-09 20.52.37](https://raw.githubusercontent.com/AlbertJ-314/img/main/202411092107806.png)



### M18182: 打怪兽 

implementation/sortings/data structures, http://cs101.openjudge.cn/practice/18182/



代码：

```python
t = int(input())
for _ in range(t):
    n, m, b = map(int, input().split())
    count = {}
    for _ in range(n):
        time, x = map(int, input().split())
        if time in count:
            count[time].append(x)
        else:
            count[time] = [x]
    for i in sorted(count.keys()):
        b -= sum(sorted(count[i], reverse=True)[:min(m, len(count[i]))])
        if b <= 0:
            print(i); break
    if b > 0:
        print("alive")
```



代码运行截图

![截屏2024-11-09 20.56.05](https://raw.githubusercontent.com/AlbertJ-314/img/main/202411092107653.png)



### M28780: 零钱兑换3

dp, http://cs101.openjudge.cn/practice/28780/



代码：

```python
from math import inf
n, m = map(int, input().split())
coins = list(map(int, input().split()))
dp = [0] + [inf for _ in range(m)]
for i in range(n):
    for j in range(coins[i], m + 1):
        dp[j] = min(dp[j], dp[j - coins[i]] + 1)
print(dp[m] if dp[m] != inf else -1)
```



代码运行截图

![截屏2024-11-09 21.00.40](https://raw.githubusercontent.com/AlbertJ-314/img/main/202411092108836.png)



### T12757: 阿尔法星人翻译官

implementation, http://cs101.openjudge.cn/practice/12757



代码：

```python
dictionary = {'zero': 0, 'one': 1, 'two': 2, 'three': 3, 'four': 4, 'five': 5, 'six': 6, 'seven': 7, 'eight': 8, 'nine': 9, 'ten': 10, 'eleven': 11, 'twelve': 12, 'thirteen': 13, 'fourteen': 14, 'fifteen': 15, 'sixteen': 16, 'seventeen': 17, 'eighteen': 18, 'nineteen': 19, 'twenty': 20, 'thirty': 30, 'forty': 40, 'fifty': 50, 'sixty': 60, 'seventy': 70, 'eighty': 80, 'ninety': 90}
def convert(words):
    if words[0] == "negative":
        return -convert(words[1:])
    if "million" in words:
        ind = words.index("million")
        return convert(words[:ind]) * (10 ** 6) + (convert(words[ind + 1:]) if ind < len(words) - 1 else 0)
    if "thousand" in words:
        ind = words.index("thousand")
        return convert(words[:ind]) * (10 ** 3) + (convert(words[ind + 1:]) if ind < len(words) - 1 else 0)
    if "hundred" in words:
        ind = words.index("hundred")
        return convert(words[:ind]) * (10 ** 2) + (convert(words[ind + 1:]) if ind < len(words) - 1 else 0)
    return sum(list(map(lambda s: dictionary[s], words)))


print(convert(list(input().split())))
```



代码运行截图

![截屏2024-11-09 21.01.43](https://raw.githubusercontent.com/AlbertJ-314/img/main/202411092108170.png)



### T16528: 充实的寒假生活

greedy/dp, cs10117 Final Exam, http://cs101.openjudge.cn/practice/16528/



代码：

```python
n = int(input())
events = [list(map(int, input().split())) for _ in range(n)]
cur, ans = -1, 0
for event in sorted(events, key=lambda x: x[1]):
    if event[0] > cur:
        cur = event[1]; ans += 1
print(ans)
```



代码运行截图

![截屏2024-11-09 21.03.24](https://raw.githubusercontent.com/AlbertJ-314/img/main/202411092108888.png)



## 2. 学习总结和收获

复习了一下 dp 的一些基础算法。

额外练习：每⽇选做的所有题，以及 LeetCode 上⼀些题⽬，⽐如 509, 70, 62, 1137, 650, 264, 279, 343, 416, 494, 1049。
