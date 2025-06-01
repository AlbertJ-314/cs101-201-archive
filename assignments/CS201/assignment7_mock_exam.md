# Assignment #7: 20250402 Mock Exam

Updated 1624 GMT+8 Apr 2, 2025



> **说明：**
>
> 1. **⽉考**：AC6 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。
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

### E05344:最后的最后

http://cs101.openjudge.cn/practice/05344/

代码：

```python
n, k = map(int, input().split())
dead, kill = [False] * n, []
i, curr = -1, 0
for _ in range(n - 1):
    j, cnt = i, 0
    while cnt < k:
        j = (j + 1) % n
        if not dead[j]:
            cnt += 1
    i = j
    dead[i] = True
    kill.append(i + 1)
print(" ".join(map(str, kill)))
```



代码运行截图

[![2025-04-03-12-57-46.png](https://i.postimg.cc/ryCkp7xN/2025-04-03-12-57-46.png)](https://postimg.cc/jC59gg8D)



### M02774: 木材加工

binary search, http://cs101.openjudge.cn/practice/02774/

代码：

```python
n, k = map(int, input().split())
woods = [int(input()) for _ in range(n)]
l, r = 0, max(woods)
while l < r:
    mid = (l + r + 1) // 2
    if sum(wood // mid for wood in woods) < k:
        r = mid - 1
    else:
        l = mid
print(l)
```



代码运行截图

[![2025-04-03-12-55-00.png](https://i.postimg.cc/L69MgvVK/2025-04-03-12-55-00.png)](https://postimg.cc/BtVzdTyM)



### M07161:森林的带度数层次序列存储

tree, http://cs101.openjudge.cn/practice/07161/

代码：

```python
from collections import deque

class TreeNode:
    def __init__(self, val: str):
        self.val = val
        self.children = []

def postorder(root):
    if not root:
        return []
    res = []
    for child in root.children:
        res.extend(postorder(child))
    return res + [root.val]

seq = []
for _ in range(int(input())):
    tree = deque(input().split())
    root = TreeNode(tree.popleft())
    queue = deque([(root, int(tree.popleft()))])
    while tree:
        curr, deg = queue.popleft()
        for _ in range(deg):
            child = TreeNode(tree.popleft())
            curr.children.append(child)
            (queue.append((child, int(tree.popleft()))))
    seq.append(" ".join(postorder(root)))
print(" ".join(seq))
```



代码运行截图

[![2025-04-03-13-14-17.png](https://i.postimg.cc/4NdST05d/2025-04-03-13-14-17.png)](https://postimg.cc/Thz9VCBM)



### M18156:寻找离目标数最近的两数之和

two pointers, http://cs101.openjudge.cn/practice/18156/

代码：

```python
tar, nums = int(input()), sorted(list(map(int, input().split())))
l, r, s = 0, len(nums) - 1, nums[0] + nums[1]
while l < r:
    curr = nums[l] + nums[r]
    if abs(curr - tar) < abs(s - tar) or (s - tar == tar - curr and curr < s):
        s = curr
    if curr > tar:
        r -= 1
    else:
        l += 1
print(s)
```



代码运行截图

[![2025-04-03-13-15-50.png](https://i.postimg.cc/XYB1ryt6/2025-04-03-13-15-50.png)](https://postimg.cc/mPRyqhbm)



### M18159:个位为 1 的质数个数

sieve, http://cs101.openjudge.cn/practice/18159/

代码：

```python
from bisect import bisect_left

def eratosthenes_sieve(n: int):
    primes, is_prime = [], [True for _ in range(n)]
    for i in range(2, n):
        if is_prime[i]:
            primes.append(i)
            for k in range(i * i, n, i):
                is_prime[k] = False
    return primes

nums = []
for p in eratosthenes_sieve(10009):
    if p % 10 == 1:
        nums.append(p)

for case in range(int(input())):
    print(f"Case{case + 1}:")
    n = int(input())
    print(" ".join(map(str, nums[:bisect_left(nums, n)])) if n > 11 else "NULL")
```



代码运行截图

[![2025-04-03-13-16-30.png](https://i.postimg.cc/3wMSmxH4/2025-04-03-13-16-30.png)](https://postimg.cc/xXR3SY8n)



### M28127:北大夺冠

hash table, http://cs101.openjudge.cn/practice/28127/

代码：

```python
from collections import defaultdict, Counter

dashboard, cnt = defaultdict(set), Counter()
for _ in range(int(input())):
    name, prob, stat = input().split(",")
    if stat == "yes":
        dashboard[name].add(prob)
    cnt[name] += 1
    dashboard[name].add("dummy")

teams = []
for name, probs in dashboard.items():
    teams.append((1 - len(probs), cnt[name], name))
teams.sort()
for i, (num, sub, name) in enumerate(teams[:12]):
    print(i + 1, name, -num, sub)
```



代码运行截图

[![2025-04-03-13-18-51.png](https://i.postimg.cc/HxzBq08X/2025-04-03-13-18-51.png)](https://postimg.cc/wytXJmVj)



## 2. 学习总结和收获

第一题胡写了十几分钟，体现出这学期没太做题的人物特点☹️。
