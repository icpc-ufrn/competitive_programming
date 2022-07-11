# FFT

### Basics
```
SUMi_1_n SUMj_1_n f(i+j)g(i)h(j) = SUMk_2_2n f(k)(g*h)(k)
```
https://cp-algorithms.com/algebra/fft.html  
https://codeforces.com/blog/entry/75326  

### Productory of `(a+b)` for `a` in `[La;Ra]` and `b` in `[Lb;Rb]`
Let's say we want to compute `F(La, Ra, Lb, Rb) = PRODa_La_Ra PRODb_Lb_Rb (a+b)`.  
This can be solved using FFT by reducing this to `PRODs_(La+Lb)_(Ra+Rb) s^ex` and finding `ex` using FFT.
Since `ex` is the number of sums `a+b=s`, if we create polynomial `A` with `a_i = 1 iff i \in [La;Rb]` and polynomial `B` similar,
`(A*B)_s` will hold this value.
  
Check: https://codeforces.com/gym/103643/problem/J

### Solving `O(n^2)`DP using FFT in `O(n*log^2)`
Given is a functional equation of the form `f(i,j) = cx_i * f(i-1,j-c) + dx_i * f(i-1,j-d) + ...` where `c,d,...` are non-negative integers. This can be solved using divide-and-conquer on FFT.  

See `f(i-1,...)` and the transitions at `i` as polynomials. Computing `f(i,...)` is thus polynomial multiplication. 
For instance, if `f(i,j) =  f(i-1,j-1) + g(i) * f(i-1,k)`.
If `f(i-1,...)` is a polynomial in which `f(i-1,j) x^j` (coefficients are `f` values), the transition can be seen as a `g(i) * x^0 + x^1` polynomial (call it `P_i`).  
Thus, `f(i, ...) = f(i - 1, ...) * P_i`. 
Finally, since polynomial multiplication is associative, divide-and-conquer can be used solving `[l;r]` multiplication ranges.

Check: https://atcoder.jp/contests/abc247/tasks/abc247_h  
Check: https://codeforces.com/problemset/problem/1613/F
