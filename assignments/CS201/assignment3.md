# Assignment #3: 惊蛰 Mock Exam

Updated 1641 GMT+8 Mar 5, 2025



> **说明：**
>
> 1. **惊蛰⽉考**：AC5 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。
>
> 2. **解题与记录：**
>
>    对于每一个题目，请提供其解题思路（可选），并附上使用Python或C++编写的源代码（确保已在OpenJudge， Codeforces，LeetCode等平台上获得Accepted）。请将这些信息连同显示“Accepted”的截图一起填写到下方的作业模板中。（推荐使用Typora https://typoraio.cn 进行编辑，当然你也可以选择Word。）无论题目是否已通过，请标明每个题目大致花费的时间。
>
> 3. **提交安排：**提交时，请首先上传PDF格式的文件，并将.md或.doc格式的文件作为附件上传至右侧的“作业评论”区。确保你的Canvas账户有一个清晰可见的头像，提交的文件为PDF格式，并且“作业评论”区包含上传的.md或.doc附件。
>
> 4. **延迟提交：**如果你预计无法在截止日期前提交作业，请提前告知具体原因。这有助于我们了解情况并可能为你提供适当的延期或其他帮助。 
>
> 请按照上述指导认真准备和提交作业，以保证顺利完成课程要求。



## 1. 题目

### E04015: 邮箱验证

strings, http://cs101.openjudge.cn/practice/04015

代码：

```python
while True:
    try:
        email = input()
        if email.count("@") != 1 or email[0] in ("@", ".") or email[-1] in ("@", "."):
            print("NO")
        else:
            a, b = email.split("@")
            print("YES" if "." in b and "." not in (a[-1], b[0]) else "NO")
    except EOFError:
        break
```



代码运行截图

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202503051856954.png)



### M02039: 反反复复

implementation, http://cs101.openjudge.cn/practice/02039/

代码：

```python
n, s = int(input()), input()
m = len(s) // n
org, x, y, d = [[""] * n for _ in range(m)], 0, 0, 1
for i, char in enumerate(s):
    org[x][y] = char
    if (d == 1 and y == n - 1) or (d == -1 and y == 0):
        x += 1; d = -d
    else:
        y += d
print("".join("".join(org[i][j] for i in range(m)) for j in range(n)))
```



代码运行截图

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202503051858431.png)



### M02092: Grandpa is Famous

implementation, http://cs101.openjudge.cn/practice/02092/

代码：

```python
from collections import Counter
while True:
    n, m = map(int, input().split())
    if n == 0 and m == 0:
        break
    points = Counter()
    for _ in range(n):
        for player in map(int, input().split()):
            points[player] += 1
    res = sorted(points.items(), key=lambda x: x[1], reverse=True)
    nums =  []
    for player, point in res:
        if point == res[1][1]:
            nums.append(player)
    print(" ".join(map(str, sorted(nums))))
```



代码运行截图

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202503051900877.png)



### M04133: 垃圾炸弹

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

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202503051900129.png)



### T02488: A Knight's Journey

backtracking, http://cs101.openjudge.cn/practice/02488/

代码：

```python
directions = [(-2, -1), (-2, 1), (-1, -2), (-1, 2), (1, -2), (1, 2), (2, -1), (2, 1)]
exist_path = False

def dfs(x: int, y: int, path) -> None:
    global exist_path
    if len(path) == p * q:
        if not exist_path:
            exist_path = True
            print("".join(node[0] + str(node[1]) for node in path))
        return
    for dx, dy in directions:
        nx, ny = x + dx, y + dy
        if 0 <= nx < q and 0 <= ny < p and not vis[nx][ny]:
            vis[nx][ny] = True
            dfs(nx, ny, path + [(chr(ord("A") + nx), ny + 1)])
            vis[nx][ny] = False
            if exist_path:
                return

for t in range(int(input())):
    print(f"Scenario #{t + 1}:")
    p, q = map(int, input().split())
    vis, exist_path = [[False for _ in range(p)] for _ in range(q)], False
    for sx in range((q + 1) // 2):
        for sy in range((p + 1) // 2):
            vis[sx][sy] = True
            dfs(sx, sy, [(chr(ord("A") + sx), sy + 1)])
            vis[sx][sy] = False
            if exist_path:
                break
        if exist_path:
            break
    if not exist_path:
        print("impossible")
    print()
```



代码运行截图

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202503051907606.png)



### T06648: Sequence

heap, http://cs101.openjudge.cn/practice/06648/

代码：

```python
import heapq
for _ in range(int(input())):
    m, n = map(int, input().split())
    prev = sorted(map(int, input().split()))
    for _ in range(m - 1):
        curr = sorted(map(int, input().split()))
        heap, res = [(prev[i] + curr[0], i, 0) for i in range(n)], []
        heapq.heapify(heap)
        for _ in range(n):
            s, i, j = heapq.heappop(heap)
            res.append(s)
            if j + 1 < len(curr):
                heapq.heappush(heap, (prev[i] + curr[j + 1], i, j + 1))
        prev = res
    print(*prev)
```



代码运行截图

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202503052221715.png)



## 2. 学习总结和收获

现在每周碰一次数算，只来得及做完每日选做💔。

<img src="https://raw.githubusercontent.com/AlbertJ-314/img/main/202503052230631.jpeg" alt="截屏 2025-03-05 22.29.29" style="zoom:40%;" />
