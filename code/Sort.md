## Bucket sort

```python
def bucketsort(nums: list[int]) -> list[int]:
    buckets_size = 100
    max_nums, min_nums = max(nums), min(nums)
    buckets_len = (max_nums - min_nums) // buckets_size + 1
    buckets = [[] for _ in range(buckets_len)]
    for num in nums:
        buckets[(num - min_nums) // buckets_size].append(num)
    res = []
    for bucket in buckets:
        res += sorted(bucket)
    return res
```

Time complexity: $O\left(n\log\frac{n}{k}\right)$ when elements are uniformly distributed across buckets.

Here $n$ denotes the length of the array and $k$ denotes the number of buckets.

Space complexity: $O\left(n + k\right)$.

Stablity is the same as the algorithm used within each bucket.



## Counting sort

```python
from collections import Counter

def countingsort(nums: list[int]) -> list[int]:
    nums_min, nums_max = min(nums), max(nums)
    cnt, ans = Counter(nums), []
    for num in range(nums_min, nums_max + 1):
        ans.extend([num] * cnt[num])
    return ans
```

Time complexity: $O\left(n + m\right)$, where $n$ is the length of the array and $m$ denotes the range of numbers (i.e. max minus min).

Space complexity: $O\left(n\right)$.



## Merge sort

```python
def merge(left_nums: list[int], right_nums: list[int]) -> list[int]:
    merge_nums = []
    l, r = 0, 0
    while l < len(left_nums) and r < len(right_nums):
        if left_nums[l] <= right_nums[r]:
            merge_nums.append(left_nums[l])
            l += 1
        else:
            merge_nums.append(right_nums[r])
            r += 1
    while l < len(left_nums):
        merge_nums.append(left_nums[l])
        l += 1
    while r < len(right_nums):
        merge_nums.append(right_nums[r])
        r += 1
    return merge_nums

def mergesort(nums: list[int]) -> list[int]:
    if len(nums) <= 1:
        return nums
    return merge(mergesort(nums[0: len(nums) // 2]), mergesort(nums[len(nums) // 2:]))
```

Time complexity: $O\left(n\log n\right)$, where $n$ is the length of the array.

Space complexity: $O\left(n\right)$.

Merge sort is stable.



## Quick sort

```python
import random

def partition(nums: list[int], start: int, end: int) -> tuple[int, int]:
    l, r = start, end - 1
    pivot = nums[random.randint(start, end - 1)]
    while l <= r: # l <= r CANNOT be changed into l < r
        while l <= r and nums[r] > pivot:
            r -= 1
        while l <= r and nums[l] < pivot:
            l += 1
        if l <= r:
            nums[l], nums[r] = nums[r], nums[l]
            l += 1; r -= 1
    return l, r


def quicksort(nums: list[int], start: int, end: int) -> None:
    if end - 1 <= start:
        return
    l, r = partition(nums, start, end)
    quicksort(nums, start, r + 1)
    quicksort(nums, l, end)
```

Time complexity: $O\left(n\log n\right)$ on average,  $O\left(n^2\right)$ in the worst case. Here $n$ is the length of the array.

Space complexity: $O\left(\log n\right)$ average and $O\left(n\right)$ worst-case.

Quick sort is unstable.



## Shell sort

```python
def shellsort(nums: list[int]) -> list[int]:
    gap = len(nums) // 2
    while gap > 0:
        for i in range(gap, len(nums)):
            temp, j = nums[i], i
            while j >= gap and nums[j - gap] > temp:
                nums[j] = nums[j - gap]
                j -= gap
            nums[j] = temp
        gap //= 2
    return nums
```

Time complexity: depends on the choice of [gap sequence](https://en.wikipedia.org/wiki/Shellsort#Gap_sequences).

Space complexity: $O\left(1\right)$.

Shell sort is unstable.
