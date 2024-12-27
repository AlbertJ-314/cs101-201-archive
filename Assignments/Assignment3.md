# Assign #3: Oct Mock Exam暨选做题目满百

Updated 1537 GMT+8 Oct 10, 2024



**说明：**

1）Oct⽉考： AC 5。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。

2）请把每个题目解题思路（可选），源码Python, 或者C++/C（已经在Codeforces/Openjudge上AC），截图（包含Accepted, 学号），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、作业评论有md或者doc。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### E28674:《黑神话：悟空》之加密

http://cs101.openjudge.cn/practice/28674/



代码

```python
k = int(input())
s = input()
alphabet = [('a', 'A'), ('b', 'B'), ('c', 'C'), ('d', 'D'), ('e', 'E'), ('f', 'F'), ('g', 'G'), ('h', 'H'), ('i', 'I'), ('j', 'J'), ('k', 'K'), ('l', 'L'), ('m', 'M'), ('n', 'N'), ('o', 'O'), ('p', 'P'), ('q', 'Q'), ('r', 'R'), ('s', 'S'), ('t', 'T'), ('u', 'U'), ('v', 'V'), ('w', 'W'), ('x', 'X'), ('y', 'Y'), ('z', 'Z')]
for c in s:
    print(alphabet[(ord(c.lower()) - ord("a") - k) % 26][int(c.isupper())], end="")
print()
```



代码运行截图

![截屏2024-10-11 13.33.40](https://raw.githubusercontent.com/AlbertJ-314/img/main/202410111336630.png)



### E28691: 字符串中的整数求和

http://cs101.openjudge.cn/practice/28691/



代码

```python
a, b = input().split()
print(int(a[0:2]) + int(b[0:2]))
```



代码运行截图

![截屏2024-10-11 13.41.09](https://raw.githubusercontent.com/AlbertJ-314/img/main/202410111343696.png)



### M28664: 验证身份证号

http://cs101.openjudge.cn/practice/28664/



代码

```python
n = int(input())
coeff = [7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2]
res = ["1", "0", "X", "9", "8", "7", "6", "5", "4", "3", "2"]
for _ in range(n):
    num, s = input(), 0
    for i in range(17):
        s += int(num[i]) * coeff[i]
    print("YES" if num[-1] == res[s % 11] else "NO")
```



代码运行截图

![截屏2024-10-11 13.45.12](https://raw.githubusercontent.com/AlbertJ-314/img/main/202410111346065.png)



### M28678: 角谷猜想

http://cs101.openjudge.cn/practice/28678/



代码

```python
n = int(input())
while n != 1:
    if n % 2:
        print(f"{n}*3+1={n * 3 + 1}")
        n = n * 3 + 1
    else:
        print(f"{n}/2={n // 2}")
        n //= 2
print("End")
```



代码运行截图

![截屏2024-10-11 13.47.57](https://raw.githubusercontent.com/AlbertJ-314/img/main/202410111348739.png)



### M28700: 罗马数字与整数的转换

http://cs101.openjudge.cn/practice/28700/



代码

```python
s = input()
trans1 = {1: 'I', 4: 'IV', 5: 'V', 9: 'IX', 10: 'X', 40: 'XL', 50: 'L', 90: 'XC', 100: 'C', 400: 'CD', 500: 'D', 900: 'CM', 1000: 'M'}
check1 = [1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1]
trans2 = {"I": 1, "V": 5, "X": 10, "L": 50, "C": 100, "D": 500, "M": 1000}
check2 = {"I": ["V", "X"], "X": ["L", "C"], "C": ["D", "M"]}
if s.isdecimal():
    s = int(s)
    for num in check1:
        cnt = s // num
        s %= num
        print(trans1[num]*cnt,end="")
    print()
else:
    i, ans = 0, 0
    while i < len(s):
        if s[i] == "V" or s[i] == "L" or s[i] == "D" or s[i] == "M":
            ans += trans2[s[i]]
        elif i != len(s) - 1 and s[i + 1] in check2[s[i]]:
            ans += trans2[s[i + 1]] - trans2[s[i]]
            i += 1
        else:
            ans += trans2[s[i]]
        i += 1
    print(ans)
```



代码运行截图

![截屏2024-10-11 13.52.31](https://raw.githubusercontent.com/AlbertJ-314/img/main/202410111353680.png)



### *T25353: 排队 （选做）

http://cs101.openjudge.cn/practice/25353/



代码

```python
def qsort(nums, dis):
    if len(nums) <= 1:
        return nums
    left, right = [], []
    cur_min, cur_max = nums[0], nums[0]
    for i in range(1, len(nums)):
        if abs(nums[i] - cur_min) <= dis and abs(nums[i] - cur_max) <= dis and nums[i] < nums[0]:
            left.append(nums[i])
        else:
            right.append(nums[i])
            cur_min = min(cur_min, nums[i])
            cur_max = max(cur_max, nums[i])
    return qsort(left, dis) + [nums[0]] + qsort(right, dis)


n, d = map(int, input().split())
arr = [0 for _ in range(n)]
for i in range(n):
    arr[i] = int(input())
print("\n".join(map(str, qsort(arr, d))))
```



代码运行截图

![截屏2024-10-11 13.54.33](https://raw.githubusercontent.com/AlbertJ-314/img/main/202410111355337.png)



## 2. 学习总结和收获

复习了⼀下双指针。

额外练习：每⽇选做的所有题，以及 LeetCode 上⼀些题⽬，⽐如 167, 344, 345, 125, 11, 611, 15, 16, 18, 259, 

658, 1099, 75, 360, 977, 881, 42, 443, 26, 80, 27, 283, 845, 88, 719, 334, 978。
