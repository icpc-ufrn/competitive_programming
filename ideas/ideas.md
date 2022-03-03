# Ideas
Summary of some ~not so~ frequent ideas

## General

### Automaton + Segtree
Automatons and segtrees can be combined in some ways...

### RMQ query on slinding window
Min/max heap with lazy delete, keep adding while you slide through

### Rotation `(x+y,x-y)`
You can transform `(x,y)` into `(x+y,x-y)`. Now, Manhattan distance is Chebyshev distance: `D((x1,y1), (x2,y2)) = max(abs(x1-x2), abs(y1-y2))`. Can be extended to multiple dimensions (https://oj.uz/problem/view/IOI07_pairs)

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





 



