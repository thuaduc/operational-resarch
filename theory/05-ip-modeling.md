## Definitions

| Term | Definition |
|---|---|
| **LP** | `max{cᵀx : Ax ≤ b, x ≥ 0}` — all variables continuous |
| **MIP** (mixed) | `max cᵀx + hᵀy` s.t. `Ax + Gy ≤ b`, `x ≥ 0`, `y ∈ ℕ₀` — some continuous, some integer |
| **IP** | all structural variables in `ℕ₀` (`ℤ`) |
| **BIP** (binary) | `max cᵀx` s.t. `Ax ≤ b`, `x ∈ {0,1}` |
| **LP relaxation** | the IP with every integrality/binary requirement dropped (`x ∈ {0,1}` → `0 ≤ x ≤ 1`) |
| **Big-M** | a fixed, *sufficiently large* constant (a **number, not a variable**) used to switch a constraint off |
| **Formulation strength** | between two formulations of the same IP, the one whose LP-relaxation polytope is *smaller* is **stronger** (tighter bound, faster B&B) |
| **Set covering / partitioning / packing** | `min cᵀx, Ax ≥ 1` / `min cᵀx, Ax = 1` / `max cᵀx, Ax ≤ 1`, `x` binary |

## Procedures

### Modelling checklist

1. **Index sets.** Name each set and give its range.
2. **Parameters.** List all data constants; distinguish from variables.
3. **Decision variables with domains.** Give each variable a one-line meaning and a domain (`∈ {0,1}`, `∈ ℤ`, `≥ 0`).
4. **Objective.** `min`/`max` with fully indexed sums.
5. **Constraint families.** Each with its `∀` quantifier and explicit range; time-lagged constraints must start late enough.
6. **Sign / integrality block.** Restate domains for every variable including auxiliaries.

### How to choose M

- M must upper-bound the largest value the switched-off expression can take.
- Read it off another constraint when possible.
- If nothing bounds it, state a justification (e.g. `M = Σ_j c_j`).
- M is a **constant**, never a variable.

### Reading a verbal condition into a pattern

| Words | Reach for |
|---|---|
| "exactly one" / "each … must be assigned to one" | `Σ x = 1` |
| "at most k", "no more than" | `Σ x ≤ k` |
| "only if", "may only be built if" | implication `x₁ ≤ x₂` |
| "if and only if", "indicates whether" | **two** inequalities (iff-indicator) |
| "if more than K …, then …" | big-M indicator |
| "either … or", "at least one of the two constraints" | `M z` / `M(1−z)` pair |
| "in any 7 consecutive days" | rolling window with `∀k` |
| "is started / switched on" | `y_t ≥ x_t − x_{t−1}` |
| "at least X% of" | multiply out, clear denominators |
| two goals in one sentence | weighted-sum objective |

## Formula box

### 1. Logical operators on binaries (lecture slide 15)

- **OR:** `x₁ ∨ … ∨ x_n` ⟹ `x₁ + x₂ + … + x_n ≥ 1`
- **AND:** `x₁ ∧ … ∧ x_n` ⟹ `x₁ ≥ 1; x₂ ≥ 1; … ; x_n ≥ 1`
- **IMPL:** `x₁ ⇒ x₂` ⟹ `x₁ ≤ x₂` (equiv. `x₂ − x₁ ≥ 0`)
- **EQUIV:** `x₁ ⇔ x₂` ⟹ `x₁ − x₂ ≤ 0 ; x₂ − x₁ ≤ 0` (i.e. `x₁ = x₂`)
- **NOT:** `Z = ¬A` ⟹ `Z = 1 − A`

### 2. Selection from k values (slide 14)

- **Selection:** y may take only one value from `{a₁,…,a_k}`:
  - `y = Σ_j a_j x_j`, `Σ_j x_j = 1`, `x_j ∈ {0,1}`

### 3. Reified logic — a binary that *equals* a logical expression

- **Z = A ∨ B:** `Z ≥ A`, `Z ≥ B`, `Z ≤ A + B`
- **Z = A ∧ B:** `Z ≤ A`, `Z ≤ B`, `Z ≥ A + B − 1`
- **Z = (A ⇒ B):** `Z ≥ 1 − A`, `Z ≥ B`, `Z ≤ 1 + 2B − A`

### 4. Assignment / cardinality / capacity

- **Exactly one:** `Σ_{i∈I} x_ij = 1` `∀j ∈ J`
- **At most one:** `Σ_{j∈J} x_ij ≤ 1` `∀i ∈ I`
- **At most k of:** `Σ_{i∈I} z_i ≤ k`
- **Capacity:** `Σ_{j∈J} x_ij ≤ c_i` `∀i ∈ I`
- **Capacity + opening:** `Σ_{j∈J} x_ij ≤ c_i · y_i` `∀i ∈ I`
- **GAP capacity:** `Σ_{j∈J} d_ij x_ij ≤ s_i` `∀i ∈ I`
- **Knapsack:** `Σ_{j∈J} w_j x_j ≤ W`
- **Bin packing:** `Σ_{j∈J} d_j x_ij ≤ s · y_i` `∀i ∈ I`, `min Σ_i y_i`

### 5. Linking (facility open) — disaggregated vs aggregated

- **Disaggregated:** `x_ij ≤ y_i` `∀i ∈ I, ∀j ∈ J` (|I|·|J| rows)
- **Aggregated:** `Σ_{j∈J} x_ij ≤ M·y_i` `∀i ∈ I` (|I| rows, M = |J| or c_i)
Both are valid. **The disaggregated form is strictly stronger**: its LP relaxation forbids `y_i = (Σ_j x_ij)/M` fractions like `y_i = 0.01` that the aggregated form allows, so it gives a tighter bound and much faster branch & bound. Write the disaggregated one unless the question asks for "one constraint per facility".

### 6. Big-M indicator — the two directions (lecture slide 17)

- **(A)** `x > K ⇒ b = 1` ⟹ `x ≤ K + M·b` ["99 + Mb ≥ x" for K = 99]
- **(B)** `b = 1 ⇒ x ≥ K+1` ⟹ `x ≥ (K+1)·b` ["x ≥ 100b"]
Direction (A) alone allows `b = 1` with small `x` (harmless if the objective penalises `b`). Direction (B) alone allows `x` large with `b = 0`. **State which direction the wording needs.**

### 7. iff-indicator (both directions) — the classic 4-point sub-question

**P = 1 iff Σ_j x_j > 0:**

- `P ≤ Σ_j x_j` (Σx = 0 ⇒ P = 0)
- `M·P ≥ Σ_j x_j` (Σx > 0 ⇒ P = 1)

**P = 1 iff Σ_j x_j > K (integer x):**

- `Σ_j x_j ≤ K + M·P`
- `Σ_j x_j ≥ (K+1)·P`
Real instances: `1000 R_t ≥ x_{R,g,t} + x_{R,b,t}` (rye grown in `t`); `300 P_t ≤ x_{P,g,t} + x_{P,b,t}` (≥300 ha potatoes); `Σ_i Σ_j A_i x_ij ≤ 200 + M y` **together with** `Σ_i Σ_j A_i x_ij ≥ 201 y`.
Note `Z = 1 ⟺ P ≤ 10` for **continuous** `P` is **impossible** — the feasible set is not closed, so it is not a polytope. Only strict/non-strict thresholds on *integer* quantities can be modelled exactly.

### 8. Either-or (disjunction) — at least one of two constraints holds

**One auxiliary binary:** `z ∈ {0,1}`

- `f₁(x) ≤ b₁ + M·z`
- `f₂(x) ≤ b₂ + M·(1 − z)`

**Two auxiliary binaries:**

- `f₁(x) − b₁ ≤ M z₁`, `f₂(x) − b₂ ≤ M z₂`, `z₁ + z₂ ≤ 1`
Concrete (sheet T5.1): "`x+y ≤ 3` **or** `2x+5y ≤ 12`" ⟹ `x + y − 3 ≤ Mz`, `2x + 5y − 12 ≤ M(1−z)`.
The two-binary form generalises to an OR over many constraints.

### 9. XOR (exactly one of the two holds)

- `z₁ + z₂ = 1` instead of `z₁ + z₂ ≤ 1`

with the constraint *and its negation* both reified (integer variables let you negate `x+y ≤ 3` as `x+y ≥ 4`):

- `x + y − 3 ≤ M(1 − z₁)` and `4 − x − y ≤ M z₁`
- `2x + 5y − 12 ≤ M(1 − z₂)` and `13 − 2x − 5y ≤ M z₂`
- `z₁ + z₂ = 1`
Single-binary XOR on a threshold: `Σx ≤ K + Mz` paired with `Σx ≥ (K+1)z`, mirrored with `(1−z)` on the other block.

### 10. min / max of two expressions

- **min{x₁, x₂} ≤ 1:** `x₁ ≤ 1 + M y₁`, `x₂ ≤ 1 + M(1 − y₁)`, `y₁ ∈ {0,1}`
(only one of the two must be ≤ 1). `max{x₁,x₂} ≤ 1` needs no binary: `x₁ ≤ 1` and `x₂ ≤ 1`.

### 11. Linearising products

- **Binary x binary:** `Y = x_k · x_l` ⟹ `Y ≥ x_k + x_l − 1`, `Y ≤ x_k`, `Y ≤ x_l`
- **Binary x cont./int.:** `w = z · a` ⟹ `w ≤ a`, `w ≤ M·z`, `w ≥ a − M(1 − z)`, `w ≥ 0`
In a **max** problem where `Y` is rewarded, `Y ≤ x_k, Y ≤ x_l` suffice; where it is penalised, `Y ≥ x_k + x_l − 1` suffices. Writing all three is always safe. Used in Endterm 2023 E2e (`Σ_b Y_{klb} ≤ 2`) and Endterm 2021 A3h (`a'₄ = z·a₄`, objective `+70 a'₄`).

### 12. Mutual exclusion by a distance / conflict parameter

**Direct (parameter known at modelling time):**

- `y_i + y_{i'} ≤ 1` `∀ i,i' ∈ I, i ≠ i', with d_{i,i'} < 5`

**Via an auxiliary "both open" binary** `z_{i,i'} ∈ {0,1}`:

- `1 + z_{i,i'} ≥ y_i + y_{i'}` `∀ i,i' ∈ I, i ≠ i'`
- `d_{i,i'} · z_{i,i'} ≥ 5 · z_{i,i'}`
The second form is the official CE solution and is safer when `d` is a symbolic parameter.

### 13. Ratio / percentage / fair share

- **"at least twice as many basics as specials":** `Σ_b x_kb ≥ 2 Σ_s x_ks`
- **"at least 50% of a_b":** `s_b ≥ 0.5 a_b`
- **"at least 2% safety margin above demand":** `Σ_i p_i x_it ≥ 1.02 · d_t`
- **"type w is at most 40% of everything sold":** `Σ_b w_b ≤ 0.4 · Σ_b (w_b+h_b+s_b+a_b)`, clear the fraction ⟹ `0.6 Σ_b w_b − 0.4 Σ_b (h_b+s_b+a_b) ≤ 0`
- **"min 50% crude 1 in fuel 1":** `0.5 x₂₁ − 0.5 x₁₁ ≤ 0`
Always move the total to the left and clear denominators so all coefficients are constants.

### 14. Rolling window ("at most 5 of any 7 consecutive days")

- `Σ_{t=k}^{k+6} x_t ≤ 5` `∀k ∈ {1,…,T−6}`

The `∀k` and the *explicit range of k* are where the points are. Same shape for "no three tasks of the same type in a row":

- `Σ_{s=r}^{r+2} Σ_{a∈A} x_as · t_a ≤ 2` `∀r ∈ {1,…,10}`
- `Σ_{s=r}^{r+2} Σ_{a∈A} x_as · (1 − t_a) ≤ 2` `∀r ∈ {1,…,10}`
(both are needed — one per value of the 0/1 type parameter).

### 15. History / consecutive-past conditions

**"grown on >50 ha in each of the two preceding years ⇒ may use good soil in t":**

- `950 A_{p,t} ≥ (x_{p,g,t} + x_{p,b,t}) − 50` `∀p ∈ P, t ∈ T`
- `x_{p,g,t} ≤ 1000 (1 − A_{p,t−1})` `∀p ∈ P, t ∈ {2,…,15}`
- `x_{p,g,t} ≤ 1000 (1 − A_{p,t−2})` `∀p ∈ P, t ∈ {3,…,15}`

**"3 consecutive years of rye ⇒ none in the following year":**

- `1000 R_t ≥ x_{R,g,t} + x_{R,b,t}` `∀t ∈ T`
- `x_{R,q,t} ≤ 1000 (3 − R_{t−3} − R_{t−2} − R_{t−1})` `∀q ∈ Q, t ∈ {4,…,15}`
Pattern: `k − Σ(k past indicators)` is 0 exactly when all `k` fired.

### 16. Start-up / changeover detection

- `y_t ≥ x_t − x_{t−1}` `∀t ≥ 2` (y_t = 1 if switched on at t)
- Optionally: `y_t ≤ x_t`, `y_t ≤ 1 − x_{t−1}`
- **Objective gains:** `+ Σ_t s_i y_it` (setup cost)
In a **min** problem with a positive setup cost, the first inequality alone is enough — the objective pushes `y` down. Say so; it earns the point.

### 17. Consecutive-stage adjacency (sequencing)

**y_{i,i'} = 1 if route i in stage j is followed by route i' in stage j+1:**

- `x_ij + x_{i',j+1} ≤ 1 + y_{i,i'}` `∀i,i' ∈ I, ∀j ∈ {1,…,|J|−1}`
- `d_{i,i'} · y_{i,i'} ≤ 200` `∀i,i' ∈ I`

**Alternative in one line:**

- `(x_ij + x_{i',j+1} − 1) · d_{i,i'} ≤ 200`

### 18. No gaps / monotone prefix ("no blank page in between")

- `Σ_{a∈A} x_{a,s} ≥ Σ_{a∈A} x_{a,s+1}` `∀s ∈ {1,…,|S|−1}`

### 19. Implication chains over several binaries

**"if W and R both hold, then P or C must hold":**

- `W_t + R_t − 1 ≤ P_t + C_t` `∀t ∈ T`

**Equivalently, as a quantity requirement:**

- `x_{C,g,t} + x_{C,b,t} ≥ 300 (W_t + R_t − 1 − P_t)` `∀t ∈ T`

**"exists easy task ⇒ exists hard task":**

- `M·y ≥ Σ_a x_as (3 − k_a)` `∀s`; `8 z_s ≤ Σ_a x_as k_a` `∀s`; `Σ_s z_s ≥ y`
General shape: `Σ(premise binaries) − (#premises − 1) ≤ Σ(conclusion binaries)`.

### 20. Piecewise-linear cost / volume discount

- **Split the variable:** `x = x_L + x_H`
- `x_L ≤ B · z` (B = breakpoint)
- `x_H ≤ M · z` (high tier only above the breakpoint)
- `x_L ≥ B · z`
- **Cost:** `c_L x_L + c_H x_H`

Exam variant (Dutch Petroleum, price 1.8 below 10 000 l and 1.6 above), modelled with a cost variable `c ≥ 0`:

- `10 000 Z ≤ k`
- `c ≥ 1.8 (k − M·Z)`
- `c ≥ 1.6 (k − M·(1 − Z))`
- **Objective:** `… − c`, `Z ∈ {0,1}`

### 21. Fixed charge

- `x ≤ M · y`, `y ∈ {0,1}`, `x ≥ 0`
- **Objective:** `min f·y + c·x` (f paid once if any x > 0)
- **Minimum lot size:** `x ≥ q·y` and `x ≤ M·y` ("produce 0 or at least q")

### 22. Multi-objective scalarisation (weighted sum)

**Two goals, e.g. min construction cost and max preference score:**

- `max w_P · (Σ_i Σ_j s_ij x_ij) − w_C · (Σ_i f_i y_i)` with `w_P, w_C > 0`
- Or equivalently: `min Σ_i f_i y_i − Σ_i Σ_j s_ij x_ij`
Flip the sign of whichever objective disagrees with the chosen sense, then weight. State that the weights are strictly positive and mention that the relative size encodes the trade-off ("complexity is twice as important" ⇒ weight 2 vs 1).

### 23. Goal programming with deviation variables

> **Not in `lecture-08-ip.pdf`.** It appears in `sheet-05-solutions.pdf` T5.3h. Kept because it is worth ~5 pts when it shows up; do not cite it as lecture material.

**Target form:**

- `Σ_t x_it − ℓ⁺_i + ℓ⁻_i = target` `∀i`, `ℓ⁺, ℓ⁻ ≥ 0`
- **Objective:** `min Σ_i (ℓ⁺_i + ℓ⁻_i)`

**Weighted (two targets, first twice as important):**

- `Σ x_as k_a + δᵏ = 60`, `Σ x_as d_a + δᵈ = 95`
- `Δᵏ ≥ δᵏ`, `Δᵏ ≥ −δᵏ`, `Δᵈ ≥ δᵈ`, `Δᵈ ≥ −δᵈ`
- `min 2Δᵏ + Δᵈ`

**Preemptive (lexicographic):**

- Solve for the primary objective first, get C*
- Then add `Σ_i,t (c_i x_it + s_i y_it) = C*` as a constraint
- Minimise the deviation objective

### 24. Standard problem templates worth memorising

**Generalized Assignment (GAP):**

- `min Σ_i Σ_j c_ij x_ij`
- `Σ_{i∈I} x_ij = 1` `∀j ∈ J`
- `Σ_{j∈J} d_ij x_ij ≤ s_i` `∀i ∈ I`
- `x_ij ∈ {0,1}` `∀i ∈ I, j ∈ J`

**0-1 Knapsack:** `max Σ_j p_j x_j` s.t. `Σ_j w_j x_j ≤ W`, `x ∈ {0,1}`

**Multiple Knapsack:** `max Σ_i Σ_j p_j x_ij` s.t. `Σ_j w_j x_ij ≤ W_i` `∀i`, `Σ_i x_ij ≤ 1` `∀j`

**Bin Packing:**

- `min Σ_i y_i`
- `Σ_{i∈I} x_ij = 1` `∀j ∈ J`
- `Σ_{j∈J} d_j x_ij ≤ s·y_i` `∀i ∈ I`
- `x_ij, y_i ∈ {0,1}`
- **Multidimensional (server consolidation):** `Σ_j u_jkt x_ij ≤ s_ik y_i` `∀i∈I, k∈K, t∈T`

**Set covering:** `min cᵀx`, `Ax ≥ 1`, `x` binary (a_ij = 1 if set j contains item i)

**Set partitioning:** `min cᵀx`, `Ax = 1`, `x` binary

**Set packing:** `max cᵀx`, `Ax ≤ 1`, `x` binary

**Combinatorial procurement auction:** `min Σ_j p_j x_j` s.t. `Σ_j w_ij x_j ≥ W_i` `∀i`, `x` binary
