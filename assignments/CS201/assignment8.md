# Assignment #8: 树为主

Updated 1704 GMT+8 Apr 8, 2025



## 1. 题目

### LC108.将有序数组转换为二叉树

dfs, https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/

代码：

```python
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        if not nums:
            return None
        mid = len(nums) // 2
        return TreeNode(nums[mid], self.sortedArrayToBST(nums[:mid]), self.sortedArrayToBST(nums[mid + 1:]))
```



代码运行截图

[![2025-04-10-13-11-19.png](https://i.postimg.cc/BnWXrH04/2025-04-10-13-11-19.png)](https://postimg.cc/ZCcb39nM)



### M27928:遍历树

 adjacency list, dfs, http://cs101.openjudge.cn/practice/27928/

代码：

```python
def traverse(curr: int) -> None:
    for node in sorted(children[curr] + [curr]):
        if node == curr:
            print(node)
        else:
            traverse(node)

children, nonroot = {}, set()
for _ in range(int(input())):
    nums = list(map(int, input().split()))
    children[nums[0]] = nums[1:]
    nonroot.update(nums[1:])
for num in children.keys():
    if num not in nonroot:
        traverse(num)
```



代码运行截图

[<img src="https://i.postimg.cc/Pq1rn5jc/2025-04-10-13-52-27.png" alt="2025-04-10-13-52-27.png"  />](https://postimg.cc/yJYzFBWX)



### LC129.求根节点到叶节点数字之和

dfs, https://leetcode.cn/problems/sum-root-to-leaf-numbers/

代码：

```python
class Solution:
    def __init__(self):
        self.ans = 0

    def dfs(self, node: "TreeNode", path: "") -> int:
        path += str(node.val)
        if not node.left and not node.right:
            self.ans += int(path); return
        if node.left:
            self.dfs(node.left, path)
        if node.right:
            self.dfs(node.right, path)

    def sumNumbers(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        self.dfs(root, "")
        return self.ans
```



代码运行截图

[![2025-04-10-13-54-06.png](https://i.postimg.cc/JhMStrXS/2025-04-10-13-54-06.png)](https://postimg.cc/CRrcQVm4)



### M22158:根据二叉树前中序序列建树

tree, http://cs101.openjudge.cn/practice/22158/

代码：

```python
from typing import Optional

class BinNode:
    def __init__(self, val="", left=None, right=None):
        self.val = val
        self.left, self.right = left, right

def buildTree(preorder: str, inorder: str) -> Optional[BinNode]:
    if not inorder or not postorder:
        return None
    root_val = preorder[0]
    ind = inorder.index(root_val)
    return BinNode(root_val, buildTree(preorder[1:ind + 1], inorder[:ind]), buildTree(preorder[ind + 1:], inorder[ind + 1:]))

def postorder(root: BinNode) -> str:
    if not root:
        return ""
    return postorder(root.left) + postorder(root.right) + root.val

while True:
    try:
        tree = buildTree(input(), input())
        print(postorder(tree))
    except EOFError:
        break
```



代码运行截图

[<img src="https://i.postimg.cc/s2nyrQLT/2025-04-10-13-55-42.png" alt="2025-04-10-13-55-42.png"  />](https://postimg.cc/bG240Jpn)



### M24729:括号嵌套树

dfs, stack, http://cs101.openjudge.cn/practice/24729/

代码：

```python
def comma_index(expr: str):
    stack, res = [], []
    for i in range(2, len(expr) - 1):
        if expr[i] == "(":
            stack.append("(")
        elif expr[i] == ")":
            stack.pop()
        elif expr[i] == "," and not stack:
            yield i
    return res

def preorder(expr: str) -> str:
    if len(expr) == 1:
        return expr
    cur, res = 2, expr[0]
    for i in comma_index(expr):
        res += preorder(expr[cur:i])
        cur = i + 1
    return res + preorder(expr[cur:-1])

def postorder(expr: str) -> str:
    if len(expr) == 1:
        return expr
    cur, res = 2, ""
    for i in comma_index(expr):
        res += postorder(expr[cur:i])
        cur = i + 1
    return res + postorder(expr[cur:-1]) + expr[0]

s = input()
print(preorder(s), postorder(s), sep='\n')
```



代码运行截图

[<img src="https://i.postimg.cc/V6dwLsQm/2025-04-10-13-56-22.png" alt="2025-04-10-13-56-22.png"  />](https://postimg.cc/dkPp5YVx)



### LC3510.移除最小数对使数组有序II

doubly-linked list + heap, https://leetcode.cn/problems/minimum-pair-removal-to-sort-array-ii/

代码：

```python
class Solution:
    def minimumPairRemoval(self, nums: List[int]) -> int:
        pred, succ = list(range(-1, len(nums)-1)), list(range(1, len(nums)+1))
        succ[-1] = -1
        heap, inv, cnt = [], 0, 0
        for i in range(len(nums)-1):
            inv += nums[i] > nums[i + 1]
            heappush(heap, (nums[i] + nums[i + 1], i))
        while inv:
            s, idx = heappop(heap)
            if succ[idx] == -1 or nums[idx] + nums[succ[idx]] != s:
                continue
            if nums[idx] > nums[succ[idx]]:
                inv -= 1
            l, r = pred[idx], succ[succ[idx]]
            if l != -1:
                inv += (nums[l] > s) - (nums[l] > nums[idx])
                heappush(heap, (nums[l] + s, l))
            if r != -1:
                inv += (s > nums[r]) - (nums[succ[idx]] > nums[r])
                heappush(heap, (s + nums[r], idx))
            nums[idx], nums[succ[idx]] = s, inf
            pred[succ[succ[idx]]], succ[idx] = idx, succ[succ[idx]]
            cnt += 1
        return cnt
```



代码运行截图

[<img src="https://i.postimg.cc/FzYR30Y6/2025-04-10-15-52-35.png" alt="2025-04-10-15-52-35.png"  />](https://postimg.cc/CnVSVBHC)



## 2. 学习总结和收获

每日选做：✅，期中后加练💪
