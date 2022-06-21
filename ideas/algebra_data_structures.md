# Algebra of data structures



## Some definitions
```
Neutral/Identity element I of a binary operation F: F(I,x) = F(x,I) = x
Monoid: set of numbers + associative binary operation w/ identity element
A unary operation F is compatible to a binary G iff F(G(a,b)) = G(F(a),F(b))
A binary opeation F distributes over a binary operation G iff G(F(a, c), F(b, c)) = F(G(a, b), c)
Inverse element of element X in a binary operation F w/ identity element I: the inverse of X is Y iff F(X,Y) = I
```

### Comments

#### Associativity of a binary operation
If an op. is associative, one can accumulate it.  
**Example:** instead of inserting `N` batches of size `1` of balls, one can insert `1` batch of `N` balls

### Structures

#### Non-Idempotent Sparse Table
- Associativity
- **Example:** sum, gcd

#### Idempotent Sparse Table
- Associativity
- Idempotence
- **Example:** gcd, max, min

#### Fenwick Trees
- Associativity
- Neutral
- Inverse
- **Example:** sum, (prime) modular multiplication


#### Segtree
- Associativity
- Neutral
- **Example:** sum, gcd

#### Lazy Segtree
- There are two operations now. Let `F` be the lazy operation and `G` the query operation.
- Both need to be associative
- Both need to have a Neutral element
- Concerning the lazy mechanism
  - If `F` is unary, `F` needs to be compatible to `G`
  - If `F` is binary, `F` needs to be distributive over `G`
- **Example:** `F` multiplication and `G` sum, both `F` and `G` sum
