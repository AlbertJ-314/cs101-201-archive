# Assignment #C: 202505114 Mock Exam

Updated 1518 GMT+8 May 14, 2025



> **说明：**
>
> 1. **⽉考**：AC6。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。
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

### E06364: 牛的选举

http://cs101.openjudge.cn/practice/06364/

代码：

```python
n, k = map(int, input().split())
votes = []
for i in range(n):
    a, b = map(int, input().split())
    votes.append((a, b, i + 1))
print(sorted(sorted(votes, reverse=True)[:k], key=lambda x: x[1])[-1][2])
```



代码运行截图

[![2025-05-14-19-49-14.png](https://i.postimg.cc/BZ77RBfY/2025-05-14-19-49-14.png)](https://postimg.cc/PNZMwZxY)



### M04077: 出栈序列统计

http://cs101.openjudge.cn/practice/04077/

代码：

```python
from math import comb
n = int(input())
print(comb(2 * n, n) // (n + 1))
```



代码运行截图

[![2025-05-14-19-50-27.png](https://i.postimg.cc/2ShHbpbM/2025-05-14-19-50-27.png)](https://postimg.cc/4Y4vqS1Q)



### M05343:用队列对扑克牌排序

http://cs101.openjudge.cn/practice/05343/

代码：

```python
n, cards = int(input()), list(input().split())
nums, new_cards = [[] for _ in range(10)], []
for card in cards:
    nums[int(card[1])].append(card[0])
for i in range(1, 10):
    print(f"Queue{i}:", end="")
    print(" ".join(map(lambda x: x + str(i), nums[i])))
    new_cards.extend(list(map(lambda x: x + str(i), nums[i])))
alphas = {alpha: [] for alpha in ("A", "B", "C", "D")}
for card in new_cards:
    alphas[card[0]].append(int(card[1]))
for alpha in ("A", "B", "C", "D"):
    print(f"Queue{alpha}:", end="")
    print(" ".join(map(lambda x: alpha + str(x), alphas[alpha])))
print(*sorted(cards))
```



代码运行截图

[![2025-05-14-19-52-34.png](https://i.postimg.cc/3xFcf58p/2025-05-14-19-52-34.png)](https://postimg.cc/LJhDhWZ6)



### M04084: 拓扑排序

http://cs101.openjudge.cn/practice/04084/

代码：

```python
import heapq
from itertools import filterfalse
n, m = map(int, input().split())
graph, indegrees = [[] for _ in range(n)], [0] * n
for _ in range(m):
    u, v = map(int, input().split())
    graph[u - 1].append(v - 1)
    indegrees[v - 1] += 1
S, order = list(filterfalse(lambda x: indegrees[x], range(n))), []
heapq.heapify(S)
while S:
    u = heapq.heappop(S)
    order.append(u)
    for v in sorted(graph[u]):
        indegrees[v] -= 1
        if indegrees[v] == 0:
            heapq.heappush(S, v)
print(" ".join(map(lambda i: f"v{i + 1}", order)))
```



代码运行截图

[![2025-05-14-19-54-19.png](https://i.postimg.cc/Y9ZnLtRc/2025-05-14-19-54-19.png)](https://postimg.cc/gXyqfF14)



### M07735:道路

Dijkstra, http://cs101.openjudge.cn/practice/07735/

代码：

```python
from math import inf
import heapq
K, N, R = int(input()), int(input()), int(input())
graph = [[] for _ in range(N)]
for _ in range(R):
    S, D, L, T = map(int, input().split())
    graph[S - 1].append((D - 1, L, T))
queue, rec, res = [(0, 0, 0)], {i: inf for i in range(N)}, None
while queue:
    dis, cost, u = heapq.heappop(queue)
    if u == N - 1:
        res = dis; break
    if rec[u] <= cost:
        continue
    rec[u] = cost
    for v, l, t in graph[u]:
        if cost + t <= K:
            heapq.heappush(queue, (dis + l, cost + t, v))
print(res if res else -1)
```



代码运行截图

[![2025-05-14-19-59-15.png](https://i.postimg.cc/598qS5NQ/2025-05-14-19-59-15.png)](https://postimg.cc/w3Tm6mN9)



### T24637:宝藏二叉树

dp, http://cs101.openjudge.cn/practice/24637/

代码：

```python
from functools import lru_cache
@lru_cache(maxsize=None)
def dp(node: int, flag: bool) -> int:
    if node > n:
        return 0
    res = dp(2 * node, True) + dp(2 * node + 1, True)
    return max(res, val[node - 1] + dp(2 * node, False) + dp(2 * node + 1, False)) if flag else res

n, val = int(input()), list(map(int, input().split()))
print(dp(1, True))
```



代码运行截图

[![2025-05-14-19-56-49.png](https://i.postimg.cc/65Lctrkx/2025-05-14-19-56-49.png)](https://postimg.cc/xc8MPzj6)



## 2. 学习总结和收获

每日选做 + Leetcode: 207, 210, 802, 851, 1136, 2050。
