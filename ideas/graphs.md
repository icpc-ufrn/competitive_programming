# Graph ideas

### Query: walk 10^9 steps from a node
Binary lifting or cycle detection may be used.
Cycle detection is easier to code and update (if needed). It's suitable if we are able to walk `~O(n)` (cost of detecting a cycle) for each query.

# Graphs properties

### Functional graphs
Each node has outdegree 1. A connected component contains exactly one cycle (and possibly some chains of nodes leading into the cycle).

### Degrees directed graphs
- If every vertex has outdegree >= 1, the graph has a cycle
- If, for every vertex, indegree = outdegree, the graph is a union of cycles and it has an eulerian circuit
