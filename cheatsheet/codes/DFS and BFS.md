## DFS

#### Recursive

```python
from typing import List
# graph: List[List[int]] adjacency list
def dfs(graph: List[List[int]], u: int, vis: set) -> None:
    vis.add(u)
    for v in graph[u]:
        if v not in vis:
            dfs(graph, v, vis)
```



#### Stack

```python
from typing import List
# graph: List[List[int]] adjacency list
def dfs(graph: List[List[int]], start: int) -> None:
    stack, vis = [start], {start}
    while stack:
        u = stack.pop()
        for v in graph[u]:
            if v not in vis:
                vis.add(v)
                stack.append(v)
```



## BFS

#### Deque

```python
from collections import deque
from typing import List
# graph: List[List[int]] adjacency list
def bfs(graph: List[List[int]], start: int) -> None:
    vis, queue = {start}, deque([start])
    while queue:
        u = queue.popleft()
        for v in graph[u]:
            if v not in vis:
                vis.add(v)
                queue.append(v)
```

