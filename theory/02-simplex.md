## Definitions

| Term                              | Definition                                                                                                                                                         |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Normal form**                   | `max cᵀx s.t. Ax ≤ b, x ≥ 0`                                                                                                                                       |
| **Standard form**                 | `max cᵀx s.t. Ax = b, b ≥ 0, x ≥ 0` (equalities, nonneg. RHS)                                                                                                      |
| **Canonical form**                | standard form + every non-trivial constraint has a variable normed to 1 that appears in no other row ⇒ an identity submatrix ⇒ a basis is readable off the tableau |
| **Slack variable**                | added to a `≤` row: `aᵀx ≤ b ↦ aᵀx + s = b`, `s ≥ 0`                                                                                                               |
| **Surplus variable**              | subtracted from a `≥` row: `aᵀx ≥ b ↦ aᵀx − s = b`, `s ≥ 0`. Gives `−1` column ⇒ **not** a basis column                                                            |
| **Artificial variable** `wᵢ ≥ 0`  | added to `=` and `≥` rows purely to supply a starting basis column. Has no meaning in the original LP; must end at 0                                               |
| **Basis** `B`                     | index set of `m` columns of `A` with `A_B` invertible                                                                                                              |
| **Basic solution**                | `x_B = A_B⁻¹b`, `x_N = 0`                                                                                                                                          |
| **Basic feasible solution (BFS)** | basic solution with `x_B ≥ 0`. **BFS ⟺ vertex (extreme point) of the polyhedron**                                                                                  |
| **Reduced cost**                  | the z-row entry of a nonbasic column in the current tableau (`c̄_j = c_j − c_Bᵀ A_B⁻¹ A_j`, sign-flipped in the tableau convention below)                          |
| **Degenerate BFS**                | a **basic** variable equals 0 (i.e. `bᵢ = 0` in some row); geometrically > n constraints meet at that vertex                                                       |
| **Cycling**                       | infinite repetition of a sequence of degenerate pivots (basis changes, same vertex, same z)                                                                        |

### Tableau layout (lecture / CE convention)

![[Pasted image 20260725124344.png]]
## Procedures

### One simplex iteration (max, z-row = `−c`)
1. **Optimality check.** All z-row entries `≥ 0` → stop, read `x*` from basic rows, `z*` from z-row Res.
2. **Entering.** Pick the most negative z-row entry (column `j`).
3. **Ratio test.** Over rows with `a_{ij} > 0` only, compute `bᵢ / a_{ij}`; pick the minimum. Ties → Bland: smallest index.
4. No `a_{ij} > 0` at all → **unbounded**, stop.
5. **Pivot.** Divide row `i*` by the pivot element, then eliminate column `j` in all other rows (including z-row).
6. **Relabel.** Row `i*` gets basis label `x_j`. Go to 1.


### Read a tableau — check order
1. Every row owns a unit column and every Res. `≥ 0`? If not, fix or reject.
2. Artificial in basis with Res. `> 0` → **infeasible**.
3. Any z-row entry `< 0`? No → optimal. Yes → continue.
4. For each negative z-row column: any `a_{ij} > 0`? No → **unbounded**. Yes → do ratio test.
5. Any basic row with Res. `= 0` → **degenerate**.
6. Optimal and a nonbasic column has z-row `= 0` → **multiple optima**; pivot to get second vertex.
7. Read `x*` from basic rows (nonbasic = 0), `z*` from z-row Res.

### Forensics A — reconstruct original LP from a tableau
1. Identify structure vs. slack variables.
2. Pivot backwards until the basis is exactly the slack set (feasibility of intermediates irrelevant).
3. The constraint block is now original `A | b`; each row reads `aᵀx ≤ bᵢ`.
4. Objective coefficients = negated z-row entries of structure columns.
5. Append `x ≥ 0`.

### Forensics B — parameter ranges for a prescribed pivot

| Requirement | Condition |
|---|---|
| `x_e` may enter | z-row entry of column `e` `< 0` |
| pivot element usable | `a_{l,e} > 0` |
| `x_l` wins ratio test | `b_l / a_{l,e} ≤ bᵢ / a_{i,e}` for all rows with `a_{i,e} > 0` |
| feasibility | `b_l ≥ 0` |
| parameter irrelevant to pivot | no condition → `∈ ℝ` |

## Formula box

**LP Forms**

- **Normal:** `max cᵀx, Ax ≤ b, x ≥ 0`
- **Standard:** `max cᵀx, Ax = b ≥ 0, x ≥ 0`
- **Canonical:** standard + identity submatrix (a basis is visible)

**Conversions**

- `aᵀx ≤ b` ↦ `aᵀx + s = b, s ≥ 0` (slack; +1 column, IS a basis column)
- `aᵀx ≥ b` ↦ `aᵀx − s = b, s ≥ 0` (surplus; −1 column, NOT a basis column)
- `aᵀx ≥ b, b<0` ↦ multiply row by −1 first (flip to ≤), then slack
- `aᵀx = b` ↦ `aᵀx + w = b, w ≥ 0` (artificial, only for the start basis)
- `x ∈ ℝ` ↦ `x = x⁺ − x⁻, x⁺,x⁻ ≥ 0`
- `min cᵀx = − max (−cᵀx)`

**Tableau**

- z-row holds `−c` (max convention)
- `x_B = A_B⁻¹ b`, `x_N = 0`
- `z = c_Bᵀ A_B⁻¹ b`

**Simplex Iteration**

- **Enter:** `j = argmin_j (z-row)_j`, require `(z-row)_j < 0` (min-problem, z-row = +c: take argmax > 0)
- **Leave:** `i = argmin { b_i / a_ij : a_ij > 0 }` -- STRICTLY positive only
- **Pivot:** `R_i ← R_i / a_ij`; `R_k ← R_k − a_kj · R_i` for all `k ≠ i` (incl. z-row)

**Tableau Outcomes**

- **Optimal:** all `(z-row)_j ≥ 0`
- **Unbounded:** `∃ j: (z-row)_j < 0` and `a_ij ≤ 0 ∀ i` ⇒ `OPT = +∞`
- **Degenerate:** `∃ basic i` with `b_i = 0` (⇔ min ratio = 0, z unchanged after pivot)
- **Multiple optima:** optimal tableau, nonbasic `j` with `(z-row)_j = 0`; `x*(α) = α x*₁ + (1−α) x*₂, α ∈ [0,1]`
- **Infeasible:** artificial `w_i` remains basic with `w_i > 0`
- **Bland's rule:** entering = smallest index with `(z-row)_j < 0`; leaving = smallest index among min-ratio ties ⇒ no cycling

**Counting**

- `#vertices ≤ C(n+m, m)` (bases); each BFS ↔ vertex, but a degenerate vertex corresponds to SEVERAL bases
