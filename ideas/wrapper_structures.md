# Wrapper structures
TODO write complexities.  

```
Q: query cost
I: insertion cost
M: merge cost
U: update cost
N: number of inserted elements
```

### Inserting after building (`log` structures) \[ONLINE\]
**Query: `O(logN * Q)`**
**Insert: `O(logN * I)`**
If you are dealing with structures without `inserts` after querying, you can keep a set of structures with sizes of power of 2, 
keeping only one structure with size `2^x` at a time.  
Always try to insert at the `2^0`-sized structure and solve `2^x` duplicates by merging and creating a `2^(x+1)`-sized structure.  
Query on all `O(log(elements))` structures.
  
Check: https://atcoder.jp/contests/abc244/tasks/abc244_h

### Implementing two pointers with 2 stacks / structures without `remove` \[ONLINE\]

A queue can be implemented using 2 stacks `A`, `B`, each stack will be responsible for a range `[lA;rA]` and `[lB;rB]`. The union of theses ranges is also contiguos.:
- New elements are added into `A`.
- Old elements are popped from `B`. When `B` reaches size `0`, build `B` using elements from `A`. Also, build it from `rB` to `lB`, that is, the top of the stack will be `lB`. Thus, when popping, elements from `[lB+1;rB]` will keep inside the structure.
  
This is particularly useful if you are implementing 2 pointers in a structure without `remove` operation.  
If the query operation is associative, query each stack and then combine their answers. Otherwise, merge them together for querying.
  
Check: https://www.codechef.com/problems/MIXFLVOR

### Removing from structures without `remove` \[OFFLINE\]
TODO

### Removing from structures without `remove` \[ONLINE\]
TODO
