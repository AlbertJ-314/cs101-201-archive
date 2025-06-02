## 0 - 1 knapsack

```python
def knapsack(weight: list[int], value: list[int], W: int) -> int:
    dp = [0] * (W + 1)
    for i in range(len(weight)):
        for w in range(W, weight[i] - 1, -1):
            dp[w] = max(dp[w], dp[w - weight[i]] + value[i])
    return dp[W]
```

Time complexity: $O\left(N \times W\right)$, where $N$ is the number of items.

Space complexity: $O\left(W\right)$.



#### Brute force

```python
def knapsack(weight: list[int], value: list[int], W: int) -> int:
    n, max_value = len(weight), 0
    for mask in range(1 << n):
        if sum(weight[i] for i in range(n) if mask & (1 << i)) <= W:
            max_value = max(max_value, sum(value[i] for i in range(n) if mask & (1 << i)))
    return max_value
```

Time complexity: $O\left(N \times 2^N\right)$, where $N$ is the number of items.

Space complexity: $O\left(1\right)$.



#### [Meet-in-the-middle](https://en.wikipedia.org/wiki/Knapsack_problem#Meet-in-the-middle)

```python
from bisect import bisect_right
from math import inf

def knapsack(weight: list[int], value: list[int], W: int) -> int:
    n = len(weight)
    lA, lB = (n + 1) // 2, n // 2
    B = []

    for mask in range(1 << lB):
        w = sum(weight[i + lA] for i in range(lB) if mask & (1 << i))
        v = sum(value[i + lA] for i in range(lB) if mask & (1 << i))
        B.append([w, v])

    B.sort()
    for i in range(1, 1 << lB):
        B[i][1] = max(B[i - 1][1], B[i][1])

    max_value = 0
    for mask in range(1 << lA):
        w = sum(weight[i] for i in range(lA) if mask & (1 << i))
        v = sum(value[i] for i in range(lA) if mask & (1 << i))
        if w <= W:
            idx = bisect_right(B, [W - w, inf]) - 1
            if idx >= 0:
                max_value = max(max_value, v + B[idx][1])

    return max_value
```

Time complexity: $O\left(N \times 2^\frac{N}{2}\right)$, where $N$ is the number of items.

Space complexity: $O\left(2^\frac{N}{2}\right)$.



## Unbounded knapsack

```python
def unbounded_knapsack(weight: list[int], value: list[int], W: int) -> int:
    dp = [0] * (W + 1)
    for i in range(len(weight)):
        for w in range(weight[i], W + 1):
            dp[w] = max(dp[w], dp[w - weight[i]] + value[i])
    return dp[W]
```

Time complexity: $O\left(N \times W\right)$ , where there is $N$ kinds of items.

Space complexity: $O\left(W\right)$.



## Bounded knapsack

```python
def bounded_knapsack(weight: list[int], value: list[int], count: list[int], W: int) -> int:
    dp = [0] * (W + 1)
    for i in range(len(weight)):
        for w in range(W, weight[i] - 1, -1):
            for k in range(min(count[i], w // weight[i]) + 1):
                dp[w] = max(dp[w], dp[w - k * weight[i]] + k * value[i])
    return dp[W]
```

Time complexity: $O\left(W \sum\limits_{i = 1}^N k_i\right)$ , where the $i$-th item has $k_i$ copies.

Space complexity: $O\left(W\right)$.



#### Binary splitting

```python
def bounded_knapsack(weight: list[int], value: list[int], count: list[int], W: int) -> int:
    weight_new, value_new = [], []
    for i, cnt in enumerate(count):
        k = 1
        while k <= cnt:
            weight_new.append(weight[i] * k)
            value_new.append(value[i] * k)
            cnt -= k
            k <<= 1
        if cnt:
            weight_new.append(weight[i] * cnt)
            value_new.append(value[i] * cnt)

    dp = [0] * (W + 1)
    for i in range(len(weight_new)):
        for w in range(W, weight_new[i] - 1, -1):
            dp[w] = max(dp[w], dp[w - weight_new[i]] + value_new[i])

    return dp[W]
```

Time complexity: $O\left(W \sum\limits_{i = 1}^N \log k_i\right)$ , where the $i$-th item has $k_i$ copies.

Space complexity: $O\left(W + \sum\limits_{i = 1}^N \log k_i\right)$.



#### Monoqueue

TBC



## 2D knapsack

```python
def two_dimensional_knapsack(weight: list[tuple[int, int]], value: list[int], W: tuple[int, int]) -> int:
    dp = [[0] * (W[1] + 1) for _ in range(W[0] + 1)]
    for i in range(len(weight)):
        for w0 in range(W[0], weight[i][0] - 1, -1):
            for w1 in range(W[1], weight[i][1] - 1, -1):
                dp[w0][w1] = max(dp[w0][w1], dp[w0 - weight[i][0]][w1 - weight[i][1]] + value[i])
    return dp[W[0]][W[1]]
```

Time complexity: $O\left(N \times W\left[0\right] \times W\left[1\right]\right)$ , where $N$ is the number of items.

Space complexity: $O\left(W\left[0\right] \times W\left[1\right]\right)$.



#### Alternative

```python
from math import inf
def two_dimensional_knapsack(weight: list[tuple[int, int]], value: list[int], W: tuple[int, int]) -> int:
    N, V = len(weight), sum(value)
    # dp[i][j] is the least w1 needed to achieve value i under the constraint of w0 <= j
    dp = [[inf] * (W[0] + 1) for _ in range(V + 1)]
    for i in range(W[0] + 1):
        dp[0][i] = 0

    for i in range(N):
        for v in range(V, value[i] - 1, -1):
            for w0 in range(W[0], weight[i][0] - 1, -1):
                dp[v][w0] = min(dp[v][w0], dp[v - value[i]][w0 - weight[i][0]] + weight[i][1])

    for i in range(N, -1, -1):
        if dp[i][-1] <= W[1]:
            return i
```

Time complexity: $O\left(N \times V \times W\left[0\right]\right)$ , where $N$ is the number of items and $V$ is the total value of $N$ items.

Space complexity: $O\left(V \times W\left[0\right]\right)$.