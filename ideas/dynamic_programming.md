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
  
Let's say our modelling is something like `f(x) = min(f(y) + c)`. Using the direct method (in oppositon to successive approximation), this can be computed using **push** or **pull**.  
In the **push** method, we **update `f(x)` while in `f(y)`**. That is, we are pushing values from `f(y)` into `f(x)`. Also, `f(y)` is *contributing* to `f(x)` (more common in sums).
While in the **pull method**, we only **compute `f(x)` only when we reach it**. 

Note that, for both, when we reach `f(x)`, all `f(y)` that `f(x)` depends on must be already computed. 
Note that, when using **memoization (recursive DP)**, we are using the **pull method**.

Each problem is a problem but probably both pull and push methods can be used in every one of them (?). However, for some modellings, it may be that push or pull makes more sense.
  
For example, let's say that `f(x) = max_ab(f(a), f(b))`. How can we push here? Note that, while in `a`, trying to push into `x`, computing which state `b` is may be confusing ~(at least I thought so)~. This problem https://codeforces.com/contest/678/problem/E works like this. 
