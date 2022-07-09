# Number Theory

```
1- If D divides A and B, A < B, D divides B-A
2- Consecutive Fibonacci numbers are coprimes
3- floor(floor(A/B)/C) = floor(A/(B*C))
4- ceil(ceil(A/B)/C) = ceil(A/(B*C))
```

#### Arithmetic progressions with same sum
Between perfect squares X^2 and (X+1)^2, intervals [X^2;X^2+X] and [X^2+X+1;(X+1)^2-1] have the same sum. First interval w/ size X+1 and second w/ size X. 

#### LCM of sequence
The LMC of `a_0, a_1, ..., a_n` is the productory of `p^max_e` for each prime `p` that occurs in the factorization of `a_i` where `max_e` is the maximum exponent `p` has between all `a_i`.

#### GCD of `X+A_i`
By `(1)`, `GCD(X+A_i,X+A_j)=GCD(X+A_i, A_i-A_j)` and now by associativity and idempotency, `GCD(X+A_0, X+A_1, X+A_2, ...) = GCD(X+A_0, A_1-A_0, A_2-A_1, ...)`

### Permutation graphs are the union of simple cycles
They just are. Every node has outdegree `= 1`.

### Swapping elements and number of cycles
By choosing `i` and `j` (`i!=j`) and swapping `p_i` and `p_j`, in the functional graph, the number of cycles (including a cycle of size 1) increases or decreases by 1.  

Proof: if elements are from the same cycle, we are dividing this cycle into 2 new ones. If they are from different cycles, we are merging them.  

For instance, in order to reach the `1,2,...,N` permutation, we need to get `N` cycles. Thus, swap `N-c` times (`c`: current number of cycles).
