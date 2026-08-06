
# §1 · LP basics and simplex

### Conversions

| From | To | Note |
|---|---|---|
| `aᵀx ≤ b` | `aᵀx + s = b`, `s ≥ 0` | **slack** — is a basis column |
| `aᵀx ≥ b` | `aᵀx − s = b`, `s ≥ 0` | **surplus** — *not* a basis column |
| `aᵀx ≥ b`, `b < 0` | flip the row by `−1` **first** | then add a slack |
| `aᵀx = b` | `aᵀx + w = b`, `w ≥ 0` | **artificial** — start basis only |
| `x` free | `x = x⁺ − x⁻`, both `≥ 0` | |
| `min cᵀx` | `−max(−cᵀx)` | |

### Geometry

A **vertex** ⟺ `n` linearly independent active constraints ⟺ a basic feasible solution
(`x_B = B⁻¹b ≥ 0`, `x_N = 0`). An optimum, if one exists, sits at a vertex.

### The four outcomes

| Situation | Outcome | Report |
|---|---|---|
| `X = ∅` | **infeasible** | no `z` |
| `∃ d` in the recession cone with `cᵀd > 0` | **unbounded** | `z → ∞` |
| `c ⊥` an optimal edge | **infinitely many optima** | `{(1−α)v₁ + αv₂ : α ∈ [0,1]}`, one `z*` |
| otherwise | **unique optimum** | at a vertex |

### Simplex iteration — max, z-row holds `−c`

```
ENTER   j = argmin (z-row)_j,  require (z-row)_j < 0
LEAVE   i = argmin { b_i / a_ij : a_ij > 0 }        strictly positive only
PIVOT   R_i ← R_i / a_ij ;  R_k ← R_k − a_kj·R_i   for k ≠ i, incl. z-row
```

| Tableau shows | Means |
|---|---|
| no negative z-row entry | **optimal** |
| entering column has no positive entry | **unbounded** |
| tie in the ratio test | next basis is **degenerate** |

---

# §2 · Reading a tableau

1. **All Row-0 entries `≥ 0`?** → optimal (max). Check this *first*.
2. **Basis column ↔ RHS column**, row by row → the basic variables and their values.
3. **Everything not in the Basis column = 0.** State these too.
4. **Row 0's RHS entry** → `z`.
5. `sᵢ = 0` → constraint binding · `sᵢ > 0` → slack, leftover is `sᵢ`.

No Basis column? Find the **unit columns** — the row holding the `1` gives that variable's value.

```
c_B     = objective coefficients of the BASIC variables, in Basis-column order
yᵀ      = c_Bᵀ B⁻¹                shadow prices
Row0_j  = yᵀa_j − c_j             optimal ⟺ all ≥ 0     ← COURSE sign convention
b'      = B⁻¹b                    the plan; feasible ⟺ b' ≥ 0
z       = c_Bᵀ b' = yᵀb
```

| Where in Row 0 | What it is |
|---|---|
| under a **basic** column | always `0` |
| under **slack `sᵢ`** | `yᵢ` — the shadow price |

---

# §3 · Sensitivity

### Which half breaks

| You change | What moves | So only this can break |
|---|---|---|
| `b` — a resource | the plan `b' = B⁻¹b` | **feasibility** |
| `c` — a price | Row 0 | **optimality** |

### RHS ranging · `bᵢ → bᵢ + δ`

1. Take the **column** of slack `sᵢ` — it *is* `B⁻¹eᵢ`, already in the tableau.
2. `x_B(δ) = b' + δ·(that column) ≥ 0` — one inequality per row.
3. Intersect → the `δ` range. Inside it, `z(δ) = z + yᵢ·δ`.
4. At each endpoint, **name the basic variable that hits zero**.

### Cost ranging · `c_k → c_k + Δ`

| `x_k` is | Use | Range | `z` inside |
|---|---|---|---|
| **basic** | that variable's **row**; new Row 0 = Row 0 + `Δ`·(row) `≥ 0` | two-sided | `z + Δ·x_k` |
| **non-basic** | its own entry only: `c'_j − Δ ≥ 0` | one-sided, `c_j ≤ yᵀa_j` | unchanged |

The plan is unchanged in both cases.

### New column, and two tableau readings

```
c'_new = yᵀa_new − c_new          enters ⟺ c_new > yᵀa_new
                                  with production cost k:  p ≥ yᵀa_new + k
```

| Tableau shows | Means |
|---|---|
| a **basic** variable with RHS `= 0` | **degenerate** — and simplex can cycle |
| a **non-basic** variable with Row 0 `= 0` | **multiple optima** |

The endpoints of an RHS range are exactly where degeneracy occurs.

---

# §4 · Duality

### Deriving the dual

1. `max` ↔ `min`.
2. One dual **variable** per primal **constraint** — sign restrictions don't count.
3. One dual **constraint** per primal **variable**.
4. Dual constraint `j` reads **down column `j`** of `A`.
5. Swap `c` and `b`.

### SOB — classify, then translate

| | S (sensible) | O (odd) | B (bizarre) |
|---|---|---|---|
| **variable** | `x ≥ 0` | `x` free | `x ≤ 0` |
| **constraint in max** | `≤` | `=` | `≥` |
| **constraint in min** | `≥` | `=` | `≤` |

| Type | primal **constraint** → dual **variable** | primal **variable** → dual **constraint** |
|---|---|---|
| S | `y ≥ 0` | sensible direction |
| O | `y` free | `=` |
| B | `y ≤ 0` | bizarre direction |

**Sensible direction** = `≥` if the dual is a min, `≤` if it's a max. Bizarre is the opposite.

### Theorems

```
weak     cᵀx ≤ bᵀy      for ALL feasible x and y
strong   cᵀx* = bᵀy*    at optimality
```

| If the primal is | Then the dual is |
|---|---|
| unbounded | infeasible |
| infeasible | unbounded **or** infeasible |
| feasible, and so is D | both finite |

### Complementary slackness

```
primal CS   ((Aᵀy)_j − c_j) · x_j = 0     per variable
dual CS     ((Ax)_i − b_i) · y_i = 0      per constraint
```

Slack constraint ⟹ `yᵢ = 0`. Non-zero variable ⟹ its dual constraint is tight.

### Showing `x*` is **not** optimal

1. Check `x*` is primal feasible — if not, you're already done.
2. Every **slack** constraint forces its `yᵢ = 0`.
3. Every `x_j ≠ 0` forces dual constraint `j` to **equality** — solve for `y`.
4. Test that `y` against the dual's **remaining constraints and sign restrictions**.

| Step 4 result | Conclusion |
|---|---|
| all satisfied | `x*` **is** optimal, `y` is the dual optimum |
| any violation | `x*` is **not** optimal ← the answer |

---

# §5 · IP modelling

### The answer template — this is the rubric

```
Introduce z ∈ {0,1}:  z = 1 ⟺ <meaning, in a full sentence>
    <linking constraint(s)>
Then:
    <the requirement, keyed on z>        ∀ i ∈ <explicit range>
```

Three things score separately: **the words**, **the linking**, **the `∀` with its range**.
Every auxiliary also needs its **domain line** — without `z ∈ {0,1}` the model is broken.

### Indicator — `t = 1 ⟺ X > K`, `X` integer

```
(A)  X ≤ K + M·t        "X > K ⟹ t = 1"      forces t UP
(B)  X ≥ (K+1)·t        "t = 1 ⟹ X > K"      forces t DOWN
```

| Wording | Write |
|---|---|
| "indicates whether", "if and only if" | **both** |
| "if more than K, then …" | **(A)** plus the consequence keyed on `t` |
| unsure | **both** — never penalised |

`M` is a **constant** — justify its size (`M = |J|`, `M = cᵢ`, `M = Σwⱼ`).
For **continuous** `X`, `t = 1 ⟺ X > K` is impossible: the set isn't closed.

### Patterns

| English | Constraint |
|---|---|
| exactly one | `Σ_{i∈I} x_{i,j} = 1  ∀j` |
| at most `k` | `Σ_i z_i ≤ k` |
| capacity | `Σ_{j∈J} x_{i,j} ≤ c_i  ∀i` |
| only if built | `x_{i,j} ≤ y_i  ∀i,j` — disaggregated, **stronger** |
| `A ⇒ B` | `A ≤ B` |
| several premises | `Σ(premises) − (#premises−1) ≤ Σ(conclusions)` |
| pairwise conflict | `y_i + y_i' ≤ 1` for pairs violating a threshold |
| either–or | `f₁ ≤ b₁ + Mz` and `f₂ ≤ b₂ + M(1−z)` |
| XOR | an indicator per block, mirrored with `z` / `(1−z)` |
| rolling window | `Σ_{t=k}^{k+6} x_t ≤ 5   ∀k ∈ {1,…,T−6}` |
| ratio / percentage | move everything left, clear denominators |
| product `x_k·x_l` | `Y ≤ x_k`, `Y ≤ x_l`, `Y ≥ x_k + x_l − 1` |
| fixed charge | `x ≤ M·y`, `x ≥ q·y` — nothing, or at least `q` |
| startup | `y_t ≥ x_t − x_{t−1}`, `t ≥ 2` |
| two objectives | `min Σf_i y_i − Σs_{ij} x_{ij}` — flip one sign |

On the product: if `Y` is **rewarded**, the first two suffice; if **penalised**, the third does.
All three is always safe.

### Traps

- missing `∀`, or the wrong range — `J\{21}`, `t ∈ {4,…,15}`
- an auxiliary variable with no name in words, or no domain line
- `M` treated as a variable
- one direction of an indicator when the wording said "iff"
- a product left non-linear
- merging two sub-questions into one answer

---

# §6 · Branch and bound

### Bounds and branching

| Problem | Relaxation gives | Integer `c` and `x` |
|---|---|---|
| max | an **upper** bound | `OPT ≤ ⌊z_LP⌋` |
| min | a **lower** bound | `OPT ≥ ⌈z_LP⌉` |

Branch on a **fractional** `x_i = f` into `x_i ≤ ⌊f⌋` and `x_i ≥ ⌈f⌉`. No integer point is lost —
none lies strictly between. Children **inherit** every ancestor constraint.

### The three pruning rules

| | Maximise | Minimise |
|---|---|---|
| **integrality** | LP integral; update `Z*` if `Z > Z*` | update if `Z < Z*` |
| **infeasibility** | relaxation has no feasible point | same |
| **bound** | `Z_node ≤ Z*` | `Z_node ≥ Z*` |

Write **MAX** or **MIN** at the top of your answer. A tie (`Z_node = Z*`) still prunes.

### Solving a node on the plot

| Branch constraint | Line | Keep |
|---|---|---|
| `x₁ ≤ 2` | vertical at 2 | left |
| `x₁ ≥ 3` | vertical at 3 | right |
| `x₂ ≤ 1` | horizontal at 1 | below |
| `x₂ ≥ 2` | horizontal at 2 | above |

Constraints accumulate down the path. **Empty region → prune by infeasibility**, no arithmetic.
Otherwise slide the objective line to the last vertex it touches.

### Node order

| | Structure | Takes |
|---|---|---|
| **FIFO** | queue — breadth-first | the **oldest** open node |
| **LIFO** | stack — depth-first | the **newest** open node |

Stop as soon as the incumbent is confirmed. **State which child you push first** — the answer
depends on it.

For every node record: **node · constraint added · vertex · `Z` · which rule closed it.**

---

# §7 · Total unimodularity

`A` is **TU** ⟺ every square submatrix has determinant in `{−1, 0, +1}`.
Hence every entry is in `{−1, 0, +1}`.

> **TU + integral `b` ⟹ integral polyhedron ⟹ the IP is solvable in polynomial time.**

### Proving it

| Route | What to say |
|---|---|
| **recognition** | incidence matrix of a **bipartite** or **directed** graph is TU — usually the whole answer |
| | consecutive-ones (interval matrix) is TU |
| **three conditions** | 1. entries in `{−1,0,+1}` · 2. at most 2 non-zeros per **column** · 3. rows split into `M₁, M₂`: same sign → different parts, opposite signs → same part |

**Do not write "Ghouila-Houri"** — the lecture never names it. Reproduce the three conditions.

### Disproving it

Find a **zero-free `2×2`** with `|det| ≥ 2`. Any `2×2` containing a zero always has
`det ∈ {−1,0,+1}`, so only zero-free blocks can break it.

**Closure:** `A` TU ⟹ `−A`, `Aᵀ`, `A⁻¹`, `[A, I]` are all TU.

---

# §8 · Matroids

### The axioms

```
(1)  ∅ ∈ ℐ
(2)  B ∈ ℐ, A ⊆ B  ⟹  A ∈ ℐ                        hereditary
(3)  A,B ∈ ℐ, |A| < |B|  ⟹  ∃x ∈ B\A : A∪{x} ∈ ℐ    exchange
```

> Axiom 3: the **smaller** set grows, the element comes from the **larger**.

`ℐ` is just "the selections the problem calls allowed". `E` is the pile you choose from.

### A concrete one to hold onto

```
       a          E = { ab, ac, bc }
      / \         ℐ = all edge sets with NO CYCLE
     b───c        so every set except {ab, ac, bc} itself
```

| | |
|---|---|
| independent | `∅`, `{ab}`, `{ac}`, `{bc}`, `{ab,ac}`, `{ab,bc}`, `{ac,bc}` |
| **not** independent | `{ab,ac,bc}` — a triangle |
| **bases** | the three 2-edge sets — note they all have size **2** |

### Prove / disprove

| Task | Method |
|---|---|
| **prove** | all three bullets, in order — (1) and (2) are one line each |
| **disprove** | try in this order: is `∅ ∈ ℐ`? → hereditary? → exchange? |

For exchange you must refute **every** `x ∈ B \ A` — so keep the example to 2–3 elements.

### Bases

**A basis is a MAXIMAL independent set** — independent, and you cannot add anything to it and
stay independent. *Maximal*, not maximum: nothing bigger contains it.

> **All bases of a matroid have the same size.**

**Proof** (SS23 E5b — four lines, pure axiom 3):

1. Suppose not. Take bases `B₁, B₂` with `|B₁| < |B₂|`.
2. Axiom 3 applies to them: there is an `e ∈ B₂ \ B₁` with `B₁ ∪ {e} ∈ ℐ`.
3. So `B₁ ∪ {e}` is independent and **strictly bigger** than `B₁`.
4. That contradicts `B₁` being **maximal**. Hence `|B₁| = |B₂|`. ∎

> The whole proof is: *a smaller basis could still grow, so it wasn't maximal.*

### SS23 E5c — the symmetric-difference trap

> Show or disprove: for two distinct bases, `(B₁ ∪ B₂) \ (B₁ ∩ B₂)` is a basis.

First, what that set **is** — the **symmetric difference**: everything in exactly one of the two,
with the shared part removed.

```
B₁ = {a}   B₂ = {b}      union {a,b} · intersection ∅ · difference {a,b}
B₁ = {1,2} B₂ = {2,3}    union {1,2,3} · intersection {2} · difference {1,3}
```

**The claim is FALSE.** Counterexample, two elements:

```
E = {a, b}        ℐ = { ∅, {a}, {b} }        "pick at most one"
```

Check it really is a matroid: `∅ ∈ ℐ` ✓ · hereditary ✓ · exchange — the only case is
`A = ∅`, `B = {a}` or `{b}`, and `∅ ∪ {x} ∈ ℐ` ✓.

```
bases:  B₁ = {a},  B₂ = {b}
(B₁ ∪ B₂) \ (B₁ ∩ B₂)  =  {a,b} \ ∅  =  {a,b}
but {a,b} ∉ ℐ  —  not even independent, let alone a basis.  ∎
```

> **Move to steal:** when a "show or disprove" asks about a *constructed* set, try the smallest
> matroid you can write down before attempting a proof. Two elements was enough here; three
> nodes was enough for SS24 P5b.

### Rank and greedy

| Term | Definition |
|---|---|
| rank | `r(B) = max{|A| : A ⊆ B, A ∈ ℐ}`; connected graphic: `r(E) = |V| − 1` |
| greedy | increasing weight → **min** basis · decreasing → **max** basis |

Greedy is optimal for **every** weight function **iff** the system is a matroid. That's why the
concept exists — Kruskal's MST is exactly greedy on the graphic matroid.

---

# §9 · Knapsack DP

```
B[i,w] = best value from items 1..i under capacity w

w_i ≤ w :   B[i,w] = max{ B[i−1,w] ,  v_i + B[i−1, w−w_i] }
w_i > w :   B[i,w] = B[i−1,w]
```

Row 0 and column 0 are all zeros. The answer is `B[n,W]`.

| Index by | Cost |
|---|---|
| weight | `O(n·W_max)` |
| value | `O(n·V_max)` |

**Run whichever bound is smaller — and say why.**

**Backtrack:** if `B[i,w] = B[i−1,w]` item `i` was not taken; otherwise it was, and `w := w − wᵢ`.

Pseudopolynomial, **not** polynomial. FPTAS: `θ = ε·v_max/n`, scale `vᵢ* = ⌊vᵢ/θ⌋`, run the
value-DP, report original values. Guarantee `V_approx ≥ (1−ε)·V_opt`. Hierarchy
`P ⊆ FPTAS ⊆ PTAS ⊆ APX`.

---

# §10 · Network flow

```
0 ≤ f(e) ≤ u(e)          conservation: in = out everywhere except s and t
val(f) = Σ_j f(s,j)
```

| Object | Rule |
|---|---|
| **cut** `S = [X, V\X]` | `s ∈ X`, `t ∉ X` |
| **cut capacity** | `Σ u(i,j)` over `i ∈ X`, `j ∉ X` — **forward arcs only** |
| **residual, forward** | `u(e) − f(e)` |
| **residual, backward** | `f(e)` — the undo button |

### Ford–Fulkerson

1. Find any `s–t` path in the residual network. None left → the flow is maximal.
2. `κ` = smallest residual capacity on that path.
3. `f += κ` on forward arcs, `f −= κ` on backward arcs. Repeat.

**Min cut:** `X` = the nodes reachable from `s` in the **final** residual network.
**Always verify `cap(S) = val(f)`** — a free correctness check.

`val(f) ≤ cap(S)` for every flow and cut; **max-flow = min-cut** at the optimum.
Node capacity → split `v` into `v_in → v_out` joined by an arc of that capacity.

---

# §11 · TSP

| Term | Means | Complexity |
|---|---|---|
| **Eulerian** | every **edge** once | polynomial |
| **Hamiltonian** | every **node** once | NP-complete |

TSP = minimum-weight Hamiltonian cycle. Symmetric: `(n−1)!/2` tours. Asymmetric: `(n−1)!`.

**Degree constraints alone are the assignment problem** — they permit subtours.

| | Constraint | Count | Relaxation |
|---|---|---|---|
| **SEC** | `Σ_{i,j∈U} x_ij ≤ |U|−1` for all `U`, `2 ≤ |U| ≤ n−1` | exponential | **tight** |
| **MTZ** | `u_i − u_j + 1 ≤ (n−1)(1−x_ij)`, `u₁=1`, `2 ≤ u_i ≤ n` | `O(n²)` | **weak** |

Use the lecture's `(n−1)`, not the textbook's `n`.

| Algorithm | Ratio | How |
|---|---|---|
| **MST-doubling** | **2** | double every MST edge → Euler tour → shortcut |
| **Christofides** | **3/2** | MST → min-weight matching on **odd-degree** vertices → Euler → shortcut |
| nearest neighbour | none | greedy, no guarantee |

Both ratios need the **triangle inequality**.

> ⚠ Papers mislabel MST-doubling as "Christofides". **Execute what is described.**

### Classifying a problem

| Wording | Class |
|---|---|
| pair `n` things one-to-one | assignment |
| one budget, maximise value | knapsack |
| minimise the number of containers | bin packing |
| cover every element, minimise cost | set covering |
| visit every node once and return | TSP |
| pick nodes so every **edge** is touched | vertex cover |

---

# §12 · Nonlinear, unconstrained

1. Solve `∇f = 0` for **all** critical points — factor, don't divide (`x·(…) = 0` has two branches).
2. Compute `H_f` symbolically.
3. **Plug in each critical point separately** — verdicts can differ.

### 2×2 test · `H = [[a,b],[b,d]]`, `det = ad − b²`, `tr = a + d`

| `det` | `tr` | Definiteness | Point is a |
|---|---|---|---|
| `> 0` | `> 0` | positive definite | **minimum** |
| `> 0` | `< 0` | negative definite | **maximum** |
| `< 0` | — | indefinite | **saddle** |
| `= 0` | — | — | inconclusive → eigenvalues |

Because `λ₁λ₂ = det` and `λ₁ + λ₂ = tr`.

### Leading principal minors · any size

| Pattern | Verdict |
|---|---|
| all `D_k > 0` | positive definite → **minimum** |
| `D₁ < 0, D₂ > 0, D₃ < 0, …` alternating from negative | negative definite → **maximum** |
| no `D_k = 0`, but neither pattern | indefinite → **saddle** |
| any `D_k = 0` | test **inapplicable** → eigenvalues |

### Eigenvalues · the fallback

Solve `det(H − λI) = 0`.

| Roots | Point is a |
|---|---|
| all `λ > 0` | **minimum** |
| all `λ < 0` | **maximum** |
| mixed signs | **saddle** |
| some `λ = 0` | genuinely inconclusive |

> **Shortcut:** `H ≻ 0` **everywhere** ⟹ strictly convex ⟹ **no maxima**, and any critical point
> is the unique global minimum. State this before you find the point.
> (`H ≻ 0` ⟹ strictly convex, but not conversely — see `x⁴`.)

---

# §13 · Convexity

```
convex SET        λx + (1−λ)y ∈ C                        ∀x,y ∈ C, λ ∈ [0,1]
convex FUNCTION   f(λx+(1−λ)y) ≤ λf(x) + (1−λ)f(y)
```

| Object | Compact | Convex |
|---|---|---|
| circle **curve** `x²+y²=r²` | ✓ | **✗** |
| closed **disk** `x²+y²≤r²` | ✓ | ✓ |
| halfplane | **✗** unbounded | ✓ |

> Convex objective **+** convex feasible set ⟹ every local minimum is **global**.

### Four routes to proving convexity

| Route | Use when |
|---|---|
| **definition** | `f` is abstract or non-differentiable — e.g. a norm |
| **Hessian** `H_f ⪰ 0` on a convex domain | `f` is differentiable |
| **rules** — `αf` (`α ≥ 0`), `f+g`, `f(Ax+b)`, `max{f_i}`, `h∘g` (`h` convex non-decreasing) | it's built from known pieces |
| **counterexample** — one triple `(x, y, λ)` | you need to **dis**prove |

```
norm:  ‖λx+(1−λ)y‖ ≤ ‖λx‖ + ‖(1−λ)y‖ = |λ|‖x‖ + |1−λ|‖y‖ = λ‖x‖ + (1−λ)‖y‖
                      triangle            homogeneity          λ ∈ [0,1]  ← say this
```

---

# §14 · KKT

### Standard form first

```
min f(x)   s.t.   g_i(x) ≤ 0 ,   h_j(x) = 0

g ≥ 0  becomes  −g ≤ 0            max f  becomes  min −f
```

```
L = f + Σ λ_i g_i + Σ μ_j h_j        λ_i ≥ 0  (inequalities)
                                     μ_j free (equalities)
```

### The four blocks

| | Condition |
|---|---|
| **stationarity** | `∇f + Σλ_i∇g_i + Σμ_j∇h_j = 0` |
| **primal feasibility** | `g_i ≤ 0`, `h_j = 0` |
| **dual feasibility** | `λ_i ≥ 0` |
| **complementary slackness** | `λ_i · g_i = 0` — so `λ_i = 0` **or** `g_i = 0` |

Case-split on complementary slackness. **Discard a case and say why** if `λ_i < 0` (dual
infeasible) or `g_i > 0` (primal infeasible). Kill cases early by arguing from the problem
statement that a constraint must be active or inactive.

### Constraint qualifications

| | Requires | Gives you |
|---|---|---|
| **Slater** | `f, g` convex; `h` affine; an `x̄` with every `g_i(x̄) < 0` **strictly** | a KKT point **is a global optimum** — no comparison needed |
| **LICQ** | active `∇g_i` and all `∇h_j` linearly independent | **candidates only** — you must compare `f` values |

Verify Slater by **exhibiting the point** (centroid, radius `+ 1`, and so on).

### The four-step recipe

1. **Existence** — feasible set compact (closed **and** bounded, Heine–Borel) and `f` continuous
   ⟹ a global min and max exist (Weierstrass).
2. **CQ** — Slater? Otherwise LICQ.
3. **Solve** — Lagrangian → four blocks → case split → discard.
4. **Compare** — evaluate `f` at the survivors. Skip only if Slater held.
