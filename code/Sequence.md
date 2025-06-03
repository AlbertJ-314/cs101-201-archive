## [Single-interval scheduling maximization](https://en.wikipedia.org/wiki/Interval_scheduling#Single-Interval_Scheduling_Maximization)

```python
from math import inf

def interval_scheduling(intervals: list[list[int]]) -> int:
    cur, cnt = -inf, 0
    for s, e in sorted(intervals, key=lambda x: x[1]):
        if cur <= s:
            cur = e; cnt += 1
    return cnt
```

Time complexity: $O\left(n\log n\right)$, where $n$ is the number of intervals.

Space complexity: $O\left(\log n\right)$ average and $O\left(n\right)$ worst-case (due to `sorted()`).



## Maximum subarray

#### [Kadane's algorithm](https://en.wikipedia.org/wiki/Maximum_subarray_problem#Kadane's_algorithm)

```python
from math import inf

def max_subarray(nums: list[int]) -> int:
    cur, ans = 0, -inf
    for num in nums:
        cur = cur + num if cur > 0 else num
        ans = max(ans, cur)
    return ans
```

Time complexity: $O\left(n\right)$, where $n$ is the length of the array.

Space complexity: $O\left(1\right)$.



## Longest increasing subsequence

#### Dynamic programming

```python
from bisect import bisect_left
from math import inf

def LIS(nums: list[int]) -> int:
  	# dp[i]: the possible minimum value for the end of an increasing subsequence of length i.
    dp = [-inf] + [inf for _ in range(len(nums) + 1)]
    for num in nums:
        dp[bisect_left(dp, num)] = num
    return dp.index(inf) - 1
```

Time complexity: $O\left(n\log n\right)$, where $n$ is the length of the array.

Space complexity: $O\left(n\right)$.



#### Greedy

```python
from bisect import bisect_left
from math import inf

def LIS(nums: list[int]) -> int:
    seq = [-inf]
    for num in nums:
        if num > seq[-1]:
            seq.append(num)
        else:
            seq[bisect_left(seq, num)] = num
    return len(seq) - 1
```

Time complexity: $O\left(n\log n\right)$, where $n$ is the length of the array.

Space complexity: $O\left(n\right)$.



## Longest common subsequence

```python
def LCS(s1: str, s2: str) -> int:
    n1, n2 = len(s1), len(s2)
    if n1 < n2:
        return LCS(s2, s1)
    # dp[i][j]: length of the longest common subsequence of s1[:i] and s2[:j].
    # Optimized space
    dp = [0 for _ in range(n2 + 1)]
    for c in s1:
        rec = (0, 0)
        for j in range(1, n2 + 1):
            rec = (rec[1], dp[j])
            if s2[j - 1] == c:
                dp[j] = rec[0] + 1
            else:
                dp[j] = max(rec[1], dp[j - 1])
    return dp[n2]
```

Time complexity: $O\left(n_1\times n_2\right)$, where $n_1,n_2$ are lengths of the two strings respectively.

Space complexity: $O\left(\min\{n_1,n_2\}\right)$.



## Longest palindromic substring

#### [Manacher's algorithm](https://en.wikipedia.org/wiki/Longest_palindromic_substring#Manacher's_algorithm)

```python
def LPS(s: str) -> str:
    t = "." + ".".join(s) + "."
    n, i, r = len(t), 0, 0
    maxr = [0 for _ in range(n)]
    while i < n:
        while r < min(i, n - i - 1) and t[i - r - 1] == t[i + r + 1]:
            r += 1
        maxr[i], j = r, i + 1
        while j <= i + r:
            if maxr[2 * i - j] == r + i - j:
                r += i - j; break
            maxr[j] = min(maxr[2 * i - j], r + i - j)
            j += 1
        if j > i + r:
            r = 0
        i = j
    ind = 0
    for i in range(n):
        if maxr[i] > maxr[ind]:
            ind = i
    return t[ind - maxr[ind]: ind + maxr[ind] + 1].replace(".", "")
```

Time complexity: $O\left(n\right)$, where $n$ is the length of the string.

Space complexity: $O\left(n\right)$.



## Longest palindromic subsequence

```python
def LPS(s: str) -> int:
    n = len(s)
    # dp[i][j]: length of the longest palindromic subsequence of s[i:j + 1].
    # Optimized space
    dp = [0 for _ in range(n)]
    for i in range(n - 1, -1, -1):
        dp[i], rec = 1, 0
        for j in range(i + 1, n):
            if s[i] == s[j]:
                dp[j], rec = rec + 2, dp[j]
            else:
                dp[j], rec = max(dp[j], dp[j - 1]), dp[j]
    return dp[n - 1]
```

Time complexity: $O\left(n^2\right)$, where $n$ is the length of the string.

Space complexity: $O\left(n\right)$.



#### Reducing to longest common sequence

The longest palindromic subsequence of `s` has the same length as the longest common sequence between `s` and `s[::-1]`.
