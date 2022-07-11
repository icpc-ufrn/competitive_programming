# Dynamic Programming

#### Basics
In DP problems, we will first have a **functional equation** (`f(x) = ...f(y)...`) that states the optimal modelling. Such is recursive, thus, the need of DP. 
  
We will call `f(x)` the LHS and `...f(y)...` the RHS. Also, we will say that `f(x)` depends on `f(y)`. Note that the relation *depends on* creates a graph on our states.  
  
I guess that in most of the problems this graph is a DAG or at most it has some self loops that are easy to handle.

When solving a DP problem, it **always** has to be clear (it is pointless to keep coding without these):
- **Base case**
- **Functional equation**

As solving a DP is only a smart way of traversing a graph, we need to visit each edge of this graph (transitions of the DP). 
Thus, the cost of a DP is `O(E)`. This actually given `N` (number of states) and `T` (number of transitions), `O(N*T)`.  

Some DP optimizations work aiming to reduce the cost of considering edges for a state:
- Using a query structure for `max`, `min`, `+`, ...
- Using a CHT structure for efficient evaluation of linear functions

#### Pull and push

Assuming our graph of dependence doesn't contain cycles,  
  
Let's say our modelling is something like `f(x) = min(f(y) + c)`. Using the direct method (in oppositon to successive approximation), this can be computed/implemented using **push** or **pull**.  
In the **push** method, we **update `f(x)` while in `f(y)`**. That is, we are pushing values from `f(y)` into `f(x)`. Also, `f(y)` is *contributing* to `f(x)` (more common in sums).
While in the **pull method**, we only **compute `f(x)` only when we reach it**. 

Note that, for both, when we reach `f(x)`, all `f(y)` that `f(x)` depends on must be already computed. 
Note that, when using **memoization (recursive DP)**, we are using the **pull method**.

Depending on the problem, implementing using the pulling may be easier than pushing or vice-versa. One can also combine pushing and pulling in the same modelling.
  
For example, let's say that `f(x) = max_ab(f(a), f(b))`. How can we push here? Note that, while in `a`, trying to push into `x`, computing which state `b` is may be confusing ~(at least I thought so)~. This problem https://codeforces.com/contest/678/problem/E works like this. 
  
Pushing and pulling in the same implementation: https://atcoder.jp/contests/abc242/editorial/3548.

## Modellings

### Knapsack - biggest subset with bounded cost 
DP where you maintain `A[x]: minimum cost of using x elements` and iterate through elements, minimizing `A[x]` when possible

### Game: Random vs. Greedy strategy / Black vs. white balls
Two players take elements from an array; one follows a greedy strategy and other a random. Use dynamic programming for computing `dp[i][j]: probability of j-th element be taken by first player given that i elements are laid`

Check: https://codeforces.com/problemset/gymProblem/102916/D

### Transition querying range of elements
Approach solving the transitions using a RMQ/segment query structure.  
  
For instance, `f(i) = true, iff for any i in [l;r], f(i + 1) is true`, one can solve `f(i)` by range querying `[l+1;r+1]`.
  
Check: https://codeforces.com/contest/985/problem/E

### How many ways to choose at least one element for each pair of two consecutive elements
`f(n) = f(n - 1) + f(n - 2)`  
Insert the `n`-th element by either skipping `n-1` or not.

### Choosing elements in a circular array
Let's say `f(n)` is defined as `g(n)` but on a circular array and `g(n)` builds on choosing elements or not.  
Solve `g(n)` while keeping an extra state, a flag, indicating whether the first element was taken or not. Now, each state `g[n]` will have two versions (`g[n][0]` and `g[n][1]`) indicating whether the first element was already taken. Use this to solve `f(n)` by solving cases.



