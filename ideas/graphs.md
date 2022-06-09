# Graph ideas

### Query: walk 10^9 steps from a node
Binary lifting or cycle detection may be used.
Cycle detection is easier to code and update (if needed). Such is suitable if we are able to walk `~O(n)` (cost of detecting a cycle) for each query.

### Number of spanning trees of a graph in `O(N^3)`
Kirchhoff theorem states that the number of spanning trees of a graph is equal to the determinant of any submatrix of the laplacian matrix of such graph.  
Laplacian matrix, `Lij`:
- `deg(v)` if `i=j`
- edges between `i` and `j` `* -1`

### Partitioning set of nodes `S` for DP: `g(S) = sum_T[f(T)*g(S-T)]`
For some DP modelling, when computing `f(S)`, `S` set of vertices, we might need to partition `S` into exclusive cases in order to avoid processing the same thing twice (eg. when using non-idempotent ops). 
Such partitioning can be done by choosing an arbitrary vertex `v \in S` and 

Examples:
- Number forests `g(S)` over a set of vertices `S` using auxiliary `f(S)` couting number of trees on `S`.
Note that we can partition the forests on `S` by the trees that contain `v`.
Iterate through all `T` set of nodes spanning a tree, `g(S) = sum_T[f(T)*g(S-T)]`  

- Number connected graphs `g(S)` over a set of vertices `S` using auxiliary `f(S)` couting number of graphs on `S`.
Note that we can partition the graphs on `S` by the connected components that contain `v`.
Iterate through all `T` set of nodes forming a connected graph, `g(S) = sum_T[f(T)*g(S-T)]`

# Graphs properties

### Functional graphs
Each node has outdegree 1. A connected component contains exactly one cycle (and possibly some chains of nodes leading into the cycle).

### Degrees directed graphs
- If every vertex has outdegree >= 1, the graph has a cycle
- If, for every vertex, indegree = outdegree, the graph is a union of cycles and it has an eulerian circuit
