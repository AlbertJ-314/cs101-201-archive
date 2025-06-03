## Minimum Spanning Tree

#### [Kruskal's algorithm](https://en.wikipedia.org/wiki/Kruskal%27s_algorithm)

```python
def kruskal(n: int, edges: list[tuple[int, int, float]]) -> list[tuple[int, int, float]]:
    """
    Compute the Minimum Spanning Tree (MST) using Kruskal's algorithm.

    Parameters:
        n (int): Number of vertices.
        edges (list[tuple[int, int, float]]): List of edges in the form (u, v, weight).

    Returns:
        list[tuple[int, int, float]]: The edges included in the MST.
    """
    vertices, mst = DisjointSet(n), []
    for u, v, w in sorted(edges, key=lambda edge: edge[2]):
        if vertices.find(u) != vertices.find(v):
            mst.append((u, v, w))
            vertices.union(u, v)
            if len(mst) == n - 1:
                break
    return mst
```

Time complexity: $O\left(E \log V\right)$, where $V, E$ denotes the number of vertices and edges respectively.

Space complexity: $O\left(V + E\right)$.



#### [Prim's algorithm](https://en.wikipedia.org/wiki/Prim%27s_algorithm)

###### adjacency list

```python
import heapq

def prim(graph: list[list[tuple[int, float]]]) -> list[tuple[int, int, float]]:
    """
    Compute the Minimum Spanning Tree (MST) using Prim's algorithm.

    Parameters:
        graph (list[list[tuple[int, float]]]): Adjacency list representing the graph.
            Each graph[u] contains tuples (v, weight) indicating an edge from u to v.

    Returns:
        list[tuple[int, int, float]]: The edges included in the MST.
    """
    vis = [True] + [False] * ((n := len(graph)) - 1)  # Start with vertex 0
    queue, mst = [], []
    for v, w in graph[0]:
        heapq.heappush(queue, (w, 0, v))

    while queue and len(mst) < n - 1:
        w, u, v = heapq.heappop(queue)
        if vis[v]:
            continue
        vis[v] = True
        mst.append((u, v, w))
        for neighbor, weight in graph[v]:
            if not vis[neighbor]:
                heapq.heappush(queue, (weight, v, neighbor))

    # if sum(vis) != n, the graph is not connected
    return mst
```

Time complexity: $O\left(E \log V\right)$, where $V, E$ denotes the number of vertices and edges respectively.

Space complexity: $O\left(V + E\right)$.



###### adjacency matrix

```python
from math import inf

def prim(graph: list[list[float]]) -> list[tuple[int, int, float]]:
    """
    Compute the Minimum Spanning Tree (MST) using Prim's algorithm (adjacency matrix version).

    Parameters:
        graph (list[list[float]]): An adjacency matrix where graph[u][v] is the weight
            of the edge (u, v), or `inf` if no edge exists.

    Returns:
        list[tuple[int, int, float]]: The edges in the MST as (parent, node, weight).
    """
    n = len(graph)
    key, parent, in_mst = [0] + [inf] * (n - 1), [None] * n, [False] * n

    for _ in range(n):
        u = min((v for v in range(n) if not in_mst[v]), default=-1, key=lambda v: key[v])
        if u == -1:
            break   # Disconnected graph
        in_mst[u] = True
        for v in range(n):
            if not in_mst[v] and graph[u][v] < key[v]:
                key[v], parent[v] = graph[u][v], u
    
    return [(parent[i], i, graph[i][parent[i]]) for i in range(1, n) if parent[i] is not None]
```

Time complexity: $O\left(V^2\right)$, where $V$ denotes the number of vertices.

Space complexity: $O\left(V\right)$.
