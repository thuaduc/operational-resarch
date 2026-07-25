## Definitions

### Topology (ce-10 recap)

**Open set** `U ⊆ ℝⁿ` — every point has a small ball around it still inside `U`. **Closed** — complement is open; equivalently `U` contains all its limit points. **Bounded** — fits inside some ball of finite radius.

**Compact** — for the exam: use **Heine–Borel**.

```
In ℝⁿ:   C compact   ⟺   C closed AND bounded
```

**Weierstrass (extreme value theorem).** `C ≠ ∅` compact, `f : C → ℝ` continuous ⟹ `f` attains a **global minimum and a global maximum on `C`**. This is the *existence* step; without it "the optimum is …" is unjustified.

Typical compact feasible sets: a circle `x² + y² = r²`, a closed disk, a closed triangle/polytope, a closed box. Typical **non-compact**: `x + y ≤ 2` alone (unbounded), `x² + y² < 1` (open).

### Convex sets and functions

**Convex set.**

```
C convex  ⟺  ∀ x, y ∈ C, ∀ λ ∈ [0,1] :  λx + (1−λ)y ∈ C
```

Halfspaces `{x : aᵀx ≤ b}`, hyperplanes `{x : aᵀx = b}`, balls and any **intersection** of convex sets are convex. A union generally is not. A **circle** (the curve `x²+y²=r²`) is compact but **not** convex; the closed disk is both.

**Convex function** (lecture 14, slide 32) — on a convex domain:

```
f(λx₁ + (1−λ)x₂)  ≤  λ f(x₁) + (1−λ) f(x₂)      ∀ λ ∈ [0,1]
```

**Strictly convex**: `<` holds for all `x₁ ≠ x₂` and `λ ∈ (0,1)`. **Concave** / **strictly concave**: reverse the inequality (equivalently, `−f` is convex). `f(x)=x²` is strictly convex; `f(x)=|x|` is convex but **not** strictly.

**First-order characterisation** (differentiable, `dom f` convex):

```
f convex           ⟺  f(y) − f(x) ≥ ∇f(x)ᵀ(y − x)   ∀ x,y ∈ dom f
f strictly convex  ⟺  f(y) − f(x) > ∇f(x)ᵀ(y − x)   ∀ x ≠ y
```

i.e. the graph lies above every tangent hyperplane.

**Second-order characterisation** — the one you actually compute with:

```
H_f(x) ⪰ 0  ∀ x ∈ dom f  (dom f convex)  ⟺  f convex
H_f(x) ≻ 0  ∀ x ∈ dom f                  ⟹  f strictly convex   (⟸ is FALSE: x⁴)
H_f(x) ≺ 0  ∀ x                          ⟹  f strictly concave
```

### Gradient, Hessian, definiteness

```
∇f(x) = (∂f/∂x₁, …, ∂f/∂xₙ)ᵀ            (points in the direction of steepest ascent)
H_f(x) = ∇²f(x) = ( ∂²f / ∂xᵢ∂xⱼ )ᵢⱼ     (symmetric for C² functions)
```

For a symmetric `A`:

```
positive definite      xᵀAx > 0  ∀x ≠ 0     ⟺  all eigenvalues > 0
negative definite      xᵀAx < 0  ∀x ≠ 0     ⟺  all eigenvalues < 0
positive semidefinite  xᵀAx ≥ 0  ∀x         ⟺  all eigenvalues ≥ 0
indefinite             eigenvalues of both signs
```

**Leading principal minor of order `k`**, written `D_k`: determinant of the submatrix keeping exactly the first `k` rows **and** the first `k` columns.

### Constrained problem, Lagrangian

Standard **minimisation** form used throughout this chapter:

```
min  f(x)
s.t. g_i(x) ≤ 0     i = 1,…,m       (inequalities)
     h_j(x) = 0     j = 1,…,p       (equalities)
```

```
L(x, λ, μ) = f(x) + Σᵢ λᵢ g_i(x) + Σⱼ μⱼ h_j(x)
```

`λᵢ` — multipliers of inequalities, **sign-constrained** `λᵢ ≥ 0`. `μⱼ` — multipliers of equalities, **free in sign**.

**Active (tight/binding) constraint** at `x*`: `g_i(x*) = 0`. **Inactive (slack)**: `g_i(x*) < 0`.

**KKT point** — any `(x*, λ, μ)` satisfying all four KKT blocks below. **Regular point / constraint qualification (CQ)** — a technical condition (Slater, LICQ, all-affine constraints) under which KKT is meaningful.

---

## Procedures

### 1. Classify all critical points of an unconstrained `f`

1. Compute `∇f(x)` and solve `∇f(x) = 0` for all solutions (valid at interior points only).
2. Compute `H_f(x)` symbolically.
3. Evaluate `H_f` at each critical point and classify using the definiteness test (Procedure 2).
4. If `H_f ⪰ 0` everywhere, `f` is convex, so any stationary point is the global minimum.
5. If the domain is compact, check the boundary separately.

**Parameter-dependent version:** treat the parameter as a symbol throughout, get critical points as functions of it, substitute into `H_f`, classify per family, and state a case split if a minor changes sign.

### 2. Test definiteness — minors first, eigenvalues as fallback

1. Compute leading principal minors `D₁, D₂, …, D_n`:
   - all `D_k > 0` ⟹ **positive definite**
   - `(−1)^k D_k > 0` for all `k` ⟹ **negative definite**
   - no `D_k = 0` and neither pattern ⟹ **indefinite**
2. If any `D_k = 0`, minors are inapplicable — solve `det(H − λI) = 0` for eigenvalues instead.
3. **2x2 shortcut** for `H = [[a, b], [b, d]]`:
   ```
   det > 0 and tr > 0   ⟹ positive definite  (minimum)
   det > 0 and tr < 0   ⟹ negative definite  (maximum)
   det < 0              ⟹ indefinite         (saddle)
   det = 0              ⟹ inconclusive
   ```

### 3. Prove a function convex

- **(A) Hessian.** Show `dom f` is convex and `H_f ⪰ 0` everywhere. `H_f ≻ 0` ⟹ strictly convex.
- **(B) Composition rules.** Use preservation rules: `αf` (`α ≥ 0`), `f+g`, `f(Ax+b)`, `max{f₁,…,fₘ}`, nondecreasing convex composed with convex.
- **(C) Definition.** Apply the inequality directly when `f` is non-differentiable.
- **(D) Disproof.** Exhibit one triple `(x, y, λ)` violating the convexity inequality.

**Restricted-domain variant:** compute `H_f`, force minors positive, solve for the coordinate range, then pick `r` so the box stays inside that region.

### 4. Solve a KKT system with a tight / not-tight case split

1. Write the problem in standard form: all inequalities as `g_i(x) ≤ 0`, equalities as `h_j(x) = 0`.
2. Build the Lagrangian: `L = f + Σλᵢgᵢ + Σμⱼhⱼ` (min) or `L = f − Σλᵢgᵢ − Σμⱼhⱼ` (max).
3. Write all four KKT blocks: stationarity, primal feasibility, dual feasibility, complementary slackness.
4. Enumerate cases from complementary slackness: each inequality is either `λᵢ = 0` or `gᵢ = 0`.
5. In each case, solve the resulting system.
6. Discard if `λᵢ < 0` (dual infeasible) or a slack constraint is violated (primal infeasible).
7. Surviving cases are KKT points; under Slater the survivor is the global optimum, under LICQ only compare `f` values.

### 5. Equality-constrained optimisation on a compact set

1. Argue existence: feasible set is compact (Heine-Borel) and `f` is continuous, so extrema exist (Weierstrass).
2. Check the constraint qualification: if the constraint is nonlinear, Slater does not apply; verify LICQ (`∇g ≠ 0` on the feasible set).
3. Write the Lagrangian, solve the stationarity conditions and the constraint equation simultaneously.
4. Note that `λ` is unrestricted in sign for equality constraints.
5. Compare `f` at all candidates to identify the maximiser and minimiser (mandatory under LICQ).

### 6. Global-extrema strategy — the 4-step recipe

```
1. EXISTENCE   feasible set compact + f continuous ⟹ global min/max exist (Weierstrass).
2. CQ CHECK    Slater? (convex problem, strictly feasible point) or LICQ? (independent active gradients)
3. SOLVE       write KKT blocks, case-split on complementary slackness, discard infeasible cases.
4. COMPARE     evaluate f at every candidate; largest = max, smallest = min.
               (Skip only when Slater held — KKT already certifies globality.)
```

### 7. The decision flowchart

```
                          ┌─ NO ──▶ solve ∇f(x) = 0 ──▶ classify with H_f(x)  [see Procedure 1]
   Is the problem         │
   CONSTRAINED?  ─────────┤
                          │
                          └─ YES ─▶ (1) EXISTENCE CHECK
                                        feasible set compact + f continuous?
                                              │
                                              ▼
                                    (2) Is it a CONVEX problem?
                                        (f convex, feasible set convex)
                                    ┌─────────┴─────────┐
                                  YES                   NO
                                    │                    │
                                    ▼                    ▼
                          (3a) Does SLATER hold?   (3b) Does LICQ hold at x*?
                          f, g convex; h affine;   active ∇g_i ∪ all ∇h_j
                          ∃ strictly feasible x̄    linearly independent
                          ┌────────┴────────┐      ┌───────┴────────┐
                        YES                NO ────▶YES              NO
                          │                          │               │
                          ▼                          ▼               ▼
                  KKT system                  KKT system      other CQ (MFCQ, …)
                  ⟹ GLOBAL OPTIMUM            ⟹ CANDIDATES     or direct analysis
                  (done — no comparison)             │          (e.g. parametrise
                                                     ▼           the boundary)
                                            COMPARE OBJECTIVE VALUES
```

---

## Formula box

**Gradient / Hessian:**

- `∇f(x) = (∂f/∂x₁,…,∂f/∂xₙ)ᵀ`
- `H_f(x) = (∂²f/∂xᵢ∂xⱼ)ᵢⱼ`
- **Quadratic:** `f(x) = xᵀQx + bᵀx + c` implies `∇f = 2Qx + b`, `H_f = 2Q`
- `f` convex iff `Q ⪰ 0`

**2x2 Definiteness:** `H = [[a,b],[b,d]]`, `det = ad − b²`, `tr = a + d`

- `det > 0, tr > 0` -- positive definite (min)
- `det > 0, tr < 0` -- negative definite (max)
- `det < 0` -- indefinite (saddle)
- `det = 0` -- inconclusive

**Leading Principal Minors:**

- **Positive definite:** `D_k > 0` for all `k`
- **Negative definite:** `(−1)^k D_k > 0` for all `k` (i.e. `D₁ < 0, D₂ > 0, ...`)
- Some `D_k = 0` -- test inapplicable, use eigenvalues (`det(H − λI) = 0`)

**Convexity:**

- `f(λx+(1−λ)y) ≤ λf(x) + (1−λ)f(y)`
- `f(y) ≥ f(x) + ∇f(x)ᵀ(y−x)`
- `H_f ⪰ 0` on convex domain iff convex
- `H_f ≻ 0` implies strictly convex
- **Preserved by:** `αf` (`α ≥ 0`), `f+g`, `f(Ax+b)`, `max{f₁,…,f_m}`, `h∘g` (`h` convex non-decreasing)
- **Norm convexity:** `‖λx+(1−λ)y‖ ≤ λ‖x‖+(1−λ)‖y‖` (triangle + homogeneity)

**Lagrangian:**

- **Min:** `L = f + Σᵢ λᵢ gᵢ + Σⱼ μⱼ hⱼ`
- **Max:** `L = f − Σᵢ λᵢ gᵢ − Σⱼ μⱼ hⱼ`
- **Equality only:** `∇f + μ∇h = 0`, `h = 0`, regularity: `∇h(x*) ≠ 0`
- **Shadow price:** `λ* = ∂(optimal value)/∂b ≈ λ*` (small RHS change `b → b+Δ` shifts `f*` by `≈ λ*Δ`)

**KKT** (min `f` s.t. `g ≤ 0`, `h = 0`):

- **Stationarity:** `∇f(x*) + Σᵢ λᵢ∇gᵢ(x*) + Σⱼ μⱼ∇hⱼ(x*) = 0`
- **Primal feasibility:** `gᵢ(x*) ≤ 0`, `hⱼ(x*) = 0`
- **Dual feasibility:** `λᵢ ≥ 0` (`μⱼ` free in sign)
- **Complementary slackness:** `λᵢ · gᵢ(x*) = 0` for all `i`

**Slater:**

- `f`, `gᵢ` convex; `hⱼ` affine; there exists `x̄` with `gᵢ(x̄) < 0`, `hⱼ(x̄) = 0`
- Implies KKT iff global optimum, strong duality

**LICQ:**

- `{∇gᵢ(x*) : i active} ∪ {∇hⱼ(x*)}` linearly independent
- Implies local optimum implies KKT; KKT gives candidates only

**Topology:**

- **Heine-Borel** (in `ℝⁿ`): compact iff closed and bounded
- **Weierstrass:** compact `≠ ∅` + `f` continuous implies global min and max attained

**Algorithms:**

- **Gradient descent:** `x⁽ᵏ⁺¹⁾ = x⁽ᵏ⁾ − η ∇f(x⁽ᵏ⁾)`
- **Newton:** `x⁽ᵏ⁺¹⁾ = x⁽ᵏ⁾ − (H_f(x⁽ᵏ⁾))⁻¹ ∇f(x⁽ᵏ⁾)` [O(n³)/iter, quadratic conv.]
- **Momentum:** `z⁽ᵏ⁺¹⁾ = −η∇f(x⁽ᵏ⁾) + βz⁽ᵏ⁾`, `x⁽ᵏ⁺¹⁾ = x⁽ᵏ⁾ + z⁽ᵏ⁺¹⁾` (`β=0` gives plain GD)
- **Projected GD:** `x⁽ᵏ⁺¹⁾ = Π_X(x⁽ᵏ⁾ − η∇f(x⁽ᵏ⁾))`, where `Π_X(y) = argmin_{x∈X} ‖y − x‖₂`

---
