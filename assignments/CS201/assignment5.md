# Assignment #5: 链表、栈、队列和归并排序

Updated 1348 GMT+8 Mar 17, 2025



## 1. 题目

### LC21.合并两个有序链表

linked list, https://leetcode.cn/problems/merge-two-sorted-lists/

代码：

```python
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        if not list1:
            return list2
        if not list2:
            return list1
        if list1.val <= list2.val:
            return ListNode(list1.val, self.mergeTwoLists(list1.next, list2))
        else:
            return ListNode(list2.val, self.mergeTwoLists(list1, list2.next))
```



代码运行截图

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202503192119441.png)



### LC234.回文链表

linked list, https://leetcode.cn/problems/palindrome-linked-list/

代码：

```python
class Solution:
    def isPalindrome(self, head: Optional[ListNode]) -> bool:
        if not head:
            return True
        fast, slow = head, head
        while fast.next and fast.next.next:
            fast, slow = fast.next.next, slow.next
        left, right = head, self.reverse(slow.next)
        while right:
            if left.val != right.val:
                return False
            left, right = left.next, right.next
        return True

    def reverse(self, head: Optional[ListNode]) -> Optional[ListNode]:
        prev, curr = None, head
        while curr:
            next_node = curr.next
            curr.next, prev, curr = prev, curr, next_node
        return prev
```



代码运行截图

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202503201119738.png)



### LC1472.设计浏览器历史记录

doubly-lined list, https://leetcode.cn/problems/design-browser-history/

代码：

```python
class Website:
    def __init__(self, url: str):
        self.url = url
        self.prev, self.next = None, None

class BrowserHistory:
    def __init__(self, homepage: str):
        self.curr = Website(homepage)

    def visit(self, url: str) -> None:
        self.curr.next = Website(url)
        self.curr.next.prev = self.curr
        self.curr = self.curr.next

    def back(self, steps: int) -> str:
        for _ in range(steps):
            if not self.curr.prev:
                break
            self.curr = self.curr.prev
        return self.curr.url

    def forward(self, steps: int) -> str:
        for _ in range(steps):
            if not self.curr.next:
                break
            self.curr = self.curr.next
        return self.curr.url
```



代码运行截图

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202503201321168.png)



### 24591: 中序表达式转后序表达式

stack, http://cs101.openjudge.cn/practice/24591/

代码：

```python
from typing import List
def infix_to_postfix(tokens: List[str]) -> List[str]:
    precedence = {'+': 1, '-': 1, '*': 2, '/': 2}
    stack, output = [], []
    for token in tokens:
        if token == "(":
            stack.append(token)
        elif token == ")":
            while stack and stack[-1] != "(":
                output.append(stack.pop())
            stack.pop()
        elif token in precedence:
            while stack and stack[-1] != "(" and precedence[stack[-1]] >= precedence[token]:
                output.append(stack.pop())
            stack.append(token)
        else:
            output.append(token)
    while stack:
        output.append(stack.pop())
    return output


n = int(input())
for _ in range(n):
    expr, tokens = input(), []
    for c in expr:
        if c not in "+-*/()" and tokens and tokens[-1] not in "+-*/()":
            tokens[-1] += c
        else:
            tokens.append(c)
    print(" ".join(infix_to_postfix(tokens)))
```



代码运行截图

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202503192120051.png)



### 03253: 约瑟夫问题No.2

queue, http://cs101.openjudge.cn/practice/03253/

代码：

```python
from collections import deque
while True:
    n, p, m = map(int, input().split())
    if (n, p, m) == (0, 0, 0):
        break
    queue = deque(range(1, n + 1))
    queue.rotate(1 - p)
    res = []
    while queue:
        queue.rotate(1 - m)
        res.append(queue.popleft())
    print(*res, sep=",")
```



代码运行截图

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202503201330934.png)



### 20018: 蚂蚁王国的越野跑

merge sort, http://cs101.openjudge.cn/practice/20018/

代码：

```python
from typing import List
cnt = 0
def merge(left_nums: List[int], right_nums: List[int]) -> List[int]:
    global cnt
    merge_nums, l, r = [], 0, 0
    while l < len(left_nums) and r < len(right_nums):
        if left_nums[l] <= right_nums[r]:
            merge_nums.append(left_nums[l])
            l += 1; cnt += r
        else:
            merge_nums.append(right_nums[r])
            r += 1
    while l < len(left_nums):
        merge_nums.append(left_nums[l])
        l += 1; cnt += r
    while r < len(right_nums):
        merge_nums.append(right_nums[r])
        r += 1
    return merge_nums

def mergesort(nums: List[int]) -> List[int]:
    if len(nums) <= 1:
        return nums
    return merge(mergesort(nums[0: len(nums) // 2]), mergesort(nums[len(nums) // 2:]))

v = [int(input()) for _ in range(int(input()))]
mergesort(v[::-1])
print(cnt)
```



代码运行截图

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202503201349094.png)



## 2. 学习总结和收获

每日选做 + LeetCode: 2, 19, 21, 23, 141, 142, 143, 147, 148, 160, 445, 876。
