## Definitions

Throughout: standard form `max cᵀx s.t. Ax = b, x ≥ 0`, `A ∈ R^{m×n}`, rank `m`.

| Term | Definition / size |
|---|---|
| **Basis** `B` | the `m×m` submatrix of `A` of the `m` basic columns, invertible. Also used for the index set. |
| `N` | `m×(n−m)` submatrix of the nonbasic columns. `A = (B \| N)` after reordering. |
| `c_B`, `c_N` | objective coefficients of basic / nonbasic variables (`m`, `n−m`). |
| **Basic solution** | `x_B = B⁻¹b =: b'`, `x_N = 0`. **Feasible (BFS)** iff `b' ≥ 0`. |
| `N' = B⁻¹N` | the **body** of the tableau under the nonbasic columns (`m×(n−m)`). |
| `b' = B⁻¹b` | the **RHS column** of the tableau. |
| **Simplex multipliers / dual prices / shadow prices** `y` | `yᵀ = c_Bᵀ B⁻¹` (row vector, length `m`). Also written `π`. Equals the optimal dual solution. |
| **Row 0 entry** of column `j` (course notation `c'_j`) | `c'_j = −(c_j − a_jᵀ y) = yᵀa_j − c_j`. Vector form `c'_Nᵀ = −(c_N − N'ᵀc_B)ᵀ = c_Bᵀ B⁻¹N − c_Nᵀ`. |
| **Reduced cost** (textbook sign) `c̄_j` | `c̄_j = c_j − yᵀa_j = −c'_j`. The course calls `c'_j` "reduced cost" too — **the course sign is the negated one**. |
| **Shadow price of resource `i`** | Row-0 entry under the slack `s_i` of constraint `i`: `c'_{s_i} = yᵀe_i − 0 = y_i`. Marginal value of one extra unit of resource `i`. |
| **Binding constraint** | slack `s_i = 0` at the optimum ⇒ `y_i ≥ 0`, typically `> 0`. **Non-binding** (`s_i > 0`) ⇒ `y_i = 0`. |
| **Degenerate BFS** | some basic variable equals `0` in `b'`. |

**Sign convention — memorise this, the exam grades it.** The course writes Row 0 as
```
Row 0 (column j)  =  c_B^T B^-1 a_j  -  c_j        [ = -(c_j - a_j^T y) ]
```
so for a **max** problem the tableau is **optimal ⟺ every Row-0 entry ≥ 0**, and a variable is **attractive to enter ⟺ its Row-0 entry < 0** (most negative = entering rule). Under basic columns Row 0 is automatically `0`, because `c_Bᵀ B⁻¹ B − c_Bᵀ = 0ᵀ`. Slacks have `c_j = 0`, so their Row-0 entries are exactly the `y_i` — never subtract anything there.

## Procedures

### 1. Rebuild the full tableau from a basis `B`
1. Write `B` (basic columns in row order) and `N` (remaining columns).
2. Invert: `[B | I] → [I | B⁻¹]`.
3. Check `b' = B⁻¹b ≥ 0`; if not, basis is infeasible.
4. Body: `N' = B⁻¹N` (identity under basic columns).
5. `yᵀ = c_Bᵀ B⁻¹`.
6. Row 0: entry of column `j` is `yᵀa_j − c_j`; put `0` under basic columns.
7. `z = c_Bᵀ b'`. Optimal iff all Row-0 entries `≥ 0`.

### 1b. Revised simplex iteration
1. Start with a feasible basis `B`.
2. Compute `B⁻¹`, `b' = B⁻¹b`, `N' = B⁻¹N`, `c'_N = −(c_N − N'ᵀc_B)`.
3. `c'_N ≥ 0` → optimal, stop.
4. **Entering:** most negative `c'_k`.
5. Compute `d = B⁻¹a_j`. If `d ≤ 0` → **unbounded**, stop.
6. **Leaving:** `argmin{ b'_i / d_i : d_i > 0 }`.
7. Swap entering/leaving in `B` and `N`. Go to 2.

### 2. Cost ranging — basic variable `x_k`, `c_k → c_k + Δ`
1. `b'` unchanged — same plan, only `z` moves.
2. Row 0 updates: `c'_j(Δ) = c'_j + Δ·(row-of-x_k)_j` for all nonbasic `j`.
3. Impose `c'_j(Δ) ≥ 0` for all nonbasic `j`; intersect bounds → `Δ` interval.
4. Report `c_k ∈ [c_k + Δ_min, c_k + Δ_max]`, `z(Δ) = z + Δ·x_k`.
5. Outside the range: first `c'_j` to go negative → entering variable; one pivot gives the new plan.

### 3. Cost ranging — nonbasic variable `x_j`, `c_j → c_j + Δ`
Only one Row-0 entry moves: `c'_j(Δ) = c'_j − Δ ≥ 0` → `Δ ≤ c'_j` → `c_j ∈ (−∞, c_j + c'_j]`. `z` and `x*` unchanged inside the range.

| | basic `x_k` | nonbasic `x_j` |
|---|---|---|
| what changes in the tableau | whole Row 0 (via the `x_k` row) | one Row-0 entry |
| range | two-sided, from `c'_j + Δ·(row)_j ≥ 0` over all `j∈N` | one-sided `c_j ≤ yᵀa_j` |
| `z` inside the range | changes: `z + Δ·x_k` | unchanged |
| `x*` inside the range | unchanged | unchanged |
| outside | basis change: entering = first `c'` to go negative | `x_j` enters |

### 4. RHS ranging — `b_i → b_i + δ`
1. Row 0 unchanged — optimality safe, only feasibility can break.
2. `x_B(δ) = b' + δ·(B⁻¹e_i)` where `B⁻¹e_i` = tableau column of slack `s_i`.
3. Impose `b'_r + δ·(B⁻¹e_i)_r ≥ 0` for every row `r`; intersect → `b_i ∈ [b_i + δ_min, b_i + δ_max]`. Use strict `> 0` if the problem requires variables to stay positive.
4. `z(δ) = z + y_i·δ`, valid on this interval.
5. At each endpoint, name the basic variable that hits `0`.
6. Two RHS moving together (`b_i + δ`, `b_j − δ`): use `B⁻¹(b + δe_i − δe_j)` and `z(δ) = z + (y_i − y_j)δ`.

### 5. Evaluate a new column `(a_new, c_new)`
1. Check that the resource withdrawals lie inside each RHS range from procedure 4.
2. Opportunity cost: `a_newᵀ y`.
3. Row-0 entry `c'_new = a_newᵀy − c_new`. Enters iff `c_new > a_newᵀ y`.
4. Minimum price with production cost `k`: `p ≥ a_newᵀy + k`.
5. If a resource requirement exceeds its validity range, price piecewise: first portion at current `y_i`, remainder at next shadow price from procedure 6.
6. Tableau update: new column `= B⁻¹a_new`, then pivot normally.

**New constraint:** plug `x*` in. Satisfied → redundant. Violated → append row with slack, eliminate basic variables from it; new slack is negative → dual simplex to re-optimise. Adding a constraint can only worsen `z`.

### 6. Recompute after RHS range is exceeded
1. Push `δ` to the violated endpoint. The basic variable hitting `0` is the leaving variable.
2. Find entering variable via dual ratio test: in the leaving row, among entries `< 0`, pick the one minimising `|c'_j / row_j|`.
3. Perform one pivot.
4. New shadow prices: read off new Row 0 under the slacks.
5. New validity range: re-run procedure 4 with the new basis.
6. Sanity: for a max problem, decreasing `b_i` can only increase `y_i` (concavity).

## Formula box

**Standard form:** `max cᵀx s.t. Ax = b, x ≥ 0`, `A = (B | N)`

**Basis Quantities**

- `b' = B⁻¹b` (RHS column; BFS iff `b' ≥ 0`)
- `N' = B⁻¹N` (tableau body under nonbasic cols)
- `yᵀ = c_Bᵀ B⁻¹` (shadow prices / simplex multipliers / dual sol.)
- `z = c_Bᵀ B⁻¹b = c_Bᵀ b' = yᵀb`

**Row 0** (course sign convention, max problem)

- `Row0_j = c_Bᵀ B⁻¹ a_j − c_j = yᵀa_j − c_j =: c'_j`
- `Row0 = c_Bᵀ B⁻¹ A − cᵀ` (0 under every basic column)
- `c'_Nᵀ = −(c_N − N'ᵀc_B)ᵀ`
- **Optimal** ⟺ `c'_j ≥ 0` for all `j` (textbook reduced cost `c̄_j = c_j − yᵀa_j = −c'_j`)
- **Slack** `s_i`: `c_j = 0, a_j = e_i` ⇒ `c'_{s_i} = y_i`

**Sensitivity — RHS** (`b_i → b_i + δ`)

- `x_B(δ) = b' + δ · (B⁻¹e_i) ≥ 0` (`B⁻¹e_i` = tableau column of `s_i`)
- `Δz = yᵀΔb = y_i · δ` (valid only while basis optimal)
- `Δx_B = B⁻¹Δb` (NOT given by `y`)

**Sensitivity — Cost, basic** `x_k` (`c_k → c_k + Δ`)

- `c'_N(Δ) = c'_N + Δ · (row of x_k in N') ≥ 0`
- `z(Δ) = z + Δ · x_k`

**Sensitivity — Cost, nonbasic** `x_j` (`c_j → c_j + Δ`)

- `c'_j − Δ ≥ 0` ⟺ `c_j + Δ ≤ yᵀa_j`

**New Column**

- `ā = B⁻¹a_new`, `c'_new = yᵀa_new − c_new`; enters iff `c_new > yᵀa_new`

**Unbounded**

- exists `j` with `c'_j < 0` and `B⁻¹a_j ≤ 0`
