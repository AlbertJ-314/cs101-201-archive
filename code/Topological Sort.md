## Topological Sort

#### [Kahn's algorithm](https://en.wikipedia.org/wiki/Topological_sorting#Kahn's_algorithm)

```python
from collections import deque

def toposort(graph: list[list[int]]) -> list[int] | None:
    """
    Perform topological sorting on a directed graph using Kahn's algorithm.

    Parameters:
        graph (list[list[int]]): Adjacency list representing the directed graph.

    Returns:
        list[int]: A list representing the topological order of the graph nodes if the graph is a DAG.
        None: If the graph contains at least one cycle.
    """
    indegrees = [0] * (n := len(graph))
    for u in range(n):
        for v in graph[u]:
            indegrees[v] += 1

    # sources: nodes with no incoming edge
    sources, topo_order = deque([x for x in range(n) if indegrees[x] == 0]), []
    while sources:
        u = sources.popleft()
        topo_order.append(u)
        for v in graph[u]:
            indegrees[v] -= 1
            if indegrees[v] == 0:
                sources.append(v)

    # Return None if graph has at least one cycle
    return topo_order if len(topo_order) == n else None
```

Time complexity: $O\left(V + E\right)$, where $V, E$ denotes the number of vertices and edges respectively.

Space complexity: $O\left(V\right)$.



#### [DFS](https://en.wikipedia.org/wiki/Topological_sorting#Depth-first_search)

```python
def toposort(graph: list[list[int]]) -> list[int] | None:
    """
    Perform topological sorting on a directed graph using DFS-based approach.

    Parameters:
        graph (list[list[int]]): Adjacency list representing the directed graph.

    Returns:
        list[int]: A list representing the topological order of the graph nodes if the graph is a DAG.
        None: If the graph contains at least one cycle.
    """
    vis, curr, topo_order, cycle = set(), set(), [], False

    def dfs(node: int) -> None:
        nonlocal cycle
        if node in curr:
            cycle = True
        if cycle or node in vis:
            return
        vis.add(node)
        curr.add(node)
        for neighbor in graph[node]:
            dfs(neighbor)
        curr.remove(node)
        topo_order.append(node)

    for u in range(len(graph)):
        if u not in vis:
            dfs(u)

    # Return None if graph has at least one cycle
    return None if cycle else topo_order[::-1]
```

Time complexity: $O\left(V + E\right)$, where $V, E$ denotes the number of vertices and edges respectively.

Space complexity: $O\left(V\right)$.



#### Cycle detection

Topological sorting algorithms can be used to detect cycles.

In **undirected graphs**, be careful with 2-cycles! Use DFS with parent tracking or Disjoint Set.
