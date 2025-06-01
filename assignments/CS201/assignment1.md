# Assignment #1: 虚拟机，Shell & 大语言模型

Updated 2309 GMT+8 Feb 20, 2025



## 1. 题目

### 27653: Fraction类

http://cs101.openjudge.cn/practice/27653/

代码：

```python
from math import gcd
class Fraction:
    def __init__(self, numerator: int, denominator: int):
        self.numerator = numerator
        self.denominator = denominator

    def __str__(self):
        return f"{self.numerator}/{self.denominator}"

    def __add__(self, other):
        num = self.numerator * other.denominator + self.denominator * other.numerator
        denom = self.denominator * other.denominator
        return Fraction(num // gcd(num, denom), denom // gcd(num, denom))

nums = list(map(int, input().split()))
a, b = Fraction(nums[0], nums[1]), Fraction(nums[2], nums[3])
print(a + b)
```



代码运行截图

![截屏2025-02-22 14.34.23](https://raw.githubusercontent.com/AlbertJ-314/img/main/202502221435961.png)



### 1760.袋子里最少数目的球

 https://leetcode.cn/problems/minimum-limit-of-balls-in-a-bag/

代码：

```python
class Solution:
    def minimumSize(self, nums: List[int], maxOperations: int) -> int:
        l, r = 1, max(nums)
        while l < r:
            mid = (l + r) // 2
            if sum((num - 1) // mid for num in nums) > maxOperations:
                l = mid + 1
            else:
                r = mid
        return l
```



代码运行截图

![截屏2025-02-22 15.06.12](https://raw.githubusercontent.com/AlbertJ-314/img/main/202502221506614.png)



### 04135: 月度开销

http://cs101.openjudge.cn/practice/04135

代码：

```python
n, m = map(int, input().split())
expd = [int(input()) for _ in range(n)]
l, r = max(expd), sum(expd)
while l < r:
    mid, cnt, cur = (l + r) // 2, 1, 0
    for num in expd:
        if cur + num > mid:
            cnt, cur = cnt + 1, 0
        cur += num
    if cnt > m:
        l = mid + 1
    else:
        r = mid
print(l)
```



代码运行截图

![截屏2025-02-22 14.45.25](https://raw.githubusercontent.com/AlbertJ-314/img/main/202502221445216.png)



### 27300: 模型整理

http://cs101.openjudge.cn/practice/27300/

代码：

```python
from collections import defaultdict
models = [tuple(input().split("-")) for _ in range(int(input()))]
comp = defaultdict(list)
for name, para in models:
    num = float(para[:-1]) * (10 ** 6 if para[-1] == "M" else 10 ** 9)
    comp[name].append((num, para))
for name in sorted(comp.keys()):
    print(f"{name}:", end=" ")
    print(", ".join(map(lambda x: x[1], sorted(comp[name]))))
```



代码运行截图

![截屏2025-02-22 14.55.21](https://raw.githubusercontent.com/AlbertJ-314/img/main/202502221456131.png)



## 2. 学习总结和个人收获

做了开学之后所有的每日选做，寒假的还没做完。
