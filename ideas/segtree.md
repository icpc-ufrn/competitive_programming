# Segtree

### Histogram on a weighted array
Let's say we have an array where `a_i` is a weight and we have operations like `add(lu;ru)`: add weights from `a_i`, (`lu <= i <= ru`) to out Segtree.  
What we can do is set for each Segtree node `x` with interval `[l;r]` a variable `val[x]`: sum of `a_i` (`l <= i <= r`) and use this `val[x]` in our operations:  
`add(lu;ru)` will processed as add `val[x]` to `seg[x]` if `x` is inside `[lu;ru]`.

### Area of the union of rectangles
We want the area of the union of rectangles.
We can do this with line sweep + segtree, traversing the X axis (left to right). 
Note that we can decompose every y-aligned edge into several intervals (consecutive endpoints considering all endpoints). 
Our segtree leaves are such intervals. These will have weights just as handling a **histogram on a weighted array**.  
While traversing the X axis, we want to count how many times each interval occured and use these interval sizes in our area calculation.
Even if a interval is counted multiple times, it only contributes once to our area. 
Every time we process an event in out line sweep (addition or removal of a y-aligned edge i.e. contiguous interval in our segtree), we can query the whole segtree and increment the answer.

### "Best" interval after some updates for easily mergeble intervals
A node `X` with interval `[l;r]` can keep 3 infos: the best interval, preffix and suffix totally inside it.
When merging `X` to `Y` (`X` left and `Y` right) into `Z`:  
- `Z.best = max(X.best, Y.best, f(X.suffix + Y.prefix))`
- `Z.prefix = X.prefix` or `X.prefix + Y.prefix` when `X.prefix = [lx;rx]` 
- `Z.suffix = Y.sufix` or `X.sufix + Y.sufix` when `Y.sufix = [ly;ry]`  
Check: https://codeforces.com/edu/course/2/lesson/5/3/practice/contest/280799/problem/A 

## Linear recurrences on consecutive positions on Segtrees

Linear recurrences for DP when expressed over adjacent positions can be used in segtrees since matrixes can encode such transitions. Since matrix multiplication (or other operation in question) is associative, segtree can be used.

Check: https://atcoder.jp/contests/abc246/tasks/abc246_h  
Check: https://codeforces.com/gym/102644/problem/H  

### String editting and pattern matching `[Automaton]`
Given a pattern `P` and a modifiable string `S`, count how many patterns are inside `S` or modify `S`. This can be done using KMP automaton and segtree. For each `[l;r]` node for `S`, keep `to[x]` (to which automaton node the substr `S[l;r]` leads if you start traversing it from `x`) and `accs[x]` (how many times we visit the accepted node from the automaton if we start traversing `S[l;r]` from `x`).

Merge two nodes using function composition: `(a+b).to[x] = b.to[a.to[x]]` and `(a+b).accs[x] = b.accs[a.to[x]] + a.accs[x]`  
Check: https://codeforces.com/gym/101908/problem/H

### Cost (chars to erase) in order to get to the accepted state `[Automaton]` 
We want to know the minimal (cost) chars to erase in order to accept some subseqs and avoid some others. 
An automata w/ costs on edges can be modelled and for each state, there will be 3 types of edges:
- Edges for advancing: reading its char means we won't delete it and we will advance to the next automata state
- Edges for staying: reading its char means we will delete it and we will force the automata to stay in this state
- Useless edges: reading its char doesn't interfer in the current state  
Note that the edges for advancing and staying have the same charset. Edges for staying have cost 1 (since we need to delete 1 char) while other edges have 0 cost.

A segtree node `(l;r)` will keep the cost of minimal path between the automata states. Thus, we only need to check the cost between the start and accepted state. Nodes can be combined using the Floyd-Warshall algorithm. `(l;l)` nodes deal with the `s[l]` char; edges that don't have `s[l]` in their charset are setted to infinite cost.

Check: https://codeforces.com/problemset/problem/750/E

## Persistent Segtree

### Each `a[i]` has a range, find for `[L;R]` `a[i]` s.t. `a[i].l <= L, R <= a[i].r`, intervals are inside `[1;N]`

There will be `N` versions of `seg`, one for each position in `[1;N]`, `seg_i` holds:
- all `a[j]` s.t. `a[j].l <= i`. `a[j]` is added at position `j` as a pair `(a[j].r, j)` (right limit and its id)

Note that `seg` at version `i` acts as a prefix, keeping all items w/ endpoint leq `i`. Thus, we will build this seg as a prefix:
- group elements by their `.l`
- iterate `i` from `1` to `N`, creating a new version of `seg` at `i` and adding in this position elements w/ `.l = i`.

When querying for interval `[L;R]`, query on `seg_L` for the max element comparing the `.r` added. If the `.r` found is `>= R`, the range of the respective element contains `[L;R]`.

### COT `[TODO]`

[segtree persistente❤️][prefixo][busca binaria paralela][histograma][arvore][lca]

Dada uma árvore com valores em seus nós, diga, para cada query (u,v,k), o k-esimo menor elemento que acontece no caminho de (u,v).

É uma ideia nova então é meio difícil de tentar trazer uma intuição por trás, então vou apresentando os pedaços e como eles chegam na solução.

-SP: segtree persistente
-lc(X): filho esq. de X
-rc(X): filho dir. de X
-r(X): limite dir. de X
-l(X): limite esq. de X
-qntd(X): qntd de valores ativos no hist. em X.
-hist(u): hist prefixo em u

(1): Para cada vértice da árvore, mantenha um histograma dos valores de vértices que acontecem da raiz até esse vértices. Essa info consegue ficar num node de uma SP. Custo O(nlogn).

Basta, numa DFS visitando X com pai Y, criar um novo node hist(X) "em cima" do hist(Y), agora com a contagem do valor de X atualizado. "em cima"(Y) é fazer um update em cima de Y, como de praxe em SP.

Note que o histograma de Y é um "prefixo" do histograma de X.

(2): Para uma segtree (normal) de histograma, achar o k-esimo menor valor em O(logn).

É uma ideia recursiva/BB.
Estamos procurando o k-esimo menor valor no nó X (com filhos lc(X) e rc(X)).
Note que qntd(lc(X)) nos diz qntd. valores ativos em [l(lc(x));r(lc(x))]
Se k > qntd(lc(X)), procure em rc(X) com k = k - lc(X).
Senão, procure em lc(X) com k.

(3): É possível combinar (somando ou subtraindo) e achar o k-esimo menor elemento em O(logn) de duas segtrees de histogramas.
É necessário que haja uma correspondência em cima dos intervalos de seus nós.

Sejam X e Y as raízes das segtrees. Temos que L = l(lc(X)) = l(lc(Y)) e R = r(lc(X)) = r(lc(Y)).
Seguindo a mesma ideia de (2), temos a quantidade de elementos no intervalo [L;R] ao combinarmos qntd(X) e qntd(Y) (somando ou subtraindo).
Compare k com qntd(X) +/- qntd(Y) e siga os passos semelhantes a (2).

(4): O k-esimo elemento no caminho de u até v pode ser econtrado usando (1) e (3).

Note que combinando (+hist(u), +hist(v), -hist(lca(u,v)), -hist(par(lca(u,v)))) tem a contagem dos valores no caminho de u até v.
Para combinar esses histogramas, tome seus nós na SP construídos em (1) e execute o algoritmo descrito em (3).

## HLD
### Non-commutative operations on arrays and trees
When dealing w/ non-commutative operations (eg. reading a string), a forward query `[l;r]` might differ from a backward `[r;l]` query.  
For handling queries in both ways (forward and backward), your node must keep 2 internal states, one for each way.  
When combining nodes, stablish that `(a+b).forward = merge(a.forward, b.forward)` and `(a+b).backward = merge(b.backward, a.backward)`.  
When querying, specify if it is a forward or a backward query. If its a forward query, on a `[l;r]` node, merge `[l;mid]`'s answer to `[mid;r]`'s answer and use only forward values. Otherwise, use only backward values and merge `[mid;r]`'s answer to `[l;mid]`'s answer.

On trees, going down and up are different directions, so this technique needs also to be used. Query and update orders need also to be respected at the HLD structure. 
Check: https://codeforces.com/gym/101908/problem/H
