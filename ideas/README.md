# Ideas
Summary of some ~not so~ frequent ideas

## General

### Automaton + Segtree
Automatons and segtrees can be combined in some ways...

### RMQ query on slinding window
Min/max heap with lazy delete, keep adding while you slide through

### Manhattan to Chebyshev distante
Chebyshev distance: `D(X,Y)=max(|X_i-Y_i|)`
The Manhattan distance between points `X` and `Y` is "equivalent" to the Chebyshev distance between `X'` and `Y'`, where `P'` is the expansion of the abs for a point (all `2^(d-1)` combinations of signals. One coordinate can be fixed since we are taking the abs). Ex:
- `(x,y)` => `(x+y,x-y)`
- `(x,y,z)` => `(x+y+z,x+y-z,x-y+z,x-y-z)`
Check (https://www.spoj.com/problems/DISTANCE/)

## Structures

### Inserting after building
If you are dealing with structures without `inserts` after querying, you can keep structures with sizes of power of 2. Only one structure with size `2^x` can exist at a time, solve duplicates by merging and creating a `2^(x+1)` sized structure. Query on all ~log~ structures.

### Implement remove using stacks and merge
If you are doing two pointers keeping a structure without `remove` operation but with (a cheap) `merge`, you can use a "queue" for removing (actually two stacks). Two stacks are needed since we don't want state `i+1` to keep info from `i` after its removal. By using two stacks, we keep one stack only for deleting where state `i` is built from `i+1`.

## Randomized

### Blogewoosh 6
Given `n` numbers in a random order, the expected times that the maximum seen value changes is `O(log(n))` while processing such numbers.

## Greedy

### Argument exchange
When trying to find the optimal order for a greedy, analyze the relation between only 2 elements. Define a cost function `f` capable of computing over both orders `AB` and `BA`. Check wich conditions are held for `f(AB) < f(BA)` (assuming `A` goes first).

## DP

### Knapsack - biggest subset with bounded cost 
DP where you maintain `A[x]: minimum cost of using x elements` and iterate through elements, minimizing `A[x]` when possible

### Game: Random vs. Greedy strategy / Black vs. white balls
Two players take elements from an array; one follows a greedy strategy and other a random. Use dynamic programming for computing `dp[i][j]: probability of j-th element be taken by first player given that i elements are laid`


## Bipartite matching

### Merging nodes
Suppose a graph with sides A and B for bipartite matching and that the size of A is really small (`<15?`). Nodes from B may be merged if they connect to the same nodes from A. The number of condensed nodes is at most `2^sz(A)`. 

## Game theory

### Last to play wins/unwanted positions
If the problem presents this variation, set the "about to win" positions as unwanted positions (really high grundy). Thus, states leading only to such positions will get a losing status.

## String

### KMP with ordered patterns
...

### KMP capabilities
Given two strings, KMP can solve in `O(n+m)`
1- biggest preffix from A that ends in `B[i]`
2- biggest suffix from A that starts in `B[i]`: if a suffix `S` starts in `T[i]`, `S` and `S` suffixes end in `T[i]+len(S)-1`
3- biggest substring between A and B
4- biggest possibly rotated substring between A and B

### Sack with Aho
You can use Sack (union by size) algorithms in the fail-link tree (suffix-link tree) when solving Aho problems.