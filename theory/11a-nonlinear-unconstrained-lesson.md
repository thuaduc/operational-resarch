# Nonlinear optimisation, unconstrained — from scratch

Teaching companion to [11-nonlinear-and-convex-kkt](11-nonlinear-and-convex-kkt.md), first half.
That file is the reference; **this one assumes you know nothing**.

Exam slot **E7a**. Nonlinear is worth **12–23 points** — the second-largest block on the paper.
The unconstrained half is the cheaper half and appears on **four of the five papers**.

Day 4's constrained half (convexity, Slater, KKT) is a separate file.

---

# Part 0 — What the exam actually asks

One sentence, near-verbatim every year:

> **SS24 P7a:** *"Find all local extrema of `f(x,y) = x² + y² + e^{2x} − (8x + 6y) + 2xy` and
> determine their type (max/min)."*
>
> **SS23 E7 (23 pts):** *"Let `α > 0` be a parameter. Determine all critical points of
> `f_α(x,y) = −¼x⁴ − ¼y⁴ + αxy` **as a function of α**, and show in each case whether it is a
> local maximum, local minimum, or a saddle point."*

Always the same three steps: **find where the gradient vanishes, compute the Hessian there,
classify.** No modelling, no proofs. It's a procedure.

The SS23 variant adds a parameter, so your answer is a *case analysis*, not a single point.
That's the harder and more recent shape.

---

# Part 1 — The one-variable idea you already know

For `f(x)` of a single variable:

```
f'(x) = 0     →  the tangent is flat  →  candidate for a max or min
f''(x) > 0    →  curves upward  →  minimum
f''(x) < 0    →  curves downward  →  maximum
f''(x) = 0    →  inconclusive (could be an inflection point)
```

Everything in this topic is that idea in `n` dimensions. The gradient replaces `f'`, the Hessian
replaces `f''`, and "positive" becomes "positive definite".

---

# Part 2 — The gradient

The **gradient** collects all the first partial derivatives into a vector:

```
∇f(x) = ( ∂f/∂x₁ , ∂f/∂x₂ , … , ∂f/∂xₙ )ᵀ
```

To compute `∂f/∂x`, differentiate with respect to `x` treating **every other variable as a
constant**. That's the only new mechanic.

For `f(x,y) = x² + y² + e^{2x} − 8x − 6y + 2xy`:

```
∂f/∂x :  2x + 2e^{2x} − 8 + 2y        (y² and −6y are constants → gone)
∂f/∂y :  2y − 6 + 2x                  (x², e^{2x}, −8x are constants → gone)

∇f(x,y) = ( 2x − 8 + 2y + 2e^{2x} ,  2y − 6 + 2x )ᵀ
```

**Geometrically** `∇f` points in the direction of steepest ascent. At a peak or a valley floor
there is no uphill direction to point in, so:

```
∇f(x) = 0      ←  the condition for a critical point
```

A point where `∇f = 0` is a **critical point** (or *stationary point*). Every local max and
local min is one — but so is a **saddle**, which is why Part 4 exists.

---

# Part 3 — Finding the critical points

Set `∇f = 0`. That's `n` equations in `n` unknowns, and **solving that system is usually the
hardest part of the question** — the classification afterwards is mechanical.

Standard tactic: solve one equation for one variable, substitute into the other.

**SS24's system:**
```
2x − 8 + 2y + 2e^{2x} = 0    ÷2  →   x + y + e^{2x} = 4
2y − 6 + 2x = 0              ÷2  →   x + y = 3      →   x = 3 − y
```
Substituting `x + y = 3` into the first: `3 + e^{2x} = 4`, so `e^{2x} = 1`, so `x = 0`.
Then `y = 3`. **One critical point: `(0, 3)`.**

**SS23's system** — harder, and worth studying:
```
∇f_α = ( −x³ + αy ,  −y³ + αx )ᵀ = 0
```
From the first component: `y = x³/α`. Substitute into the second:
```
−(x³/α)³ + αx = 0    ⟺    x·( −x⁸/α³ + α ) = 0
```
This factors — so **either** `x = 0`, **or** `x⁸ = α⁴`, i.e. `x² = α`, i.e. `x = ±√α`.

Three critical points:
```
(0, 0)          (√α, √α)          (−√α, −√α)
```

Two habits worth copying from that: **factor rather than expand** (`x·(…) = 0` gives you the
`x = 0` branch for free), and note that `α > 0` is what makes `√α` real — the problem statement
gave you that for a reason.

---

# Part 4 — The Hessian

The **Hessian** is the matrix of all second partial derivatives:

```
H_f(x) = ∇²f(x) = ( ∂²f / ∂xᵢ∂xⱼ )ᵢⱼ
```

For two variables:
```
        ⎡ f_xx   f_xy ⎤
H_f  =  ⎢             ⎥
        ⎣ f_yx   f_yy ⎦
```

`f_xy` means "differentiate by `x`, then by `y`". For any well-behaved (C²) function
`f_xy = f_yx`, so **the Hessian is always symmetric** — a useful check on your arithmetic.

Compute it by differentiating the gradient again:
```
SS24:  ∇f = ( 2x − 8 + 2y + 2e^{2x} ,  2y − 6 + 2x )

       ∂/∂x of the first  = 2 + 4e^{2x}        ∂/∂y of the first  = 2
       ∂/∂x of the second = 2                  ∂/∂y of the second = 2

              ⎡ 2 + 4e^{2x}    2 ⎤
       H_f =  ⎢                  ⎥            symmetric ✓
              ⎣      2         2 ⎦
```

```
SS23:  ∇f_α = ( −x³ + αy ,  −y³ + αx )

              ⎡ −3x²    α   ⎤
       H_f =  ⎢             ⎥
              ⎣   α   −3y²  ⎦
```

Note SS23's Hessian still contains `x` and `y` — you evaluate it **at each critical point
separately**, and different points can classify differently.

---

# Part 5 — Definiteness: what the Hessian tells you

The Hessian measures curvature *in every direction at once*. The classification:

| Hessian at `x*` | Curvature | `x*` is a |
|---|---|---|
| **positive definite** | curves up in every direction | **local minimum** |
| **negative definite** | curves down in every direction | **local maximum** |
| **indefinite** | up in some directions, down in others | **saddle point** |
| semidefinite (some zero) | flat in some direction | **inconclusive** |

A saddle is the case with no one-variable analogue: flat tangent plane, but you're at a minimum
along one axis and a maximum along another — like the middle of a horse's saddle.

Formally, for symmetric `A`:
```
positive definite      xᵀAx > 0 for all x ≠ 0   ⟺   all eigenvalues > 0
negative definite      xᵀAx < 0 for all x ≠ 0   ⟺   all eigenvalues < 0
indefinite                                       ⟺   eigenvalues of both signs
```

You never compute `xᵀAx`. You use one of the two tests below.

## Test A — the 2×2 shortcut (use this by default)

For `H = [[a, b], [b, d]]`, with `det = ad − b²` and `trace = a + d`:

```
det > 0  and  tr > 0   →  positive definite   →  MINIMUM
det > 0  and  tr < 0   →  negative definite   →  MAXIMUM
det < 0                →  indefinite          →  SADDLE
det = 0                →  inconclusive → go to Test C
```

**Why it works** — worth knowing so you never misremember it. For a 2×2 matrix:
```
λ₁ · λ₂ = det          λ₁ + λ₂ = trace
```
So `det > 0` means the two eigenvalues have the **same sign**, and then the trace tells you
*which* sign. `det < 0` means **opposite signs** — one direction curves up, one down — a saddle.
And `det = 0` means an eigenvalue is exactly zero, so there's a flat direction and the test
can't decide.

## Test B — leading principal minors (any size)

`D_k` = determinant of the top-left `k×k` submatrix. So `D₁ = a`, `D₂ = det H` for a 2×2.

```
all D_k > 0                        →  positive definite
signs alternate starting negative  →  negative definite
   i.e. D₁ < 0, D₂ > 0, D₃ < 0, …   (equivalently (−1)ᵏ D_k > 0)
no D_k = 0 but neither pattern     →  indefinite
any D_k = 0                        →  test INAPPLICABLE → Test C
```

The alternating pattern is the one people forget. Sanity check it on `H = −I`:
`D₁ = −1 < 0`, `D₂ = 1 > 0`. ✓

## Test C — eigenvalues (the fallback)

When a minor is zero, solve the characteristic equation:

```
det(H − λI) = 0
```

and read the signs of the roots. All positive → PD; all negative → ND; mixed → indefinite.

**You will need this.** SS23's origin has `H = [[0, α], [α, 0]]`, so `D₁ = 0` and Test B is
inapplicable. That's not an accident — the examiner built it in.

---

# Part 6 — The convexity shortcut

If the Hessian is positive definite **everywhere** (not just at the critical point), then `f` is
**strictly convex**, and that buys you a lot:

```
H_f ≻ 0 everywhere  ⟹  f strictly convex  ⟹  at most ONE minimum, and NO maxima at all
                                              any critical point is the GLOBAL minimum
```

This is exactly SS24's opening move:

> *"Since `Tr(∇²f) > 0` and `det(∇²f) > 0` it follows that `∇²f` is positive definite. Hence `f`
> is strictly convex. Consequently, if a minimum exists it is unique. No maxima exist."*

Check: `H = [[2 + 4e^{2x}, 2], [2, 2]]`. Trace `= 4 + 4e^{2x} > 0` always. Determinant
`= 2(2 + 4e^{2x}) − 4 = 8e^{2x} > 0` always. So PD for **every** `(x,y)` — no need to wait until
you've found the critical point.

Doing this check *first* is worth it: it tells you what you're looking for before you start
solving, and "no maxima exist" is itself part of the answer to "find all local extrema".

The reverse implication is false: `f(x) = x⁴` is strictly convex but `f''(0) = 0`. So
`H ≻ 0 ⟹ strictly convex`, not `⟺`.

---

# Part 7 — Worked example: SS24 P7a

> Find all local extrema of `f(x,y) = x² + y² + e^{2x} − (8x + 6y) + 2xy` and determine their type.

**1. Gradient.**
```
∇f = ( 2x − 8 + 2y + 2e^{2x} ,  2y − 6 + 2x )ᵀ
```

**2. Hessian, and check definiteness globally.**
```
       ⎡ 2 + 4e^{2x}   2 ⎤
H_f =  ⎢                 ⎥      tr = 4 + 4e^{2x} > 0
       ⎣      2        2 ⎦      det = 2(2 + 4e^{2x}) − 4 = 8e^{2x} > 0
```
Positive definite everywhere ⟹ `f` strictly convex ⟹ **no maxima**, and any critical point is
the unique global minimum.

**3. Solve `∇f = 0`.**
```
x + y + e^{2x} = 4
x + y = 3
```
Subtracting: `e^{2x} = 1` ⟹ `x = 0` ⟹ `y = 3`.

**4. Conclude.** `(x*, y*) = (0, 3)` is the **unique global minimum**. No local maxima exist. ∎

---

# Part 8 — Worked example: SS23 E7 (the parameterised one)

> `α > 0`. Determine all critical points of `f_α(x,y) = −¼x⁴ − ¼y⁴ + αxy` as a function of `α`,
> and classify each.

**1. Gradient and critical points.** As derived in Part 3:
```
∇f_α = ( −x³ + αy , −y³ + αx )ᵀ = 0    →    (0,0),  (√α, √α),  (−√α, −√α)
```

**2. Hessian.**
```
          ⎡ −3x²    α  ⎤
H_f  =    ⎢            ⎥
          ⎣   α   −3y² ⎦
```

**3. Classify `(±√α, ±√α)`.** Both give the same matrix, since only `x²` and `y²` appear:
```
        ⎡ −3α   α  ⎤
H_f  =  ⎢          ⎥        D₁ = −3α < 0        (α > 0)
        ⎣  α   −3α ⎦        D₂ = 9α² − α² = 8α² > 0
```
Signs alternate starting negative ⟹ **negative definite** ⟹ both are **local maxima**.

**4. Classify `(0,0)`.**
```
        ⎡ 0   α ⎤
H_f  =  ⎢       ⎥        D₁ = 0   →  minors INAPPLICABLE
        ⎣ α   0 ⎦
```
Fall back to eigenvalues:
```
det(H − λI) = λ² − α² = (λ − α)(λ + α) = 0    →    λ = +α,  λ = −α
```
`α > 0`, so one eigenvalue is positive and one negative ⟹ **indefinite** ⟹ **saddle point**.

**5. State the result.** For every `α > 0`: two local maxima at `(±√α, ±√α)` and a saddle at the
origin. No case split is needed here — the signs don't change anywhere on `α > 0`. *Say that
explicitly*; it's part of answering "as a function of α".

Note also the 2×2 shortcut agrees at step 3: `det = 8α² > 0`, `tr = −6α < 0` ⟹ negative
definite. Either test scores.

---

# Part 9 — Traps and drills

## Where points are lost

1. **Stopping at `∇f = 0`.** Critical points are candidates. Without the Hessian you haven't
   answered "determine their type".
2. **Forgetting the eigenvalue fallback.** A zero minor does **not** mean semidefinite — it means
   *the test doesn't apply*. SS23 puts a `D₁ = 0` case in deliberately.
3. **Misremembering the negative-definite pattern.** It alternates *starting negative*, and
   people write "all `D_k < 0`". Check against `−I`.
4. **Evaluating the Hessian at the wrong point**, or not at all — SS23's `H` depends on `x, y`.
5. **Losing a solution branch** when solving `∇f = 0`. Factor; don't divide by something that
   might be zero. `x·(…) = 0` has two branches.
6. **Ignoring the parameter's stated range.** `α > 0` is what makes `√α` real and fixes the
   signs of the minors.
7. **Not saying "no maxima exist"** when the function is globally convex. That's a scoring
   statement, not a remark.

## The four lines that answer any such question

```
1. ∇f = …                    and solve ∇f = 0 for ALL critical points
2. H_f = …                   symbolically
3. at each point: det/tr (or minors), fall back to eigenvalues if a minor is 0
4. conclude: min / max / saddle for each — and globally convex ⟹ global, unique
```

## Warm-up ladder (untimed)

1. `D10.2` *Unconstrained Optimization* — `[EXAM]` the same task as SS24 P7a. Do two or three
   functions until the gradient→Hessian→minors loop is automatic.
2. `T10.1` *Gradient Descent vs. Newton's Method* — `[CONCEPT]` read only. Never asked as a
   computation, but it feeds multiple choice (SS25 1h was on gradient-descent convergence).

Sheet 10 is `exercises/11-nonlinear-convex-optimization/sheet-10-exercises.pdf`; CE-10 is
`central exercises/11-nonlinear-convex-optimization/ce-10-demo.pdf`.

## Then the papers (timed, one minute per point)

- **SS24 P7a** — the convexity shortcut plus a clean substitution. Start here.
- **SS21 A6** *Konvexe Funktionen* (13 pts).
- **SS23 E7** (23 pts) — the parameterised one, with the eigenvalue fallback. Hardest; do it last.

## What carries into tomorrow

The Hessian test is also how you **prove convexity** in the constrained half: `H_f ⪰ 0` on a
convex domain ⟺ `f` convex. That's the bridge to Slater and KKT.
→ [11-nonlinear-and-convex-kkt](11-nonlinear-and-convex-kkt.md) Procedures 3–7
