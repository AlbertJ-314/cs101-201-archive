## Disjoint Set

```python
class DisjointSet:
    def __init__(self, size: int) -> None:
        self.pa = list(range(size))
        self.rank = [1] * size
        # Optional: support union by size if needed
        # self.size = [1] * size
```

Space complexity: $O\left(n\right)$, where $n$ is the number of sets initially.



#### Union

```python
    def union(self, x: int, y: int) -> None:
        x, y = self.find(x), self.find(y)
        if x == y:
            return
        if self.rank[x] < self.rank[y]:
            x, y = y, x
        self.pa[y] = x

        # Union by rank
        if self.rank[x] == self.rank[y]:
            self.rank[x] += 1

        # Optional: union by size
        # self.size[x] += self.size[y]
```

Space complexity: $O\left(1\right)$.



#### Find

```python
    # Path compression
    # def find(self, x):
    #     if self.pa[x] != x:
    #         self.pa[x] = self.find(self.pa[x])
    #     return self.pa[x]

    # Path splitting
    def find(self, x: int) -> int:
        while x != self.pa[x]:
            # note the assignment order!
            self.pa[x], x = self.pa[self.pa[x]], self.pa[x]
        return x

    # Path halving
    # def find(self, x: int) -> int:
    #     while x != self.pa[x]:
    #         self.pa[x] = self.pa[self.pa[x]]
    #         x = self.pa[x]
    #     return x
```

Space complexity: $O\left(1\right)$ ($O\left(n\right)$ for path compression but iterative approach can be used to get $O\left(1\right)$).

Path splitting and path halving retain the same worst-case complexity as path compression but are more efficient in practice.



#### Time complexity

The amortized running time of each operation is $O\left(\alpha\left(n\right)\right)$. This is asymptotically optimal.

Here, the function $\alpha\left(n\right)$ is the [inverse Ackermann function](https://en.wikipedia.org/wiki/Ackermann_function#Inverse). It grows extraordinarily slowly, so this factor is $4$ or less for any $n$ that

can actually be written in the physical universe. This makes disjoint-set operations practically amortized constant time.

Time complexity of other implementations:

Path compression alone: $O\left(m\log n\right)$, where $m$ is the number of operations.

Union by rank (or by size) alone: $O\left(m\log n\right)$.
