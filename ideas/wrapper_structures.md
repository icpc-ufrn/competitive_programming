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

### Implementing two pointers without remove but with merge and stacks \[ONLINE\]
TODO
If you are doing two pointers keeping a structure without `remove` operation but with (a cheap) `merge`,
you can use a "queue" for removing (actually two stacks).  
Two stacks are needed since we don't want state `i+1` to keep info from `i` after its removal.  
By using two stacks, we keep one stack only for deleting where state `i` is built from `i+1`.

Check: https://www.codechef.com/problems/MIXFLVOR

### Removing from structures without remove \[OFFLINE\]
TODO

### Removing from structures without remove \[ONLINE]\
TODO
