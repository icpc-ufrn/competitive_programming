# Segtree

### Histogram on a weighted array
Let's say we have an array where `a_i` is a weight and we have operations like `add(lu;ru)`: add weights from `a_i`, (`lu <= i <= ru`) to out Segtree.  
What we can do is set for each Segtree node `x` with interval `[l;r]` a variable `val[x]`: sum of `a_i` (`l <= i <= r`) and use this `val[x]` in our operations:  
`add(lu;ru)` will processed as add `val[x]` to `seg[x]` if `x` is inside `[lu;ru]`.

### Area of the union of rectangles
We want the area of the union of rectangles.
We can do this with line sweep + segtree, traversing the X axis (left to right). 
Note that we can decompose every y-aligned edge into several intervals (consecutive endpoints considering all endpoints). 
Our segtree leaves are such intervals. These will have weights just as handling a **histogram on a weighted array**.  
While traversing the X axis, we want to count how many times each interval occured and use these interval sizes in our area calculation.
Even if a interval is counted multiple times, it only contributes once to our area. 
Every time we process an event in out line sweep (addition or removal of a y-aligned edge i.e. contiguous interval in our segtree), we can query the whole segtree and increment the answer.

### "Best" interval after some updates for easily mergeble intervals
A node `X` with interval `[l;r]` can keep 3 infos: the best interval, preffix and suffix totally inside it.
When merging `X` to `Y` (`X` left and `Y` right) into `Z`:  
- `Z.best = max(X.best, Y.best, f(X.suffix + Y.prefix))`
- `Z.prefix = X.prefix` or `X.prefix + Y.prefix` when `X.prefix = [lx;rx]` 
- `Z.suffix = Y.sufix` or `X.sufix + Y.sufix` when `Y.sufix = [ly;ry]`  
Check: https://codeforces.com/edu/course/2/lesson/5/3/practice/contest/280799/problem/A 

## Linear recurrences on consecutive positions on Segtrees

Linear recurrences for DP when expressed over adjacent positions can be used in segtrees since matrixes can encode such transitions. Since matrix multiplication (or other operation in question) is associative, segtree can be used.

Check: https://atcoder.jp/contests/abc246/tasks/abc246_h  
Check: https://codeforces.com/gym/102644/problem/H  

### (Automaton) String editting and pattern matching
Given a pattern `P` and a modifiable string `S`, count how many patterns are inside `S` or modify `S`. This can be done using KMP automaton and segtree. For each `[l;r]` node for `S`, keep `to[x]` (to which automaton node the substr `S[l;r]` leads if you start traversing it from `x`) and `accs[x]` (how many times we visit the accepted node from the automaton if we start traversing `S[l;r]` from `x`).

Merge two nodes using function composition: `(a+b).to[x] = b.to[a.to[x]]` and `(a+b).accs[x] = b.accs[a.to[x]] + a.accs[x]`  
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

## HLD
### Non-commutative operations on arrays and trees
When dealing w/ non-commutative operations (eg. reading a string), a forward query `[l;r]` might differ from a backward `[r;l]` query.  
For handling queries in both ways (forward and backward), your node must keep 2 internal states, one for each way.  
When combining nodes, stablish that `(a+b).forward = merge(a.forward, b.forward)` and `(a+b).backward = merge(b.backward, a.backward)`.  
When querying, specify if it is a forward or a backward query. If its a forward query, on a `[l;r]` node, merge `[l;mid]`'s answer to `[mid;r]`'s answer and use only forward values. Otherwise, use only backward values and merge `[mid;r]`'s answer to `[l;mid]`'s answer.

On trees, going down and up are different directions, so this technique needs also to be used. Query and update orders need also to be respected at the HLD structure. 
Check: https://codeforces.com/gym/101908/problem/H
