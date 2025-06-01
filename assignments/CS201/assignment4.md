# Assignment #4: 位操作、栈、链表、堆和NN

Updated 1203 GMT+8 Mar 10, 2025



## 1. 题目

### 136.只出现一次的数字

bit manipulation, https://leetcode.cn/problems/single-number/

代码：

```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        return reduce(lambda x, y: x^y, nums)
```



代码运行截图

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202503101638964.png)



### 20140:今日化学论文

stack, http://cs101.openjudge.cn/practice/20140/

代码：

```python
compressed, stack = input(), []
for char in compressed:
    if char == "[":
        stack.append(0)
    elif char.isdigit():
        stack[-1] = stack[-1] * 10 + int(char)
    elif char == "]":
        copied = stack.pop() * stack.pop()
        if not stack or type(stack[-1]) == int:
            stack.append("")
        stack[-1] += copied
    else:
        if not stack or type(stack[-1]) == int:
            stack.append("")
        stack[-1] += char
print(stack[0])
```



代码运行截图

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202503101610075.png)



### 160.相交链表

linked list, https://leetcode.cn/problems/intersection-of-two-linked-lists/

代码：

```python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> Optional[ListNode]:
        p, q = headA, headB
        while p != q:
            p = p.next if p else headB
            q = q.next if q else headA
        return p
```



代码运行截图

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202503101639044.png)



### 206.反转链表

linked list, https://leetcode.cn/problems/reverse-linked-list/

代码：

```python
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return head
        node = self.reverseList(head.next)
        head.next.next, head.next = head, None
        return node
```



代码运行截图

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202503101636100.png)



### 3478.选出和最大的K个元素

heap, https://leetcode.cn/problems/choose-k-elements-with-maximum-sum/

代码：

```python
class Solution:
    def findMaxSum(self, nums1: List[int], nums2: List[int], k: int) -> List[int]:
        combined = defaultdict(list)
        for num1, num2 in zip(nums1, nums2):
            combined[num1].append(num2)
        heap, cur, res = [], 0, {}
        for num1 in sorted(combined.keys()):
            res[num1] = cur
            for num2 in combined[num1]:
                heapq.heappush(heap, num2)
                cur += num2
                if len(heap) > k:
                    cur -= heapq.heappop(heap)
        return  list(map(lambda x: res[x], nums1))
```



代码运行截图

![](https://raw.githubusercontent.com/AlbertJ-314/img/main/202503101646238.png)



## 2. 学习总结和收获

每日选做 + Leetcode: 25, 61, 82, 83, 92, 138, 203, 234, 206, 328, 430, 707。
