# Geometry

### General observations
- Avoid using floating points (division, square root, trigonometric functions); try to use fractions
- Avoid doing too much comparisons w/ floating points
- Avoid using floating points with big magnitude. This implies a greater error.

##### *Catastrophic cancelletion*
Assume that the relative error is `10^-6`. 
Lets say we cancel out `A=10^6` and `B=10^6`, their erros are `10^0`. `A-B` is now `10^0` with error `10^0`.
We don't want this to happen.

### Side of polygon
A polygon has every side smaller ~or equal~ to the sum of all other sides.

### 3 colinear points
For each point `p`, do polar sort of all the points using `p` as center. If there are colinear points, there will be adjacent in this sorting.

### Helly's theorem
Let `C` be a finite family of convex polygons in `R^n` such that, for `k ≤ n + 1`, any `k` members of `C` have a nonempty intersection. 
Then the intersection of all members of `C` is nonempty.
Eg.: In the plane (2d), if, for every triple (3) of convex polygons, there is a non-empty intersection, the intersention of all polygons is non-empty.

### Pick's theorem
Let `P` be a polygon with integer coordinates for its vertices.
Let:
```
- A: Area of P
- i: #integer points inside the polygon
- b: #integer points in the polygon boundaries
Then, A = i + (b/2) - 1
```
