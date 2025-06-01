## Dijkstra

#### Brute force

```python
from math import inf
from typing import List
# graph: List[List[int]] adjacency matrix
def dijkstra(graph: List[List[int]], start: int) -> List[int]:
    n, vis = len(graph), set()
    dis = [inf for _ in range(n)]
    dis[start] = 0
    for _ in range(n):
        ind = -1
        for i in range(n):
            if i not in vis and dis[i] < dis[ind]:
                ind = i
        vis.add(ind)
        for i in range(n):
            dis[i] = min(dis[i], dis[ind] + graph[ind][i])
    return dis
```

Time complexity: $O\left(V^2\right)$, where $V$ is the number of vertices.

Space complexity: $O\left(V\right)$.



#### Priority queue

```python
from math import inf
import heapq
from typing import List, Tuple
# graph: List[List[Tuple[int, int]]] adjacency list
def dijkstra(graph: List[List[Tuple[int, int]]], start: int) -> List[int]:
    dis = [inf for _ in range(len(graph))]
    dis[start] = 0
    queue, vis = [(0, start)], set()
    while queue:
        _, u = heapq.heappop(queue)
        if u in vis:
            continue
        vis.add(u)
        for v, w in graph[u]:
            if dis[v] > dis[u] + w:
                dis[v] = dis[u] + w
                heapq.heappush(queue, (dis[v], v))
    return dis
```

Time complexity: $O\left(E\log E\right)$, where $E$ is the number of edges.

Space complexity: $O\left(E\right)$.