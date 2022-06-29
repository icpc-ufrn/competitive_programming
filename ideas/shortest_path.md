# Shortest Path

#### Shortest Path is Dynamic Programming
[TODO]

#### Point w/ (maximum) minimum distance to a set of points
If we run a multisource BFS from such set, every visited position will hold the minimum distance to this set.  
Visit every point in the universe and take the one w/ highest distance.

#### BFS with priority nodes
Instead of visiting any reachable node for each iteration, 
the problem wants your BFS to visit first all reachable nodes with max priority,
than those with lower priority (nodes with high priority might appear again after relaxing one w/ lower priority) and so on.  
For this, use a priority queue instead of a queue.  
Thus, you will only decrease the priority of the next visited node if those (reachable) w/ high priority run out.

#### BFS on too much edges, delete node after visiting
You need to run a BFS on a graph with too much edges.  
What you can do is, instead of storing all edges, keep a structure that allows you to query the adjacent vertices `Y`s from `X`.  
Once you visit `X`, just add all `Y`s into the queue and delete them from such structure.
You can do this since they only need to be added once into the queue given that this will be their shortest distance.

Check: https://codeforces.com/contest/1662/problem/F and https://codeforces.com/contest/59/problem/E

#### Dijkstra with buckets `O(N*K+E)`, `K`: max edge weight (`0-K` BFS) (Dial's algorithm)
Suppose that our graph has edge weights at most `K` and that we are running a BFS from `src`.  
When visiting a node `v` with distance `d[v]` from `src`, all other nodes `x` in the queue will have `d[x] <= d[v] + K`, since `K` is the max distance.
Because of this, we only need to keep and "horizon" of the `K` next layers in such graph.  
While using a circular vector of queues of size `K+1`, we can a BFS in `O(N*K+E)`: for each node, we may need to find the next valid queue in `O(K)`.

This works like a Dijkstra but we use an array of queues for sorting and also we don't need to sort inside the same queue.

#### Real-weighted edges on BFS
Suppose that our graph has edge weights in a **real** range `[1;K)`, Dijkstra is too slow but `K` is small. 
First, for using BFS instead of Dijkstra, we need something like Dial's algorithm. Secondly, we need to know to which bucket assign vertex distance. Note that inside the same bucket, in order to keep Dial's assympotitcs, vertices can't be sorted i.e. their order must not matter.
  
We thus can put all vertices `x` with same `floor(dist[x])` together since every weight is at least `1`.
Also, we can then solve `[A;K*A)` if we index/cluster using `floor(d[x]/A)`.  

#### (Irreplaceable) Edges in shortest path from `s` to `t`
First, run a Dijkstra from `s` and other from `t`, which will compute, for each node `x`, its minimum distance to `s` (`d_s[x]`) and to `t` (`d_t[x]`).  
If an edge `(u,v)` with weight `w` is in the shortest path from `s` to `t`, then, it must be that `d_s[u] + w + d_t[v] = d_s[t] = d_t[s]`.  
Every edge `(u,v)` in the shortest path is responsible for an interval `[d_s[u], d_s[v]]` in the shortest path. If an edge is irreplaceable, it must be the only responsible for such interval i.e. doesn't intersect w/ other edge intervals.

#### `K`-th shortest path
Use priority queue instead of set.  
For finding the `k-th` shortest path to a node `sink` we actually need to find the `1...k`-th shortest paths for all nodes.  
Note that the `i-th` pop of the PQ in the vertice `x` is the `i-th` shortest path from the source to `x`. Thus, for getting the `k-th` shortest path for a node `x`, we need to allow `k` pops.

Note that we will restrict the number of pops, not the number of insertions in the priority queue.
Concerning optimization (as in vanilla Dijkstra):
- **Lazy delete:** We won't process the `i-th` pop if `i > k` since this won't change any approximation
- **Don't add useless approximations:** Add an approximation to the PQ only if it is better than the current candidates to top `K`
  - For this, solving "keep top `K` elements from a stream" problem will do. If it is the case of blocked paths, for each unique value, only one shortest path must be kept.

#### Keep the `K` shortests paths because of `K` blocked paths
Some problems need for you to keep for the same node the `K` shortests paths since some of these may be blocked and then you would need to ignore the blocked path and consider the following one.  
If the blockage happens because of a condition (source or time mod `P`), the shortest paths need to be unique concerning such condition i.e. there is no need in keeping `K` different shortest paths if all of them will be blocked. Thus, keep the shortest path for each source or time mod `P`.


Check: https://codeforces.com/gym/102006/problem/E  
Check: https://atcoder.jp/contests/abc245/tasks/abc245_g  
Check: https://www.hackerearth.com/challenges/competitive/september-clash-15/algorithm/dangerous-dungeon/description/  

