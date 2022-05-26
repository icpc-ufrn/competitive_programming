# Ideas
Summary of some ~not so~ frequent ideas

## General

### RMQ query on slinding window
Min (or max) heap with lazy delete; keep adding while you slide through

### Manhattan to Chebyshev distante
Chebyshev distance: `D(X,Y)=max(|X_i-Y_i|)`  
The Manhattan distance between points `X` and `Y` is "equivalent" to the Chebyshev distance between `X'` and `Y'`, where `P'` is the expansion of the abs for a point (all `2^(d-1)` combinations of signals. One coordinate can be fixed since we are taking the abs). Ex:
- `(x,y)` => `(x+y,x-y)`
- `(x,y,z)` => `(x+y+z,x+y-z,x-y+z,x-y-z)`  
Check: https://www.spoj.com/problems/DISTANCE/

### Quadratic functions in linear models
**TODO** (https://codeforces.com/gym/102644/problem/G)
In linear contexts (linear recurrences, min cost max flow), the quadratic function `i^2` can be modelled as the sum of the `i` first odd numbers.

### Minimum distance from a set of points/nodes
Given a set of points, compute for other points in your field the min. distance for such set. Run a multisource BFS ~or Dijkstra~ and, on each iteration, try to visit all 1-distance apart points from the current point. Note that any point is only visited once. The maximum minimum distance can be found if your field is small.

### BFS with priority nodes
Instead of visiting any reachable node for each iteration, the problem wants your BFS to visit first all reachable nodes with max priority, than those with lower priority (nodes with high priority might appear again after relaxing one w/ lower priority) and so on. For this, use a priority queue instead of a vanilla queue. Thus, you will only decrease the priority of the next visited node if those (reachable) w/ high priority run out.

### BFS on too much edges, delete node after visiting
You need to run a BFS on a graph with too much edges. What you can do is, instead of keeping all edges, keep a structure that allows you to query the adjacent vertices `Y`s from `X`. Once you visit `X`, just add all `Y`s into the queue and delete them from such structure. You can do this since they only need to be added once into the queue given that this will be their shortest distance.

Check: https://codeforces.com/contest/1662/problem/F and https://codeforces.com/contest/59/problem/E

### `min(x, n - x)`
You are given a list of elements that have a linear order and that compose a recursive structure (eg. list of tree elements in inorder).
You want to divide and conquer these elements. However, splitting them into the recursive calls isn't trivial.
We can apply `min(x,n-x)` if there is a search algorithm that traverses these elements following the given order for finding a delimiter and thus splitting.
Instead of searching in only one direction with 1 pointer in `O(x), x <= n`, we will use 2 pointers w/ opposite direction, one starting at the beggining and the other at the end (we will traverse these elements following the given order and its reverse). 
Now, the cost of this search is `O(min(x,n-x))`.

Note that:
- We will split the elements of the current call (`all`) into 2 sets: `small` and `large`. 
  - `small`: elements visited in the direction in which the delimiter was found (`all` >= 2*`small`)
  - `large`: the remaining. 
  - `all` will be splitted into these two disjoint sets for the child calls. 
- For each call, we don't traverse more than `O(|small|)` elements.
- An element is at most at `O(logN)` `small` sets: from bottom to top, these sets have their size at least doubled (union-by-size); their sizes don't exceed `N`.
- For each `small` element in a call, we execute a `O(C)` operation (eg. adding to a set, sorting).
- The final cost is bounded by the sum of `small` sets along the divide and conquer
Thus, the final cost is:
```
N: number of elements
log(N): max. number of small sets an element is
C: cost of operation per element inside a small set
= N*log(N)*C
```

In some problems, both original and reverse order can be expressed inside the same list that will be traversed in forward in backward directions.
However, in some less trivials problems, one list for each direction (original and reverse order) need to be build.
For such cases, we will still traverse in both direciton simultaneously, but we need to know which elements will be passed for the child calls, since a simple suffix/preffix split won't work.
What can be done is:
1. Create new lists for the `small` elements, sort if necessary (`C=log(N)`).
2. Solve for the `small` set
3. Delete the `small` set elements from `all`. Can be done with lazy deleting.
4. Solve `large`. Use `all` as `large`, orders are preserved after deleting in (3).
 
## Segtree

### Area of the union of rectangles
We want the area of the union of rectangles.
We can do this with line sweep + segtree, traversing the X axis (left to right). 
Note that we can decompose every y-aligned edge into several intervals. Our segtree leaves are such intervals.
While traversing the X axis, we want to count how many times each interval occured and use these interval sizes in our area calculation.
Even if a interval is counted multiple times, it only contributes once to our area. 
Every time we process an event in out line sweep (addition or removal of a y-aligned edge i.e. contiguous interval in our segtree), we can query the whole segtree and increment the answer.

### Best interval after some updates for easily mergeble intervals
A node `X` with interval `[l;r]` can keep 3 infos: the best interval, preffix and suffix totally inside it.
When merging `X` to `Y` (`X` left and `Y` right) into `Z`:  
- `Z.best = max(X.best, Y.best, f(X.suffix + Y.prefix))`
- `Z.prefix = X.prefix` or `X.prefix + Y.prefix` when `X.prefix = [lx;rx]` 
- `Z.suffix = Y.sufix` or `X.sufix + Y.sufix` when `Y.sufix = [ly;ry]`  
Check: https://codeforces.com/edu/course/2/lesson/5/3/practice/contest/280799/problem/A 

### (Automaton) String editting and pattern matching
Given a pattern `P` and a modifiable string `S`, count how many patterns are inside `S` or modify `S`. This can be done using KMP automaton and segtree. For each `[l;r]` node for `S`, keep `to[x]` (to which automaton node the substr `S[l;r]` leads if you start traversing it from `x`) and `accs[x]` (how many times we visit the accepted node from the automaton if we start traversing `S[l;r]` from `x`).

Merge two nodes using function composition: `(a+b).to[x] = b.to[a.to[x]]` and `(a+b).accs[x] = b.accs[a.to[x]] + a.accs[x]`  
Check: https://codeforces.com/gym/101908/problem/H

### Non-commutative operations on arrays and trees (HLD)
When dealing w/ non-commutative operations (eg. reading a string), a forward query `[l;r]` might differ from a backward `[r;l]` query.  
For handling queries in both ways (forward and backward), your node must keep 2 internal states, one for each way.  
When combining nodes, stablish that `(a+b).forward = merge(a.forward, b.forward)` and `(a+b).backward = merge(b.backward, a.backward)`.  
When querying, specify if it is a forward or a backward query. If its a forward query, on a `[l;r]` node, merge `[l;mid]`'s answer to `[mid;r]`'s answer and use only forward values. Otherwise, use only backward values and merge `[mid;r]`'s answer to `[l;mid]`'s answer.

On trees, going down and up are different directions, so this technique needs also to be used. Query and update orders need also to be respected at the HLD structure. 
Check: https://codeforces.com/gym/101908/problem/H

### (Automaton) Cost (chars to erase) in order to get to the accepted state  
We want to know the minimal (cost) chars to erase in order to accept some subseqs and avoid some others. 
An automata w/ costs on edges can be modelled and for each state, there will be 3 types of edges:
- Edges for advancing: reading its char means we won't delete it and we will advance to the next automata state
- Edges for staying: reading its char means we will delete it and we will force the automata to stay in this state
- Useless edges: reading its char doesn't interfer in the current state  
Note that the edges for advancing and staying have the same charset. Edges for staying have cost 1 (since we need to delete 1 char) while other edges have 0 cost.

A segtree node `(l;r)` will keep the cost of minimal path between the automata states. Thus, we only need to check the cost between the start and accepted state. Nodes can be combined using the Floyd-Warshall algorithm. `(l;l)` nodes deal with the `s[l]` char; edges that don't have `s[l]` in their charset are setted to infinite cost.

Check: https://codeforces.com/problemset/problem/750/E

## Structures

### Inserting after building
If you are dealing with structures without `inserts` after querying, you can keep a set of structures with sizes of power of 2, keeping only one structure with size `2^x` at a time. Always try to insert at the `2^0`-sized structure and solve `2^x` duplicates by merging and creating a `2^(x+1)`-sized structure. Query on all `O(log(elements))` structures.
https://atcoder.jp/contests/abc244/tasks/abc244_h

### Implement remove using stacks and merge
If you are doing two pointers keeping a structure without `remove` operation but with (a cheap) `merge`, you can use a "queue" for removing (actually two stacks). Two stacks are needed since we don't want state `i+1` to keep info from `i` after its removal. By using two stacks, we keep one stack only for deleting where state `i` is built from `i+1`.

Check: https://www.codechef.com/problems/MIXFLVOR

### Query which intervals contain a number
We want to keep intervals in a structure and query which intervals contain an integer `x`.  
Create a sort of Segment Tree in which each node of this scructure keeps a set of intervals.  
Update: Insert interval `(a;b)` into the `O(log)` nodes `[l;r]` maximals inside `(a;b)`.  
Query: The intervals from all nodes from root to the leaf (`x`) contain `x`. A lazy delete is needed since one interval can be in multiple nodes.  

Check: https://codeforces.com/contest/786/problem/B

## Randomized

### Blogewoosh 6
Given `n` numbers in a random order, the expected times that the maximum seen value changes is `O(log(n))` while processing such numbers.

## Greedy

### Argument exchange
When trying to find the optimal order for a greedy, analyze the relation between only 2 elements. Define a cost function `f` capable of computing over both orders `AB` and `BA`. Check wich conditions are held for `f(AB) < f(BA)` (assuming `A` goes first).

## Dynamic Programming

### Knapsack - biggest subset with bounded cost 
DP where you maintain `A[x]: minimum cost of using x elements` and iterate through elements, minimizing `A[x]` when possible

### Game: Random vs. Greedy strategy / Black vs. white balls
Two players take elements from an array; one follows a greedy strategy and other a random. Use dynamic programming for computing `dp[i][j]: probability of j-th element be taken by first player given that i elements are laid`

Check: https://codeforces.com/problemset/gymProblem/102916/D

## Bipartite matching

### Merging nodes
Suppose a graph with sides A and B for bipartite matching and that the size of A is really small (`<15?`). Nodes from B may be merged if they connect to the same nodes from A. The number of condensed nodes will be at most `2^sz(A)`, what might be smaller than `B`. 

### Hall's theorem on contiguous intervals
We have a bipartite matching problem in which the `i`-th position from the left side reaches `[i-r;i+r]` on the right side. Even though Hall's theorem states that we need to check for every interval, we only need to worry about contiguous intervals: define `f(S)` as reachable positions from `S=a,b,...,z \in left`. Suppose that Hall's theorem fails for a non-contiguous `S` i.e. `|S| < |f(S)|`:
- If `f(S)` is contiguous, the contiguous interval `[l(S);r(S)]` will also fail
- If `f(S)` is non-contiguous, it must be that one of the contiguous segments of `S` fails too, since these are independent queries.   

Thus, if a non-contiguous `S` fails, it is guaranteed that a contiguous interval will fail, so only these need to be checked.   
Check: https://szkopul.edu.pl/problemset/problem/EwpbJWZPly_zZ5i4ytg_8fDE/site/?key=statement

## Game theory

### Last to play wins/unwanted positions
If the problem presents this variation, set the "about to win" positions as unwanted positions (really high grundy). Thus, states leading only to such positions will get a losing status.


