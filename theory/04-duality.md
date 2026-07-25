## Definitions

**Standard (symmetric) primal–dual pair.**

```
(P)  max  cᵀx              (D)  min  bᵀy
     s.t. Ax ≤ b                s.t. Aᵀy ≥ c
          x  ≥ 0                     y  ≥ 0

A ∈ ℝ^{m×n},  x ∈ ℝⁿ (n vars),  y ∈ ℝᵐ (m vars),  b ∈ ℝᵐ,  c ∈ ℝⁿ
```

| primal object | dual object |
|---|---|
| constraint `i` (m of them) | variable `yᵢ` |
| variable `xⱼ` (n of them) | constraint `j` |
| matrix `A` | `Aᵀ` |
| objective vector `c` | right-hand side `c` |
| right-hand side `b` | objective vector `b` |
| `max` | `min` |

Non-negativity/sign restrictions are **not** counted as constraints when counting dual variables.

**Symmetry property.** The dual of the dual is the primal. So "primal" and "dual" are labels, not properties.

**Normal vs. standard form pair** (lecture p. 15):

| | Primal | Dual |
|---|---|---|
| Normal form | `max cᵀx, Ax ≤ b, x ≥ 0` | `min bᵀy, Aᵀy ≥ c, y ≥ 0` |
| Standard form | `max cᵀx, Ax = b, x ≥ 0` | `min bᵀy, Aᵀy ≥ c, y free` |

**Binding / tight / slack.** Constraint `i` is *binding* at `x` if `Aᵢx = bᵢ`; *slack* (non-binding) if `Aᵢx < bᵢ` (for a `≤` row).

**Shadow price.** `yᵢ*` = marginal change of the optimal objective per unit increase of `bᵢ`. Also called Lagrange multiplier or dual variable.

---

## Procedures

### 1. Dualize any LP — 5 steps + SOB

**5-step recipe:**

1. Switch `max` ↔ `min`.
2. Introduce one dual variable per primal constraint (not counting sign restrictions).
3. Define one dual constraint per primal variable (not counting sign restrictions).
4. Transpose the coefficient matrix (`A → Aᵀ`): dual constraint `j` uses column `j` of `A`.
5. Swap objective coefficients and right-hand sides (`c ↔ b`).

**SOB table:** classify every variable and constraint as S / O / B, then translate S→S, O→O, B→B.

| | | **S (sensible)** | **O (odd)** | **B (bizarre)** |
|---|---|---|---|---|
| **Variable** | any problem type | `x ≥ 0` | `x` free | `x ≤ 0` |
| **Constraint** | in a **max** problem | `Ax ≤ b` | `Ax = b` | `Ax ≥ b` |
| **Constraint** | in a **min** problem | `Ax ≥ b` | `Ax = b` | `Ax ≤ b` |

| translate | primal **variable** type → dual **constraint** | primal **constraint** type → dual **variable** |
|---|---|---|
| **S** | sensible direction | `≥ 0` |
| **O** | `=` | free |
| **B** | bizarre direction | `≤ 0` |

Sensible direction in the dual: `≥` if dual is min, `≤` if dual is max. Bizarre is the opposite.

### 2. Certify or refute optimality of a given `x*` via CS

1. Check primal feasibility of `x*` in all constraints and sign restrictions; if violated, stop — not optimal.
2. For every slack primal constraint, set the corresponding `yᵢ = 0`.
3. For every `xⱼ* ≠ 0`, set dual constraint `j` to equality; substitute the zeros from step 2 and solve for the remaining `yᵢ`.
4. Check dual feasibility of the recovered `y` (remaining constraints and sign restrictions).
   - All satisfied: `x*` is optimal and `y` is the dual optimum.
   - Any violation or inconsistency: `x*` is not optimal.

If a dual point `y` is given instead, swap roles: check dual feasibility, slack dual constraints force `xⱼ = 0`, `yᵢ ≠ 0` forces primal row `i` tight, solve for `x`, check primal feasibility.

### 3. Bound / classify an LP without solving it

1. Find one feasible point of (P) — by weak duality, (D) is not unbounded.
2. Find one feasible point of (D) — so (D) is not infeasible.
3. Both feasible implies (D) has a finite optimum by strong duality.

To prove infeasibility of one problem: show the other is unbounded, or derive a contradiction from a non-negative combination of its constraints.

---

## Formula box

**Primal-Dual Pair**

- **(P):** `max cᵀx, Ax ≤ b, x ≥ 0`
- **(D):** `min bᵀy, Aᵀy ≥ c, y ≥ 0`

**Duality Theorems**

- **Weak duality:** `cᵀx' ≤ bᵀy'` for all feasible `x'`, `y'`
- **Strong duality:** `cᵀx* = bᵀy*` at optimality

**Complementary Slackness**

- **Vector form:** `(Aᵀy* − c)ᵀx* = 0` and `(Ax* − b)ᵀy* = 0`
- **Component form:** `((Aᵀy*)ⱼ − cⱼ) · xⱼ* = 0`, `((Ax*)ᵢ − bᵢ) · yᵢ* = 0`

**Optimal Dual**

- `yᵀ = c_Bᵀ B⁻¹` (= shadow prices / simplex Row 0 entries under the slacks)

**Unboundedness**

- P unbounded ⇒ D infeasible; D unbounded ⇒ P infeasible

**Farkas' Lemma**

- Exactly one of: `{Ax = b, x ≥ 0}`, `{Aᵀy ≤ 0, bᵀy > 0}`

---
