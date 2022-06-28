# Shortest Path

#### Point w/ maximum minimum distance to a set of points
If we run a multisource BFS from such set, every visited position will hold the minimum distance to this set.  
Visit every point in the universe and take the one w/ highest distance.

#### BFS with priority nodes
Instead of visiting any reachable node for each iteration, 
the problem wants your BFS to visit first all reachable nodes with max priority,
than those with lower priority (nodes with high priority might appear again after relaxing one w/ lower priority) and so on.  
For this, use a priority queue instead of a queue.  
Thus, you will only decrease the priority of the next visited node if those (reachable) w/ high priority run out.

### BFS on too much edges, delete node after visiting
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

#### Real-weighted edges on BFS
Suppose that our graph has edge weights in a **real** range `[1;K)`, Dijkstra is too slow but `K` is small. 
First, for using BFS instead of Dijkstra, we need something like Dial's algorithm.  
  
Secondly, we need to know to which bucket assign vertex distance. 
Since every weight is at least `1`, we can put all vertices `x` with same `floor(dist[x])` together in the same queue.
We can then solve `[A;K*A)` if we index/cluster using `floor(d[x]/A)`.  

This works like a Dijkstra but we use an array of queues for sorting and also we don't need to sort inside the same distance cluster/queue.
Some tricks can be used for avoiding precision errors since we are dealing with real numbers.

#### (Irreplaceable) Edges in shortest path from `s` to `t`
First, run a Dijkstra from `s` and other from `t`, which will compute, for each node `x`, its minimum distance to `s` (`d_s[x]`) and to `t` (`d_t[x]`).  
If an edge `(u,v)` with weight `w` is in the shortest path from `s` to `t`, then, it must be that `d_s[u] + w + d_t[v] = d_s[t] = d_t[s]`.  
Every edge `(u,v)` in the shortest path is responsible for an interval `[d_s[u], d_s[v]]` in the shortest path. If an edge is irreplaceable, it must be the only responsible for such interval i.e. doesn't intersect w/ other edge intervals.
