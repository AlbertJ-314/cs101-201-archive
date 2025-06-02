## [Josephus problem](https://en.wikipedia.org/wiki/Josephus_problem)

```python
def josephus(n: int, k: int) -> int:
    """
    Parameters:
        n (int): Number of people in the circle, indexed from 0.
        k (int): Every k-th person will be eliminated.

    Returns:
        int: The index of the last remaining person.
    """
    res = 0
    for i in range(1, n + 1):
        res = (res + k) % i
    return res
```

Time complexity: $O\left(n\right)$ , where $n$ is the number of people in the circle.

Space complexity: $O\left(1\right)$.



#### Alternative

```python
def josephus(n: int, k: int) -> int:
    """
    Parameters:
        n (int): Number of people in the circle, indexed from 0.
        k (int): Every k-th person is eliminated.

    Returns:
        int: The index of the last remaining person.
    """
    if n == 1:
        return 0
    if k == 1:
        return n - 1
    if k > n:
        return (josephus(n - 1, k) + k) % n
    res = josephus(n - n // k, k) - n % k
    if res < 0:
        res += n
    else:
        res += res // (k - 1)
    return res
```

Time complexity: $O\left(k\log n\right)$ , where $k$ denotes the count for each step.

Space complexity: $O\left(n\right)$ at worst.



#### Special case when $k = 2$

The answer is $2(n - 2^{\lfloor\log_2n\rfloor})$.





## Expressions parsing

[Shunting yard algorithm](https://en.wikipedia.org/wiki/Shunting_yard_algorithm)

#### Infix expression to postfix expression

```python
def infix_to_postfix(tokens: list[str]) -> list[str]:
    """Convert infix expression to postfix (Reverse Polish) notation.

    Supported:
    - Operators: +, -, *, /
    - Parentheses: ( )
    - Operands: integers, floats, variables (multi-character allowed)

    Parameters:
        tokens (list[str]): A list of tokens in infix order.

    Returns:
        list[str]: The corresponding postfix expression.
    """
    precedence = {'+': 1, '-': 1, '*': 2, '/': 2}
    stack, output = [], []

    for token in tokens:
        if token == "(":
            stack.append(token)
        elif token == ")":
            while stack and stack[-1] != "(":
                output.append(stack.pop())
            stack.pop()
        elif token in precedence:
            while stack and stack[-1] != "(" and precedence[stack[-1]] >= precedence[token]:
                output.append(stack.pop())
            stack.append(token)
        else:
            output.append(token)

    while stack:
        output.append(stack.pop())
    return output
```

The postfix expression can be used to produce an [abstract syntax tree](https://en.wikipedia.org/wiki/Abstract_syntax_tree).





## [Knight's tour](https://en.wikipedia.org/wiki/Knight%27s_tour)

#### [**Warnsdorf's rule**](https://en.wikipedia.org/wiki/Knight%27s_tour#Warnsdorf's_rule)

```python
directions = [(-2, -1), (-2, 1), (-1, -2), (-1, 2), (1, -2), (1, 2), (2, -1), (2, 1)]
def knight_tour(n: int, sx: int, sy: int) -> bool:
    vis = [[False for _ in range(n)] for _ in range(n)]
    vis[sx][sy] = True

    def degree(x: int, y: int) -> int:
        cnt = 0
        for dx, dy in directions:
            nx, ny = x + dx, y + dy
            if 0 <= nx < n and 0 <= ny < n and not vis[nx][ny]:
                cnt += 1
        return cnt

    def Warnsdorf(x: int, y: int, steps: int) -> bool:
        if steps == n * n:
            return True
        moves = []
        for dx, dy in directions:
            nx, ny = x + dx, y + dy
            if 0 <= nx < n and 0 <= ny < n and not vis[nx][ny]:
                moves.append((degree(nx, ny), nx, ny))
        # proceeds to the square from which the knight will have the fewest onward moves
        for _, nx, ny in sorted(moves):
            vis[nx][ny] = True
            if Warnsdorf(nx, ny, steps + 1):
                return True
            vis[nx][ny] = False
        return False

    return Warnsdorf(sx, sy, 1)
```

Quote from wikipedia: On many graphs that occur in practice this heuristic is able to successfully locate a solution in linear time.
