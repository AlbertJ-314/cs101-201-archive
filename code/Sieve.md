## Sieve

#### [Sieve of Eratosthenes](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes#Overview)

```python
def eratosthenes_sieve(n: int) -> list[int]:
    """
    Generate all prime numbers less than n using the Sieve of Eratosthenes.

    Parameters:
        n (int): Upper limit (non-inclusive) to generate primes.

    Returns:
        list[int]: A list of all prime numbers less than n.
    """
    primes, is_prime = [], [True for _ in range(n)]
    for i in range(2, n):
        if is_prime[i]:
            primes.append(i)
            # Optimization: start from i * i, since smaller multiples are already marked
            for k in range(i * i, n, i):
                is_prime[k] = False
    return primes
```

Time complexity: $O\left(n\log\log n\right)$, where $n$ is the exclusive upper limit for finding primes.

Space complexity: $O\left(n\right)$.



#### [Euler's sieve](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes#Euler's_sieve)

```python
def euler_sieve(n: int) -> list[int]:
    """
    Generate all prime numbers less than n using the Euler sieve algorithm.

    Parameters:
        n (int): The upper bound (exclusive) for generating primes.

    Returns:
        list[int]: A list of prime numbers less than n.
    """
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
