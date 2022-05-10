# Strings

## KMP

### KMP with ordered patterns
KMP can also be used for ranked sequence matching i.e. find matchings of a `P_0 P_1 ... P_k` pattern in which, if `P_i > P_j` then `S_pi > S_pj` (`S_pi` char in substring matched to `P_i`). For this, we need to generalize the exact char matching (`P[i]==P[j]?`) of the original prefix function to a *can I fit `P[0:i]` into `P[j-i:j]`?* predicate. Such predicate .:
```
Given P[0:i] (pattern prefix I'm trying to fit) and P[j-i:j] (suffix for the current char),
If, for every distance D<i, the order relation (<,=,>) between (P_(j-D) and P_j) and (P_(i-D) and P_i)
(note that P_(j-D) is correspondent to P_(i-D)) are the same, then, the prefix can be fitted.

That is, in both prefix and suffix,
the last char which we are trying to add have the same order relation to the other chars positions.    

Note that, when trying to match to P[0:i], this same predicate was asserted for all char positions < i.    
Thus, only this new position needed to be checked.
```
Such formulation is however `O(n)`. Some optimizations can lead this to `O(1)`:
- `=> O(alphabet)`: for each possible value, we only need to check its last position. Suppose that our pattern has two occurrences of the same value, the second occourence has already accepted the first one, so we don't need to do this again.
- `=> O(1)`: it we are trying to fit value `v`, we only need to check the last potions of `prev(v)` and `next(v)` (not necessarily adjacent). Due to transitivity.


Check: https://szkopul.edu.pl/problemset/problem/6b-q_dPI_KRHD3VArapVq7EP/site/?key=statement  
Check: http://poj.org/problem?id=3167  

### KMP capabilities
**TODO**
Given two strings, KMP can solve in `O(n+m)`:  
1- biggest prefix from A that ends in `B[i]`  
2- biggest suffix from A that starts in `B[i]`: if a suffix `S` starts in `T[i]`, `S` and `S` suffixes end in `T[i]+len(S)-1`  
3- biggest substring between A and B  
4- biggest possibly rotated substring between A and B