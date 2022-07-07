# Graph ideas

### Query: walk 10^9 steps from a node
Binary lifting or cycle detection may be used.
Cycle detection is easier to code and update (if needed). Such is suitable if we are able to walk `~O(n)` (cost of detecting a cycle) for each query.

### Number of spanning trees of a graph in `O(N^3)`
Kirchhoff theorem states that the number of spanning trees of a graph (allows multiple edges) is equal to the determinant of any submatrix of the laplacian matrix of such graph.  
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

## Bipartite matching

### Merging nodes
Suppose a graph with sides A and B for bipartite matching and that the size of A is really small (`<15?`). Nodes from B may be merged if they connect to the same nodes from A. The number of condensed nodes will be at most `2^sz(A)`, what might be smaller than `B`. 

### Hall's theorem on contiguous intervals
We have a bipartite matching problem in which the `i`-th position from the left side reaches `[i-r;i+r]` on the right side. Even though Hall's theorem states that we need to check for every interval, we only need to worry about contiguous intervals: define `f(S)` as reachable positions from `S=a,b,...,z \in left`. Suppose that Hall's theorem fails for a non-contiguous `S` i.e. `|S| < |f(S)|`:
- If `f(S)` is contiguous, the contiguous interval `[l(S);r(S)]` will also fail
- If `f(S)` is non-contiguous, it must be that one of the contiguous segments of `S` fails too, since these are independent queries.   

Thus, if a non-contiguous `S` fails, it is guaranteed that a contiguous interval will fail, so only these need to be checked.   
Check: https://szkopul.edu.pl/problemset/problem/EwpbJWZPly_zZ5i4ytg_8fDE/site/?key=statement

# Graphs properties

### Functional graphs
Each node has outdegree 1. A connected component contains exactly one cycle (and possibly some chains of nodes leading into the cycle).

### Degrees directed graphs
- If every vertex has outdegree >= 1, the graph has a cycle
- If, for every vertex, indegree = outdegree, the graph is a union of cycles and it has an eulerian circuit
