## Shortest Paths

#### Bellman–Ford

```python
from math import inf
def bellman_ford(n: int, edges: list[tuple[int, int, float]], source: int) -> list[float]:
    """
    Compute shortest distances from the source using the Bellman-Ford algorithm.

    Parameters:
        n (int): Number of vertices in the graph.
        edges (list[tuple[int, int, float]]): List of edges in the form (u, v, weight).
        source (int): The starting vertex.

    Returns:
        list[float]: The shortest distance from the source to each vertex.

    Raises:
        ValueError: If the graph contains a negative-weight cycle.
    """
    dis = [inf] * n
    dis[source] = 0

    for i in range(n):
        updated = False
        for u, v, w in edges:
            if dis[u] + w < dis[v]:
                dis[v] = dis[u] + w
                updated = True
        if not updated:
            break
        if i == n - 1:
            raise ValueError("Graph contains a negative-weight cycle.")

    return dis
```

Time complexity: $O\left(V \cdot E\right)$, where $V$ is the number of vertices and $E$ is the number of edges.

Space complexity: $O\left(V\right)$.

Applicable to directed weighted graphs with positive or negative weights and can detect negative cycles.



#### SPFA

```python
from collections import deque
from math import inf

def spfa(graph: list[list[tuple[int, float]]], source: int):
    """
    Compute shortest distances from the source using the SPFA algorithm.

    Parameters:
        graph (list[list[tuple[int, float]]]): Adjacency list where graph[u] contains (v, weight).
        source (int): The starting vertex.

    Returns:
        list[float]: The shortest distances from the source to each vertex.

    Raises:
        ValueError: If a negative-weight cycle is detected.
    """
    dis = [inf] * (n := len(graph))
    dis[source] = 0
    queue, vis, path_len = deque([source]), {source}, [0] * n

    while queue:
        u = queue.popleft()
        vis.remove(u)
        for v, w in graph[u]:
            if dis[u] + w < dis[v]:
                dis[v] = dis[u] + w
                path_len[v] = path_len[u] + 1
                if path_len[v] >= n:
                    raise ValueError("Graph contains a negative-weight cycle.")
                if v not in vis:
                    queue.append(v)
                    vis.add(v)

    return dis
```

Time complexity: $O\left(V \cdot E\right)$, where $V$ is the number of vertices and $E$ is the number of edges.

In practice, SPFA usually runs much faster — often around $O\left(E\right)$ for many sparse or randomly weighted graphs.

Space complexity: $O\left(V\right)$.

Applicable to directed weighted graphs with positive or negative weights and can detect negative cycles.



#### Dijkstra

```python
from math import inf

def dijkstra(graph: list[list[int]], source: int) -> list[float]:
    """
    Compute the shortest distances from the source using Dijkstra's algorithm (adjacency matrix version).

    Parameters:
        graph (list[list[float]]): Adjacency matrix where graph[u][v] is the weight from u to v,
                                   and `inf` if u and v are not directly connected.
        source (int): The starting vertex.

    Returns:
        list[float]: The shortest distances from the source to each vertex.
    """
    dis, vis = [inf] * (n := len(graph)), set()
    dis[source] = 0

    for _ in range(n):
        u = None
        for i in range(n):
            if i not in vis and (u is None or dis[i] < dis[u]):
                u = i
        vis.add(u)
        for v in range(n):
            dis[v] = min(dis[v], dis[u] + graph[u][v])

    return dis
```

Time complexity: $O\left(V^2\right)$, where $V$ is the number of vertices.

Space complexity: $O\left(V\right)$.

Applicable to positively-weighted directed graphs.



###### Priority queue

```python
from math import inf
import heapq

def dijkstra(graph: list[list[tuple[int, float]]], source: int) -> list[float]:
    """
    Compute the shortest distances from the source using Dijkstra's algorithm with a priority queue.

    Parameters:
        graph (list[list[tuple[int, float]]]): Adjacency list where graph[u] contains (v, weight).
        source (int): The starting vertex.

    Returns:
        list[float]: The shortest distances from the source to each vertex.
    """
    dis = [inf] * len(graph)
    dis[source] = 0
    queue, vis = [(0, source)], set()

    while queue:
        _, u = heapq.heappop(queue)
        if u in vis:
            continue
        vis.add(u)
        for v, w in graph[u]:
            if dis[u] + w < dis[v]:
                dis[v] = dis[u] + w
                heapq.heappush(queue, (dis[v], v))

    return dis
```

Time complexity: $O\left(E\log V\right)$, where $V$ is the number of vertices and $E$ is the number of edges.

Space complexity: $O\left(E\right)$.

Applicable to positively-weighted directed graphs.



#### Floyd–Warshall

```python
from copy import deepcopy

def floyd(graph: list[list[float]]) -> list[list[float]]:
    """
    Compute all-pairs shortest paths using the Floyd–Warshall algorithm.

    Parameters:
        graph (list[list[float]]): Adjacency matrix where graph[x][y] is the weight from x to y,
                                   and `inf` if x and y are not directly connected.

    Returns:
        list[list[float]]: Matrix `distance` where distance[x][y] is the shortest distance from x to y.
    """
    n, dis = len(graph), deepcopy(graph)
    for k in range(n):
        for x in range(n):
            for y in range(n):
                dis[x][y] = min(dis[x][y], dis[x][k] + dis[k][y])
    return dis
```

Time complexity: $O\left(V^3\right)$, where $V$ is the number of vertices.

Space complexity: $O\left(V^2\right)$.

Applicable to directed weighted graphs with positive or negative weights (but with no negative cycles reachable from the source).

Negative cycles can be detected post-computation by checking if `dis[i][i] < 0` for any `i`.
