## Pattern Matching

#### Brute force

```python
def match(s: str, p: str) -> list[int]:
    """Naive pattern matching: returns all indices where p matches s."""
    n, m, matches = len(s), len(p), []
    for i in range(n - m + 1):
        for j in range(m):
            if s[i + j] != p[j]:
                break
        else:
            matches.append(i)
    return matches
```

Time complexity: $O\left(nm\right)$, where $n, m$ is the length of $s, p$ respectively.

Space complexity: $O\left(1\right)$.



#### Rabin Karp

T.B.C.



#### KMP

```python
def failure(pattern: str) -> list[int]:
    lps, length = [0] * len(pattern), 0
    for i in range(1, len(pattern)):
        while length > 0 and pattern[i] != pattern[length]:
            length = lps[length - 1]
        if pattern[i] == pattern[length]:
            length += 1
            lps[i] = length
    return lps


def kmp(s: str, p: str) -> list[int]:
    n, m, lps = len(s), len(p), failure(p)
    matches, i, j = [], 0, 0
    for i in range(n):
        while j > 0 and s[i] != p[j]:
            j = lps[j - 1]
        if s[i] == p[j]:
            j += 1
            if j == m:
                matches.append(i - m + 1)
    return matches
```

Time complexity: $O\left(n + m\right)$, where $n, m$ is the length of $s, p$ respectively.

Space complexity: $O\left(m\right)$.



#### Boyer Moore

T.B.C.



#### Horspool

T.B.C.



#### Sunday

T.B.C.