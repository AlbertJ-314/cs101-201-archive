# Assignment #2: 深度学习与大语言模型

Updated 2204 GMT+8 Feb 25, 2025



## 1. 题目

### 18161: 矩阵运算

matrices, http://cs101.openjudge.cn/practice/18161

代码：

```python
a, b, c = [], [], []
ma, na = map(int, input().split())
for i in range(ma):
    a.append(list(map(int, input().split())))
mb, nb = map(int, input().split())
for i in range(mb):
    b.append(list(map(int, input().split())))
mc, nc = map(int, input().split())
for i in range(mc):
    c.append(list(map(int, input().split())))
if na != mb or ma != mc or nb != nc:
    print("Error!")
else:
    for i in range(ma):
        for j in range(nb):
            c[i][j] += sum([a[i][k] * b[k][j] for k in range(mb)])
    for i in range(mc):
        print(" ".join(map(str, c[i])))
```



代码运行截图

![截屏2025-02-27 13.11.18](https://raw.githubusercontent.com/AlbertJ-314/img/main/202502271312339.png)



### 19942: 二维矩阵上的卷积运算

matrices, http://cs101.openjudge.cn/practice/19942/

代码：

```python
m, n, p, q = map(int, input().split())
mat, ker = [],[]
for _ in range(m):
    mat.append(list(map(int, input().split())))
for _ in range(p):
    ker.append(list(map(int, input().split())))
ans = [[0 for _ in range(n - q + 1)] for _ in range(m - p + 1)]
for i in range(m - p + 1):
    for j in range(n - q + 1):
        temp = 0
        for x in range(i, i + p):
            for y in range(j, j + q):
                temp += mat[x][y] * ker[x - i][y - j]
        ans[i][j] = temp
print("\n".join((map(lambda row : " ".join(map(str, row)), ans))))
```



代码运行截图

![截屏2025-02-27 13.13.13](https://raw.githubusercontent.com/AlbertJ-314/img/main/202502271313966.png)



### 04140: 方程求解

牛顿迭代法，http://cs101.openjudge.cn/practice/04140/

代码：

```python
x = 6
for _ in range(4):
    x = 2 * x / 3 + 5 / 9 - (10 * x - 670) / (27 * x ** 2 - 90 * x + 90)
print(f"{x:.9f}")
```



代码运行截图

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202502271328296.png)



### 06640: 倒排索引

data structures, http://cs101.openjudge.cn/practice/06640/

代码：

```python
from collections import defaultdict
index = defaultdict(str)
for i in range(int(input())):
    for word in set(input().split()[1:]):
        index[word] += " " + str(i + 1)
for _ in range(int(input())):
    query = input()
    print(index[query][1:] if index[query] else "NOT FOUND")
```



代码运行截图

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202502271316962.png)



### 04093: 倒排索引查询

data structures, http://cs101.openjudge.cn/practice/04093/

代码：

```python
index, books = [], set()
for _ in range(int(input())):
    index.append(set(map(int, input().split()[1:])))
    books |= index[-1]
for _ in range(int(input())):
    res = books.copy()
    for i, q in enumerate(map(int, input().split())):
        if q == 1:
            res &= index[i]
        elif q == -1:
            res -= index[i]
    print(" ".join(map(str, sorted(res))) if res else "NOT FOUND")
```



代码运行截图

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202502271318323.png)



## 2. 学习总结和个人收获

寒假给Leetcode加强数据赚了100个欢乐豆🥳

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202503022215582.png)
