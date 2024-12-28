## Merge sort

```python
from typing import List

def merge(left_nums: List[int], right_nums: List[int]) -> List[int]:
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

def mergesort(nums: List[int]) -> List[int]:
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
from typing import List, Tuple

def partition(nums: List[int], start: int, end: int) -> Tuple[int, int]:
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


def quicksort(nums: List[int], start: int, end: int) -> None:
    if end - 1 <= start:
        return
    l, r = partition(nums, start, end)
    quicksort(nums, start, r + 1)
    quicksort(nums, l, end)
```

Time complexity: $O\left(n\log n\right)$ on average,  $O\left(n^2\right)$ in the worst case. Here $n$ is the length of the array.

Space complexity: $O\left(1\right)$.

Quick sort is unstable.
