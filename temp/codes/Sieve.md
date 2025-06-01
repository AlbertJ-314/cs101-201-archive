## Sieve

#### [Sieve of Eratosthenes](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes#Overview)

```python
from typing import List
def eratosthenes_sieve(n: int) -> List[int]:
		primes, is_prime = [], [True for _ in range(n)]
    for i in range(2, n):
        if is_prime[i]:
            primes.append(i)
            # Optimization: start enumerating from i * i instead of 2 * i
            for k in range(i * i, n, i):
                is_prime[k] = False
    return primes
```

Time complexity: $O\left(n\log\log n\right)$, where $n$ is the exclusive upper limit for finding primes.

Space complexity: $O\left(n\right)$.



#### [Euler's sieve](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes#Euler's_sieve)

```python
from typing import List
def euler_sieve(n: int) -> List[int]:
    primes, is_prime = [], [True for _ in range(n)]
    for i in range(2, n):
        if is_prime[i]:
            primes.append(i)
        for p in primes:
            if p * i >= n:
                break
            is_prime[p * i] = False
            if i % p == 0:
                break
    return primes
```

Time complexity: $O\left(n\right)$, where $n$ is the exclusive upper limit for finding primes.

Space complexity: $O\left(n\right)$.
