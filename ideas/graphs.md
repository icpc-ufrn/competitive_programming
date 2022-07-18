# Graph ideas

### Query: walk 10^9 steps from a node
Binary lifting or cycle detection may be used. This is probably a graph in which every outdegree is `1`.

Binary lifting: Precompute `bf[x][i]`: where will I stop if start from `x` and walk `2^i` edges. 
Cycle detection: find a cycle of length `L` in `O(n)`, use this cycle `K/L` times and walk `L` naively. Since `L` is `O(n)`, the total cost is `O(n)`.

If there are multiple queries, binary lifting might be better. If there are updates, binary lifting might have a drawback since it's idea is to be a precomputed structure.

### Number of spanning trees of a graph in `O(N^3)`
Kirchhoff theorem states that the number of spanning trees of a graph (allows multiple edges) is equal to the determinant of any submatrix of the laplacian matrix of such graph.  
Laplacian matrix, `Lij`:
- if `i=j`: `deg(v)` 
- else: edges between `i` and `j` `* -1`

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
We have a bipartite (`left` and `right` sets) matching problem in which an element `x` from `left` side have edges to `[lx_0;rx_0]`, `[lx_1;rx_1]`, ... (that is, it has edges to a set of intervals) in `right`.  
  
We can use Hall's theorem in order to check if there is a matching that **saturates all elements in `right` that have outgoing edges**, using some of these same edges.  
Even though Hall's theorem states that we need to check for every subset, we only need to worry about contiguous intervals: define `f(S)` as reachable positions from `S=a,b,...,z`, `S` is a subset from `left`. Suppose that Hall's theorem fails for a non-contiguous `S` i.e. `|S| > |f(S)|`:
- If `f(S)` is contiguous, the contiguous interval `[l(S);r(S)]` will also fail
- If `f(S)` is non-contiguous, it must be that one of the contiguous segments of `S` fails too, since these are independent queries.   

Thus, if a non-contiguous `S` fails, it is guaranteed that a contiguous interval will fail, so only these need to be checked.   
Check: https://szkopul.edu.pl/problemset/problem/EwpbJWZPly_zZ5i4ytg_8fDE/site/?key=statement  
Check: https://atcoder.jp/contests/arc144/submissions/33347474

## 2SAT
Build an implication graph: nodes are propositions (`x`) and their negations (`~x`) and there is a **direct** edge `(p,q)` if `p => q`. Also, if `(p,q)` exists, so does `(~q,~p)` (contraposition). 
  
A path `x...y` in this graph indicates that, if `x` is set to true, `y` also needs also to be true. A valoration will satisfy the clauses iff there won't be `x=true` and `y=false` and there exists a path `x...y`.  
  
**First, we need to check whether `x` and `~x` are in the same SCC**. 
If that is the case, there is a path from `x` to `~x` and from `~x` to `x`, it means that we can't satisfy the clauses:
- if we set `x` to true, since `x => ~x`, `~x` would also be true (absurd)
- if we set `x` to false, `~x` would need to be true, and, since `~x => x`, `x` would also be true (absurd)

Then, build the condensation graph. All variables in the same SCC have the same truth value. 

TODO
  


# Graphs properties

### Functional graphs
Each node has outdegree 1. A connected component contains exactly one cycle (and possibly some chains of nodes leading into the cycle).

### Degrees directed graphs
- If every vertex has outdegree >= 1, the graph has a cycle
- If, for every vertex, indegree = outdegree, the graph is a union of cycles and it has an eulerian circuit

### Condensation graph
Graph resulted after collapsing SCCs into a single node. This graph is acyclic.

