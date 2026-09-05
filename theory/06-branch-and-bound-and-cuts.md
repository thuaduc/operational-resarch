## Definitions

**LP relaxation.** Drop `x ∈ ℤ` (or `x ∈ {0,1}` → `0 ≤ x ≤ 1`); keep everything else. The feasible set grows, so the relaxation optimum is never worse than the IP optimum.

**Subproblem / node.** The original LP plus the branching constraints accumulated on the path from the root. Node `P_0` = pure LP relaxation. In exam notation the subscript often names the constraint added (SS25: "`P_1` refers to the constraint that was added").

**Incumbent `Z*`.** The value of the best *integral* feasible solution found so far. `−∞` for max / `+∞` for min if none is known. For a max problem `Z*` is a valid **lower** bound on `OPT(IP)`.

**Global bound `Z'`.** The best (largest for max) LP value among all still-open subproblems. Gives `Z* ≤ OPT(IP) ≤ Z'` (max orientation). When the open list empties, the two coincide and the incumbent is proven optimal.

**Open subproblem.** A node whose relaxation value is still better than `Z*` and at least one child of which has not been solved. `L` = list of open subproblems.

**Pruning / fathoming.** Closing a node without exploring its children, on one of the three grounds below.

**Valid inequality.** An inequality `a^T x ≤ b` satisfied by **every** integer-feasible point of the IP. Adding it does not change the IP's feasible set, but can shrink the LP relaxation's polyhedron.

**Cutting plane.** A valid inequality that is *violated by the current fractional LP optimum* — it cuts that vertex off without losing any integer point.

**Ideal formulation.** The description of `conv{x ∈ ℤⁿ : Ax ≤ b, x ≥ 0}` — the **convex hull** of the integer feasible points ("integral hull"). Its vertices are all integral, so plain simplex solves the IP. This is the theoretical ideal outcome of adding cuts.

**Integrality / duality gap.** `z' − z`, the distance between the relaxation optimum `z'` and the IP optimum `z`. Model-dependent; a smaller gap means B&B prunes earlier.

## Procedures

### B&B in 6 steps

1. Solve LP relaxation `P_0`. If integral, done. Set `Z* = −∞` (max) or `+∞` (min).
2. Select an open subproblem from `L` using the stated node-selection rule.
3. Pick a fractional variable `x_i = f` and create two children: `x_i ≤ ⌊f⌋` and `x_i ≥ ⌈f⌉`. Each child inherits all ancestor constraints.
4. Solve each child's LP and record `x` and `Z`.
5. Test each child against the three pruning rules. If integral and better than `Z*`, update the incumbent.
6. If `L` is empty, return the incumbent; otherwise go to step 2.

### The three pruning rules

| # | Rule | Maximisation | Minimisation |
|---|---|---|---|
| 1 | **Integrality** | LP solution integral; update `Z*` if `Z > Z*` | LP solution integral; update `Z*` if `Z < Z*` |
| 2 | **Infeasibility** | LP relaxation infeasible | LP relaxation infeasible |
| 3 | **Bound** | `Z_node ≤ Z*` | `Z_node ≥ Z*` |

Prune when the node's optimistic bound is no better than the incumbent.

### FIFO vs LIFO node order

| | FIFO | LIFO |
|---|---|---|
| Full name | breadth-first | depth-first |
| Data structure | queue (pop oldest) | stack (pop newest) |
| Order | finish a level before descending | dive deep, then backtrack |
| Warm start | re-solve from scratch | reoptimise with dual simplex |
| First incumbent | may come late | found quickly |

Reading the order off a tree:
1. Start at `P_0`.
2. **FIFO:** process nodes level by level, left to right. Stop when the incumbent dominates all open bounds.
3. **LIFO:** always continue with the most recently pushed node; on pruning, backtrack to the deepest unexplored sibling.

### Reconstructing an obscured tree

1. `P_0` has no added constraints — match it to the row with "–" in the table.
2. Read the branch variable and its fractional value from the parent; edge labels are `x_i ≤ ⌊f⌋` and `x_i ≥ ⌈f⌉`.
3. A node's constraint set = parent's set plus its own edge label; find the matching table row.
4. Sanity-check: child `Z` must be weakly worse than parent `Z`; conflicting constraints mean infeasible.

### Solving a 2-variable subproblem graphically

1. Draw the original constraints; branch constraints are vertical or horizontal lines.
2. Shade the feasible intersection. If empty, prune.
3. Slide the objective line in the improving direction until it last touches the region; read off the vertex.

### Derive a Gomory fractional cut

1. Pick a row whose basic variable must be integral but has a fractional value in the final tableau.
2. Write the row as an equation in the non-basic variables:
   ```
   x_B + Σ_j ā_j x_j = b̄
   ```
3. Split each coefficient: `ā_j = ⌊ā_j⌋ + f_j`, `b̄ = ⌊b̄⌋ + f_0`, with `0 ≤ f < 1`.
4. Rearrange so the integer part is on the left and the fractional part on the right; the left side must be `≤ 0`.
5. State the cut in both forms:
   ```
   integer form:     x_B + Σ_j ⌊ā_j⌋ x_j − ⌊b̄⌋ ≤ 0
   fractional form:  f_0 − Σ_j f_j x_j ≤ 0
   ```

**Why ≤ 0 (validity):** for integer-feasible `x` the left side `x_B + Σ ⌊ā_j⌋ x_j − ⌊b̄⌋` is an integer, and by the split it equals `f_0 − Σ f_j x_j < 1` (`f_0 < 1`, `f_j ≥ 0`, `x_j ≥ 0`). An integer below 1 is at most 0.
**Why it cuts:** at the current LP vertex all nonbasic `x_j = 0`, so the fractional form reads `f_0 ≤ 0` — false, since `b̄` was chosen fractional (`f_0 > 0`). The cut removes the fractional vertex but no integer point.

To add to the tableau: rewrite the cut as `−Σ f_j x_j + x_{n+1} = −f_0` with slack `x_{n+1} ≥ 0`; append this row (coefficients **`−f_j`**, RHS `−f_0` — the NEGATED fractional parts). The tableau is now primal infeasible (negative RHS) but still dual feasible, so reoptimise with **dual simplex**.

### Worked Gomory cut (endterm 2026 P5, 15cr)

`max z = 5x₁ + 8x₂` s.t. `x₁ + x₂ + s₁ = 6`, `5x₁ + 9x₂ + s₂ = 45`, `x ∈ ℕ₀`. Optimal LP tableau (`s₁, s₂` nonbasic):

```
z :   0   0   5/4   3/4  │ 165/4
x₁:   1   0   9/4  −1/4  │  9/4
x₂:   0   1  −5/4   1/4  │ 15/4
```

1. Both basic variables fractional; take the `x₂` row: `x₂ − (5/4)s₁ + (1/4)s₂ = 15/4`.
2. Split with the **floor** — `0 ≤ f < 1` always, also for negative coefficients:
   - `−5/4 = −2 + 3/4` → `f₁ = 3/4`  (`⌊−5/4⌋ = −2`, so `f₁` is **not** `1/4` and never negative)
   - `1/4 = 0 + 1/4` → `f₂ = 1/4`
   - `15/4 = 3 + 3/4` → `f₀ = 3/4`
3. Both forms:
   ```
   integer:      x₂ − 2s₁ + 0·s₂ − 3 ≤ 0
   fractional:   3/4 − (3/4)s₁ − (1/4)s₂ ≤ 0   i.e.   (3/4)s₁ + (1/4)s₂ ≥ 3/4
   ```
4. Violation check: the LP optimum has `s₁ = s₂ = 0`, so the cut reads `0 ≥ 3/4` — violated, as required.
5. Append `−(3/4)s₁ − (1/4)s₂ + s₃ = −3/4` with `s₃ ≥ 0` basic:
   ```
   z :   0   0   5/4   3/4    0  │ 165/4
   x₁:   1   0   9/4  −1/4    0  │  9/4
   x₂:   0   1  −5/4   1/4    0  │ 15/4
   s₃:   0   0  −3/4  −1/4    1  │ −3/4
   ```
6. Dual simplex: leaving row `s₃` (only negative RHS); entering column by min ratio `|c̄_j / ā_rj|` over the row's **negative** entries: `s₁: (5/4)/(3/4) = 5/3` vs `s₂: (3/4)/(1/4) = 3` → `s₁` enters. Pivot on `−3/4`:
   ```
   z :   0   0   0   1/3   5/3  │ 40
   x₁:   1   0   0  −1     3    │  0
   x₂:   0   1   0   2/3  −5/3  │  5
   s₁:   0   0   1   1/3  −4/3  │  1
   ```
   RHS ≥ 0, reduced costs ≥ 0 → optimal and **integral**: `x* = (0, 5)`, `z = 40`. One cut closed the gap from `165/4 = 41.25` to 40.
7. In original variables (substitute `s₁ = 6 − x₁ − x₂`, `s₂ = 45 − 5x₁ − 9x₂`): the cut is `2x₁ + 3x₂ ≤ 15`.

### Express a cut in the original variables

1. Take the integer form of the cut.
2. Substitute each slack by its defining equation from the original constraints.
3. Simplify to `a^T x ≤ b` in the original variables and divide out any common factor.

## Formula box

**Bounds and bracketing**

- **Relaxation bound:** `max: OPT(IP) ≤ Z_LP` / `min: OPT(IP) ≥ Z_LP`
- **Unbounded relaxation:** `Z_LP = +∞` ⇒ no pruning by bound at that node; only if integrality *alone* was dropped (rational data) does it imply the IP itself is infeasible or unbounded
- **Bracketing (max):** `Z* ≤ OPT(IP) ≤ Z'`
- **Prune by bound:** `max: Z_node ≤ Z*` / `min: Z_node ≥ Z*`
- **Quality guarantee:** `k = Z'/Z* ⇒ k·Z* = Z' ≥ OPT(IP)`
- **Integrality gap:** `z' − z` with `z ≤ z' = q'`
- **Rounded bound:** `c ∈ ℤⁿ, x ∈ ℤⁿ ⇒ max: OPT(IP) ≤ ⌊Z_LP⌋, min: OPT(IP) ≥ ⌈Z_LP⌉`

**Branching**

- **Branching:** `x_i = f ∉ ℤ → x_i ≤ ⌊f⌋ | x_i ≥ ⌈f⌉`
- **Binary branching:** `x_i = 0 | x_i = 1`

**Gomory cuts**

- **Gomory row:** `x_B + Σ ā_j x_j = b̄`
- **Split:** `ā_j = ⌊ā_j⌋ + f_j, b̄ = ⌊b̄⌋ + f_0, 0 ≤ f < 1`
- **Cut (integer):** `x_B + Σ ⌊ā_j⌋ x_j − ⌊b̄⌋ ≤ 0`
- **Cut (fractional):** `f_0 − Σ f_j x_j ≤ 0`
