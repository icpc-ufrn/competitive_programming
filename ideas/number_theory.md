# Number Theory

```
1- If D divides A and B, A < B, D divides B-A
2- Consecutive Fibonacci numbers are coprimes3
3- floor(floor(A/B)/C) = floor(A/(B*C))
4- ceil(ceil(A/B)/C) = ceil(A/(B*C))
5- Between perfect squares X^2 and (X+1)^2, intervals [X^2;X^2+X] and [X^2+X+1;(X+1)^2-1] have the same sum. First interval w/ size X+1 and second w/ size X. 
```

#### GCD of `X+A_i`
By `(1)`, `GCD(X+A_i,X+A_j)=GCD(X+A_i, A_i-A_j)` and now by associativity and idempotency, `GCD(X+A_0, X+A_1, X+A_2, ...) = GCD(X+A_0, A_1-A_0, A_2-A_1, ...)`
