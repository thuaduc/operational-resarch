## Definitions

**Approximation complexity classes** — slide 4.

```
P  ⊆  FPTAS  ⊆  PTAS  ⊆  APX
```

| Class | Meaning |
|---|---|
| `P` | solvable exactly in polynomial time |
| `FPTAS` | ε-approximation for every constant `ε > 0`, time polynomial in **both** `n` and `1/ε` |
| `PTAS` | ε-approximation for every constant `ε > 0`, time polynomial in `n` only (`1/ε` may sit in the exponent) |
| `APX` | some **constant-factor** approximation exists |
| APX-complete | constant-factor approximation exists, but no PTAS unless `P = NP` |
| **not in APX** | no constant-factor approximation at all — **maximum clique, set cover, max independent set** |

**FPTAS** — slide 5: an ε-approximation algorithm for *each* constant `ε > 0`, producing an
arbitrarily good guarantee, terminating in time polynomial in `n` **and** `1/ε`. Trade-off between
optimality and time; such algorithms do not always exist.

**Bellman's principle of optimality** — slide 6:

```
Combining the optimal solutions of the subproblems yields an optimal solution overall.
Structural requirement: every partial solution of an optimal solution must itself be
                        the optimal solution of its corresponding subproblem.
```

Only then may DP build the answer bottom-up from ever larger subproblems.

**Knapsack DP table** — slide 7:

```
B[i, w] := maximal total value achievable with items 1..i under capacity w
           (i := item index, w := weight/capacity)
```

**Totally unimodular (TU) matrix** — slide 23.

```
A is TU  :⟺  every square submatrix of A has determinant in {−1, 0, +1}.
Consequence: every entry a_ij ∈ {−1, 0, +1}   (the 1×1 submatrices).
```

**Consecutive-ones property** — slide 32.

```
A ∈ {0,1}^{m×n} has the consecutive-ones property
  :⟺  ∃ permutation matrix P ∈ {0,1}^{m×m} such that in P·A the 1s of every column
       are directly consecutive.
Then A is TU.   (Interval matrices, e.g. shift-rostering matrices.)
```

**Incidence matrix, undirected** — slide 37: rows = vertices, columns = edges, `a_ij = 1` iff `v_i` is
incident to `e_j`. **Incidence matrix, directed** — slide 40: for `e_j = (k, i)`,
`a_kj = −1` (outgoing), `a_ij = +1` (incoming), all other entries 0.

**Independence system** `(E, I)` with `E` finite, `∅ ≠ I ⊆ 𝒫(E)` — CE D8.3:

```
(1) ∅ ∈ I
(2) (hereditary / downward closure)  B ∈ I and A ⊆ B  ⇒  A ∈ I
```

**Matroid** `(E, I)` — slide 45: an independence system that additionally satisfies

```
(3) (exchange / augmentation property)
    A, B ∈ I  with  |A| < |B|   ⇒   ∃ x ∈ B \ A  with  A ∪ {x} ∈ I
```

`E` = **ground set**, elements of `I` = **independent sets**, a **maximal** independent set is a
**basis**. Warning about the direction: the smaller set `A` is the one that gets augmented, and the
new element is taken from the *larger* set `B`.

**Rank function** — slide 57:

```
r_M : 𝒫(E) → ℕ₀ ,   r(B) = max{ |A| : A ⊆ B, A ∈ I }
```

i.e. the cardinality of the largest independent subset of `B`. For the graphic matroid of a connected
graph, `r(E) = |V| − 1` (spanning tree).

**Weighted matroid** — slide 52: `w : E → ℝ_{≥0}`, extended linearly by `w(A) = Σ_{a∈A} w(a)`.

---

## Procedures

### (0) Solve a 0-1 knapsack by DP

1. Choose the smaller of `W_max` and `V_max` as the DP table index.
2. Initialize `B[0, w] = 0` for all `w` and `B[i, 0] = 0` for all `i`.
3. Fill row by row using the recurrence:
   ```
   if w_i <= w:  B[i,w] = max{ B[i-1,w],  v_i + B[i-1, w-w_i] }
   else:         B[i,w] = B[i-1, w]
   ```
4. Optimal value is `B[n, W]`.
5. **Backtrack** from `(n, W)`: if `B[i,w] = B[i-1,w]` skip item `i`; otherwise take item `i` and set `w := w - w_i`. Repeat until `i = 0`.

### (0') Run the FPTAS

1. Compute `θ = ε·v_max/n`.
2. Scale values: `v_i* = ⌊v_i/θ⌋`. Leave weights and capacity unchanged.
3. Run the value-indexed DP on the scaled instance.
4. Return the item set with its original unscaled total value.
5. Guarantee: `V_approx ≥ (1−ε)·V_opt`.

### (1) Prove TU via Ghouila-Houri

1. Check all entries lie in `{−1, 0, +1}`.
2. Check every column has at most 2 non-zeros; if not, the criterion is not directly applicable.
3. 2-colour the rows: for each column with two non-zeros, same sign forces different parts, different signs forces same part.
4. If no conflict arises, state `(M₁, M₂)` and conclude TU.
5. On conflict, try the criterion on `Aᵀ`, `−A`, or `[A, I]` instead -- all preserve TU.

### (2) Disprove TU

1. Check for an entry outside `{−1, 0, +1}` -- instant disproof.
2. Otherwise find a square submatrix with `|det| ≥ 2`. Focus on zero-free `2×2` blocks first.
3. Exhibit the submatrix, compute its determinant, conclude not TU.

### (3) Find all parameter values making a matrix TU

1. **1x1:** each parameter must be in `{−1, 0, +1}`.
2. **2x2:** for each zero-free `2×2` submatrix containing a parameter, require `det ∈ {−1, 0, +1}`.
3. **3x3+:** expand only submatrices still containing a parameter.
4. Intersect all conditions and list surviving assignments.

### (4) Prove a set system is a matroid

1. **Axiom 1:** show `∅ ∈ I`.
2. **Axiom 2 (hereditary):** show `B ∈ I, A ⊆ B ⇒ A ∈ I`.
3. **Axiom 3 (exchange):** for `A, B ∈ I` with `|A| < |B|`, construct `x ∈ B \ A` with `A ∪ {x} ∈ I`.
4. Axioms 1+2 give an independence system; adding 3 gives a matroid.

### (5) Disprove a set system is a matroid

1. First check downward closure: find `B ∈ I` with some `A ⊆ B` where `A ∉ I`.
2. If closure holds, find `A, B ∈ I` with `|A| < |B|` where every `x ∈ B \ A` gives `A ∪ {x} ∉ I`.
3. State the counterexample explicitly.

### (6) Greedy algorithm for a weighted matroid

```
Greedy(M = (E, I), w):
  1. A := ∅
  2. sort E by w (increasing for min-weight basis, decreasing for max-weight)
  3. for each x in that order:
  4.     if A ∪ {x} ∈ I then A := A ∪ {x}
  5. return A
```

On the graphic matroid this is **Kruskal's MST algorithm**.

---

## Formula box

**Approximation classes**

- **Classes:** `P ⊆ FPTAS ⊆ PTAS ⊆ APX`; not in APX: max clique, set cover, max independent set
- **FPTAS:** ε-approx `∀ε > 0`, time polynomial in `n` AND `1/ε`

**Knapsack DP**

- **Recurrence:** `B[k,w] = B[k-1,w]` if `w_k > w`; `= max{B[k-1,w], B[k-1,w-w_k] + v_k}` otherwise
- **Runtime:** `O(nW)` by weight / `O(nV) = O(n²·v_max)` by value; brute force `O(2ⁿ)`
- **Pseudo-polynomial:** polynomial in the VALUE `W`, not in the input length `log W`
- **Which DP:** run the one indexed by `min(W_max, V_max)`
- **Backtrack:** `B[i,w] = B[i-1,w]` ⇒ item i out `(i−1)`; else item i in `(i−1, w−w_i)`

**FPTAS**

- **Scaling:** `θ = ε·v_max/n`, `v_i* = ⌊v_i/θ⌋`, max scaled value `n/ε`, runtime `O(n³/ε)`
- **Bound:** `V_approx ≥ V_opt − nθ = V_opt − ε·v_max ≥ (1−ε)·V_opt` (uses `v_max ≤ V_opt`)

**Total unimodularity**

- **TU:** `∀` square submatrices `S ⊆ A` : `det(S) ∈ {−1, 0, +1}`
- **Cramer:** `x_i = det(A_i)/det(A)`, `A_i` = `A` with column `i` replaced by `b`
- **Integrality:** `A` TU, `b ∈ ℤ^m` ⇒ `P = {x ∈ ℝ₊ⁿ : Ax ≤ b}` integral polyhedron
- **Closure:** `A` TU ⇒ `−A`, `Aᵀ`, `A⁻¹`, `[A, I]` TU; appending identity rows preserves TU
- **Ghouila-Houri:** entries in `{−1,0,1}` ∧ ≤2 non-zeros per column ∧ `∃(M₁,M₂)`: same sign → different parts, different signs → same part
- **2×2 shortcut:** a 2×2 submatrix containing a 0 always has `det ∈ {−1,0,1}`

**Matroids**

- **Matroid axioms:** (1) `∅ ∈ I` (2) `B ∈ I, A ⊆ B ⇒ A ∈ I` (3) `A,B ∈ I, |A| < |B| ⇒ ∃x ∈ B\A : A ∪ {x} ∈ I`
- **Basis:** maximal independent set; all bases have equal cardinality
- **Rank:** `r(B) = max{|A| : A ⊆ B, A ∈ I}`; graphic, connected: `r(E) = |V| − 1`
- **Weight:** `w(A) = Σ_{a∈A} w(a)` (linear weight function, `w : E → ℝ_{≥0}`)
- **Greedy:** increasing order → min-weight basis; decreasing → max-weight basis
- **Greedy runtime:** `O(n log n + n·f(n))`, `n = |E|`, `f(n)` = cost of one independence test

---
