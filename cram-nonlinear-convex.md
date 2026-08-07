# Nonlinear & Convex Optimization — the cram sheet

Everything you need for **Exercise 7**, in the order you should learn it.
You already have the unconstrained half, so Part 1 is a refresher and **Parts 2–9 are the real
work**. The exercise plan is at the bottom.

---

## Part 0 · What the exam actually asks

Exercise 7 has two flavours. Some years you get one, some years both.

```
7a  UNCONSTRAINED    "Find all local extrema of f(x,y) = … and determine their type."
                     → gradient, Hessian, classify. Pure procedure.

7b  CONSTRAINED      A story → a convex program → convexity proof → Slater → KKT.
                     → definitions and verification, very little arithmetic.
```

**Where the weight has gone recently:**

| Paper | Points | Split |
|---|---|---|
| SS25 E7 | 20 | **100% constrained** — no gradient/Hessian at all |
| SS24 P7 | 20 | split across both halves |
| SS23 E7 | 23 | 100% unconstrained, with a parameter |
| SS21 A6 | 13 | convex functions |

So the constrained half is where to spend your time. It's also the half that's mostly
**definitions you can memorise**, which is exactly what you want a few hours out.

---

# PART 1 · Unconstrained

You said you can already do this, so here it is compressed. Skip to Part 2 if the four lines
below feel automatic.

### The whole idea in one translation

One variable you already know:

```
f'(x) = 0    → flat tangent → candidate
f''(x) > 0   → curves up    → minimum
f''(x) < 0   → curves down  → maximum
```

In `n` dimensions the **gradient** replaces `f'`, the **Hessian** replaces `f''`, and "positive"
becomes "positive definite". That's the entire translation.

```
∇f(x) = (∂f/∂x₁, …, ∂f/∂xₙ)ᵀ          H_f = (∂²f/∂xᵢ∂xⱼ)    — always symmetric
```

`∇f` points uphill. At a peak or a valley floor there is no uphill direction, so `∇f = 0` is the
condition for a **critical point**. Saddles satisfy it too — that's why you need the Hessian.

### The four lines that answer any 7a question

```
1. ∇f = …        and solve ∇f = 0 for ALL critical points
2. H_f = …       symbolically (it usually still contains x and y)
3. at each point separately: det/trace, or minors, or eigenvalues
4. conclude min / max / saddle for each
```

### Test A — the 2×2 shortcut (your default)

For `H = [[a,b],[b,d]]`, `det = ad − b²`, `tr = a + d`:

| `det` | `tr`  | Verdict                         |
| ----- | ----- | ------------------------------- |
| `> 0` | `> 0` | positive definite → **minimum** |
| `> 0` | `< 0` | negative definite → **maximum** |
| `< 0` | —     | indefinite → **saddle**         |

### The shortcut 

If `H_f ≻ 0` **everywhere** — not just at the critical point — then `f` is strictly convex, and:

```
⟹  no maxima exist at all
⟹  any critical point is the unique GLOBAL minimum
```


---

# PART 2 · Constrained

This is the first place marks get lost, because the two ideas share a name and you usually need
both in the same question.

### A convex SET — about the region

```
C is convex  ⟺  λx + (1−λ)y ∈ C      for all x, y ∈ C and all λ ∈ [0,1]
```

**In words:** take any two points in the set; the entire straight line between them stays inside.
No dents, no holes, no gaps.

### A convex FUNCTION — about the graph

```
f is convex  ⟺  f(λx + (1−λ)y)  ≤  λf(x) + (1−λ)f(y)      for all λ ∈ [0,1]
```

**In words:** the chord joining any two points on the graph lies **above** the graph. The left side is the function at an in-between point; the right side is the straight-line interpolation.

- **Strictly convex** — `<` instead of `≤`, for `x ≠ y`. `x²` is strictly convex; `|x|` is convex but *not* strictly, because it's flat-linear on each side.
- **Concave** — flip the inequality. Equivalently, `−f` is convex.
- **Affine** `aᵀx + b` — both convex and concave, because its chord is its graph.

### The classic trap

| Object | Compact? | Convex? |
|---|---|---|
| circle **curve** `x² + y² = r²` | ✓ | **✗** — the chord cuts through the middle, which isn't on the curve |
| closed **disk** `x² + y² ≤ r²` | ✓ | ✓ |
| halfplane `x + y ≤ 2` | ✗ (unbounded) | ✓ |
| **intersection** of convex sets | | ✓ always |
| **union** of convex sets | | ✗ generally |

### Why anyone cares

```
convex objective  +  convex feasible set   ⟹   every LOCAL minimum is GLOBAL
```

That is the entire payoff of the topic. For a general nonlinear problem you find candidates and hope. For a convex one, finding a local solution **finishes** the problem. Everything in Parts 3–9 is machinery for cashing that in.

---

# PART 3 · Proving a function is convex

Two routes, and the exams use them **back to back in the same question** — SS25 E7b proves the
norm from the definition, then E7c uses that result to knock out every constraint in one line.

### Route A — straight from the definition

Use when `f` is abstract or non-differentiable — a norm, for instance. **This is SS25 E7b, and
it's the part people skip.** Memorise these three lines; they are free points.

> **Claim.** Any norm `‖·‖` on `ℝᵈ` is convex.
>  **Proof.** Let `x, y ∈ ℝᵈ` and `λ ∈ [0,1]`.
> ```
> ‖λx + (1−λ)y‖  ≤  ‖λx‖ + ‖(1−λ)y‖        triangle inequality
>                =  |λ|·‖x‖ + |1−λ|·‖y‖     homogeneity
>                =  λ‖x‖ + (1−λ)‖y‖         because λ ∈ [0,1], both are ≥ 0
> ```
> which is exactly the definition of convexity. ∎

That last step is doing real work: you must **say why** the absolute values come off, namely that
`λ ≥ 0` and `1 − λ ≥ 0` because `λ ∈ [0,1]`. Omitting that sentence is the usual mark lost.

### Route B — composition rules

Once you have a few convex building blocks, these rules let you assemble more without any new
proof:

```
α·f              for α ≥ 0
f + g            sum of convex functions
f(Ax + b)        affine substitution into a convex function
max{f₁,…,fₘ}     pointwise maximum
```

Plus the fact you'll lean on constantly: **every affine function `aᵀx + b` is convex** (it's also
concave — its chord *is* its graph).

This is how SS25 E7c disposes of every constraint in one line:

```
gᵢ(r, c₁, c₂)  =  ‖M − pᵢ‖₂  −  r
                   └────┬───┘   └┬┘
                        │        └─  affine, hence convex
                        └─  a norm of an affine function of M
                            → convex by Route A + the affine rule

sum of two convex functions  ⟹  gᵢ is convex  ✓
```

> The exam is built so that **(b) feeds (c)**. When part (c) asks you to show the constraints are
> convex, the expected answer is to *cite the result you just proved*, not to start over.

### The third route — the Hessian, when `f` is differentiable

Same machinery as Part 1, but note the wording changes: for **classifying a point** you check the
Hessian *at that point*; for **convexity** you must check it **everywhere on the domain**, and the
domain must itself be convex.

| Hessian, everywhere on a convex domain | Conclusion | Reverse direction? |
|---|---|---|
| `H_f ⪰ 0` — positive **semi**definite | `f` is **convex** | ✓ **⟺** — this one is an iff |
| `H_f ≻ 0` — positive **definite** | `f` is **strictly convex** | ✗ **⟹ only** |

**The difference is `≥ 0` versus `> 0` on the eigenvalues** (equivalently: minors allowed to be
zero, or required to be strictly positive):

```
⪰ 0   semidefinite   all λ ≥ 0    flat directions ALLOWED   →  convex
≻ 0   definite       all λ > 0    no flat directions        →  strictly convex
```

**Why one is an iff and the other isn't.** Convexity permits flat stretches — a straight line is
convex — so "curvature never negative" is exactly convexity, both ways. Strict convexity forbids
flat stretches, but a function can bend strictly while its second derivative *touches* zero at an
isolated point:

```
f(x) = x⁴     strictly convex    but  f''(0) = 0    →  H is NOT positive definite there
```

So `x⁴` is the counterexample that kills the converse. Remember it — it's a natural
multiple-choice item.

```
H ≻ 0 everywhere   ⟹   strictly convex        TRUE
strictly convex    ⟹   H ≻ 0 everywhere       FALSE   (x⁴)
H ⪰ 0 everywhere   ⟺   convex                 TRUE both ways
```

**What each one buys you.** This is why the exam cares:

```
convex           ⟹  every local min is a GLOBAL min      (there may be several, e.g. a flat valley)
strictly convex  ⟹  at most ONE minimiser, and NO maxima  ⟹  a critical point is THE global min
```

SS24 P7a runs the second line: trace and determinant are both positive **for every `(x,y)`**, so
`H ≻ 0` everywhere, so `f` is strictly convex, so *no maxima exist* and the one critical point it
finds is the unique global minimum. Note it checks the whole plane, not just the critical point —
that's what licenses the global claim.

For concavity, mirror everything: `H ⪯ 0` ⟺ concave, `H ≺ 0` ⟹ strictly concave.

---

# PART 4 · Compactness — the existence step

Cheap, mechanical, and frequently forgotten. It's step 1 of the recipe in Part 9.

```
CLOSED    contains its own boundary        [0,1] yes   ·  (0,1) no
BOUNDED   fits inside a finite box         [0,1] yes   ·  [0,∞) no
COMPACT   closed AND bounded               (Heine–Borel, in ℝⁿ)
```

Each condition blocks a different failure:

- On `(0,1)`, `max x` climbs toward 1 and never reaches it — **not closed**.
- On `[0,∞)`, `max x` runs away to infinity — **not bounded**.
- On `[0,1]` it's attained at `x = 1`. ✓

> **Weierstrass.** A non-empty **compact** set plus a **continuous** `f` ⟹ a global minimum
> **and** a global maximum are **attained**.

Without this you have no right to say "the optimum is …". One sentence, real marks.

Careful: "not open" ≠ "closed". `[0,1)` is neither; `ℝⁿ` is both.

---

# PART 5 · Standard form — convert first, always

Every method below assumes this exact shape:

```
min  f(x)
s.t. gᵢ(x) ≤ 0        i = 1,…,m       inequalities, all written "≤ 0"
     hⱼ(x) = 0        j = 1,…,p       equalities
```

Two conversions you will need:

```
g(x) ≥ 0     becomes     −g(x) ≤ 0
max f        becomes     min −f
```

A sign error here propagates through the entire question. **Write the conversion down explicitly**
rather than doing it in your head.

Then, at a given point `x*`:

```
ACTIVE   (tight, binding)   gᵢ(x*) = 0     the constraint is pressing on you
INACTIVE (slack)            gᵢ(x*) < 0     there's room to spare
```

Same vocabulary as LP duality, doing the same job.

---

# PART 6 · The Lagrangian

The Lagrangian folds every constraint into the objective, each weighted by a **multiplier**:

```
L(x, λ, μ)  =  f(x)  +  Σᵢ λᵢ gᵢ(x)  +  Σⱼ μⱼ hⱼ(x)          (MIN problem)

λᵢ   inequality multipliers   —   λᵢ ≥ 0     SIGN-CONSTRAINED
μⱼ   equality multipliers     —   free in sign
```

**Why `λ ≥ 0` but `μ` free.** An inequality has a direction. Violating `gᵢ ≤ 0` means `gᵢ > 0`,
and the penalty term `λᵢgᵢ` has to **increase** the objective you're minimising — otherwise
breaking the rule would be rewarded. So `λᵢ` cannot be negative. An equality can be violated in
either direction, so its multiplier needs both signs available.

This is the **SOB pattern from LP duality** all over again: `≤` rows get sign-restricted duals,
`=` rows get free ones. And `λ` **is a shadow price** — `λ* ≈ ∂(optimal value)/∂b`, the same
reading as `yᵢ` in sensitivity.

For a **max** problem the convention flips to `L = f − Σλᵢgᵢ − Σμⱼhⱼ`. Simpler advice: convert to
a min and never think about it again.

---

# PART 7 · The four KKT blocks

The exam asks you to "state the KKT conditions" and scores each block. Memorise them as four
labelled items.

| | Condition |
|---|---|
| **1. Stationarity** | `∇f(x*) + Σᵢ λᵢ∇gᵢ(x*) + Σⱼ μⱼ∇hⱼ(x*) = 0` — equivalently `∇ₓL = 0` |
| **2. Primal feasibility** | `gᵢ(x*) ≤ 0` for all `i`, and `hⱼ(x*) = 0` for all `j` |
| **3. Dual feasibility** | `λᵢ ≥ 0` for all `i` (`μⱼ` free) |
| **4. Complementary slackness** | `λᵢ · gᵢ(x*) = 0` for all `i` |

### Reading them

**Stationarity** generalises `∇f = 0`. Unconstrained, the gradient has to vanish. Constrained, it
doesn't — it only has to be **balanced** by the constraint gradients pushing back. You're allowed
to be standing on a slope, provided a wall is holding you there.

**Complementary slackness** is the same statement as in LP duality:

```
λᵢ · gᵢ(x*) = 0    ⟺    for each i:   λᵢ = 0   OR   gᵢ(x*) = 0
```

> **An inactive constraint has multiplier zero. A non-zero multiplier means the constraint is
> active.** Economically: a resource you aren't fully using has zero shadow price.

---

# PART 8 · Solving a KKT system — the case split

Complementary slackness is what turns KKT from a description into something you can actually
solve. Each inequality gives you a fork:

```
for each i:    either  λᵢ = 0        (constraint inactive)
               or      gᵢ(x*) = 0    (constraint active)
```

With `m` inequalities that's up to `2ᵐ` cases — but most die immediately.

### The procedure

```
1. Convert to standard form.
2. Write the Lagrangian.
3. Write all four blocks.
4. Enumerate the cases from complementary slackness.
5. Solve each case's system.
6. DISCARD a case, AND SAY WHY:
        λᵢ < 0        →  violates dual feasibility
        gᵢ(x*) > 0    →  violates primal feasibility
7. Whatever survives is a KKT point.
```

Step 6 is where most branches die, and **stating the reason for each dead branch is worth marks**.
Don't just cross them out.

### Kill cases before you enumerate

The best move is to argue a constraint must be active or inactive *from the problem statement*,
before doing any algebra. SS25's part (f) does exactly this:

> Since at least two distribution centres are distinct, the enclosing circle cannot have radius
> zero, so `r > 0`. Complementary slackness gives `μr = 0`, hence **`μ = 0`**.

One sentence removes an entire branch. The problem's hints usually point straight at this.

---

# PART 9 · Slater vs LICQ — this decides your conclusion

KKT only means something under a **constraint qualification**. Which one holds determines how
strong a statement you're allowed to make, and this is the single most examinable distinction in
the topic.

### Slater's condition

```
Requires:  f and all gᵢ CONVEX, all hⱼ AFFINE,
           and a point x̄ with  gᵢ(x̄) < 0  STRICTLY, for every i
```

```
Slater holds  ⟹  a KKT point IS a GLOBAL optimum.
                  You are finished. No comparison needed.
```

**How to verify it:** exhibit the point. That's the whole proof. SS25 E7d builds one by taking the
**centroid** of the given points and inflating the radius by `+1`:

```
c̄ = centroid ,   r̄ = maxᵢ‖c̄ − pᵢ‖₂ + 1     ⟹    gᵢ = ‖c̄ − pᵢ‖₂ − r̄ ≤ −1 < 0   ✓
```

The `+1` is the trick — it manufactures the *strictness* that Slater demands.

### LICQ (linear independence constraint qualification)

```
Requires:  { ∇gᵢ(x*) : i ACTIVE }  ∪  { ∇hⱼ(x*) }   linearly independent
```

```
LICQ holds  ⟹  KKT gives CANDIDATES only.
                You must evaluate f at each one and COMPARE.
```

### The distinction, in one table

| | Slater | LICQ |
|---|---|---|
| Needs convexity? | **yes** | no |
| Checked where? | globally, at one strictly feasible point | at the candidate point `x*` |
| What you conclude | KKT point is a **global optimum** | KKT points are **candidates** |
| Must compare `f` values? | **no** | **yes** |

> **The multiple-choice trap** (it was SS25 1h):
> *"a KKT point is always a local optimum"* — **FALSE**.
> *"for a convex problem, a KKT point is a global minimiser if Slater holds"* — **TRUE**.

---

# PART 10 · The four-step recipe

For any constrained problem, in this order:

```
1. EXISTENCE   Feasible set compact (closed AND bounded) + f continuous
               ⟹ a global min and max EXIST, by Weierstrass.
               Cheap. Frequently forgotten. Do it first.

2. CQ CHECK    Convex problem with a strictly feasible point?  →  SLATER
               Otherwise check LICQ at the candidate.

3. SOLVE       Lagrangian → four blocks → case split on complementary slackness
               → discard branches, with a stated reason for each.

4. COMPARE     Evaluate f at every survivor. Largest = max, smallest = min.
               SKIP this step ONLY if Slater held — KKT already certified globality.
```

---

# PART 11 · Worked in full — SS25 E7, "Minimal Circle Enclosure"

This is the model question. If you only do one thing, do this end to end.

> `n` distribution centres sit at points `pᵢ = (xᵢ, yᵢ) ∈ ℝ²`, at least two of them distinct.
> Find the centre `M = (c₁, c₂)` and radius `r ≥ 0` of the **smallest circle enclosing all of
> them**.

### (a) Define a convex function

> `f : ℝⁿ → ℝ` is convex if for all `x, y ∈ ℝⁿ` and all `λ ∈ [0,1]`,
> `f(λx + (1−λ)y) ≤ λf(x) + (1−λ)f(y)`.

### (b) Prove any norm is convex

The three lines from Part 3, Route A. Include the "`λ ∈ [0,1]` so both are `≥ 0`" justification.

### (c) Formulate it, and show convexity

Minimise the radius, subject to every point being inside the circle:

```
min_{r, c₁, c₂}   r
        s.t.      ‖M − pᵢ‖₂ − r ≤ 0     ∀i
                  r ≥ 0 ,  c₁, c₂ ∈ ℝ
```

- `f(r, c₁, c₂) = r` is **linear, hence convex**.
- `gᵢ = ‖M − pᵢ‖₂ − r` is, **by part (b)**, a sum of two convex functions, hence convex.

Notice how (b) is *used* by (c). The parts are built to chain — that's a hint about what the
examiner wants you to cite.

### (d) Show Slater holds

Take the centroid, inflate the radius:

```
c̄₁ = (1/n)Σxᵢ ,   c̄₂ = (1/n)Σyᵢ ,   M̄ = (c̄₁, c̄₂) ,   r̄ = maxᵢ‖M̄ − pᵢ‖₂ + 1
```

Then `gᵢ(r̄, c̄) = ‖M̄ − pᵢ‖₂ − r̄ ≤ −1 < 0` for every `i`. Strictly feasible ⟹ **Slater holds**,
so any KKT point is a global optimum.

### (e) State the KKT conditions

With `L = f + Σλᵢgᵢ − μr` (the `−μr` handles `r ≥ 0`, i.e. `−r ≤ 0`, with `μ ≥ 0`):

```
Stationarity:             ∇L = 0
Primal feasibility:       gᵢ ≤ 0 ,   −r ≤ 0
Dual feasibility:         λᵢ ≥ 0 ,   μ ≥ 0
Complementary slackness:  λᵢgᵢ = 0 ,  μr = 0
```

### (f) Derive `Σλᵢ = 1` and `c* = Σλᵢpᵢ`

**Kill `μ` first.** At least two points are distinct, so the enclosing circle has positive radius:
`r > 0`. Complementary slackness gives `μr = 0`, hence **`μ = 0`**.

**Stationarity in `r`:**
```
∂L/∂r  =  1 − Σᵢλᵢ − μ  =  0        with μ = 0   ⟹   Σᵢλᵢ = 1   ✓
```

**Stationarity in `c₁`:**
```
∂L/∂c₁  =  Σᵢ λᵢ · (c₁ − xᵢ)/‖pᵢ − M‖₂  =  0
```
Use complementary slackness again: for each `i`, either `λᵢ = 0` (that term vanishes) or `gᵢ = 0`,
which means `‖pᵢ − M‖₂ = r`. So every surviving denominator equals `r`:
```
Σᵢ λᵢ(c₁ − xᵢ)/r = 0    ⟹    Σᵢ λᵢ(c₁ − xᵢ) = 0    ⟹    c₁·Σλᵢ = Σλᵢxᵢ
```
and since `Σλᵢ = 1`, **`c₁* = Σλᵢxᵢ`**. Identical for `c₂`. ∎

Since `λᵢ ≥ 0` and they sum to 1, this says the optimal centre is a **convex combination** of the
distribution centres.

### (g) What does `λᵢ > 0` imply about `pᵢ`?

By complementary slackness, `λᵢ > 0` forces `gᵢ = 0`, i.e. `‖pᵢ − M‖₂ = r`. So **`pᵢ` lies exactly
ON the boundary circle** — it's one of the points the circle is actually touching.

---

# PART 12 · Where the marks are lost

1. **Not converting to standard form**, then carrying a sign error through the whole question.
2. **`λ` free and `μ` sign-restricted** — it's the other way round.
3. **Skipping the definition or the convexity proof** because they don't feel like "real work".
   They were 4–6 points on SS25.
4. **Forgetting to justify `|λ| = λ`** in the norm proof.
5. **Confusing convex sets with convex functions.** The circle *curve* isn't convex; the disk is.
6. **Claiming Slater without exhibiting a point.** The point *is* the proof.
7. **Comparing objective values after Slater held** (unnecessary), or **not comparing after LICQ**
   (required).
8. **Discarding cases silently.** Say why each branch died.
9. **Skipping the existence step** before asserting an optimum exists.
10. *(7a)* Treating a zero minor as semidefinite instead of falling back to eigenvalues.

### The two statements to have absolutely cold

```
Slater  ⟹  KKT point is a GLOBAL optimum     (needs convexity; you're done, no comparison)
LICQ    ⟹  KKT points are CANDIDATES only    (no convexity needed; you must compare)
```

---

# PART 13 · Which exercises to do

Files live in:

```
exercises/11-nonlinear-convex-optimization/          →  sheet-10, sheet-11  (the T-numbers)
central exercises/11-nonlinear-convex-optimization/  →  ce-10, ce-11        (the D-numbers)
exams/endterm/                                       →  the past papers
```

### Everything that exists, and what it's actually about

**Demo exercises (CE-10)** — `central exercises/…/ce-10-demo.pdf`

| | Title | What it actually asks | Verdict |
|---|---|---|---|
| `D10.1` | Topological Properties and Convex Sets | `X₁ = {‖x‖₂ < 1}`, `X₂ = {x₁+x₂ ≤ 1, x ≥ 0}`. (a) open / closed / bounded / compact for each, justified. (b) prove `X₂` is convex. | **DO** — this is Part 4 and the convex-set definition, exactly as examined |
| `D10.2` | Unconstrained Optimization | `f = x²y + y³ − 3y`. Gradient, Hessian, all stationary points, classify **by explicitly computing eigenvalues**. | Optional — the 7a half you already have |
| `D10.3` | KKT-conditions | (a) state KKT (b) how it changes for **max** (c) prove `f` convex via composition (d) show **Slater is VIOLATED** (e) gradients (f) show `∇h ≠ 0` ⟹ all points **regular** (LICQ) (g) describe how to find the global min/max from four given KKT points | **THE ONE TO DO** — see below |

**Demo exercises (CE-11)** — `ce-11-demo.pdf`: `D11.1` Mirror Descent, `D11.2` Projected Gradient
Descent, `D11.3` FTRL. **All online optimization. Skip entirely — never examined.**

**Training sheet 10** — `exercises/…/sheet-10-exercises.pdf`

| | Title | What it actually asks | Verdict |
|---|---|---|---|
| `T10.1` | Gradient Descent vs. Newton's Method | One iteration of each from `x⁽⁰⁾ = (1,1)`, then discuss. | **Read only** — never a written computation, but it feeds a multiple-choice item |
| `T10.2` | Lagrange multipliers | `f = −e^{x²+y²}`, `s.t. 2x + y = 5`. Show `f` concave, build `L`, find critical points, classify. **Equality constraint only.** | Warm-up if the Lagrangian feels shaky |
| `T10.3` | Non-linear optimization – KKT | `max sin x₁ + cos x₂` with four constraints. (a) state KKT, and how min differs from max (b) show `(1,1)` is **not** a local maximiser (c) set `μ = 0`, derive the point, check it, then ask **global? unique?** | **DO** — second rep, and (c) is the case-split move |

**Training sheet 11** — `sheet-11-exercises.pdf`

| | Title | What it actually asks | Verdict |
|---|---|---|---|
| `T11.1` | Convex Functions – Examples | Decide convexity for `x⁴ − x`, `\|x\| + cos x`, `max{f,g}`, and a piecewise `‖x‖` / `‖x‖²`. | **DO** — cheap, and drills the composition rules from Part 3 |
| `T11.2` | Convex functions | `f = e^{2x} + xy + y²`. Find an `r > 0` such that `f` is convex on the square `Qr`, and justify. | **DO** — this is the Hessian route **on a restricted convex domain**, the exact point Part 3 makes |
| `T11.3` | The Hedge Algorithm and the Learning Rate | — | **Skip.** Online optimization. |

> **Note.** No sheet exercise asks for the **norm convexity proof** (Route A). That appears only in
> **SS25 E7b** itself. So drill it straight from Part 3 of this file — there's no practice problem
> to fall back on.

### The order to do them in

| # | Exercise | Minutes | Why this one |
|---|---|---|---|
| **1** | **`D10.3`** | 40 | The single closest thing to a real Exercise 7. It walks the entire arc: state KKT → convexity by composition → **Slater fails** → fall back to regularity/LICQ → **compare `f` at the KKT points**. That's precisely the Slater-vs-LICQ split from Part 9, and it's the branch people get wrong. |
| **2** | **`T11.2`** | 15 | Hessian + convex domain. Short, and it's why Part 3 keeps saying "say the domain is convex". |
| **3** | **SS25 E7** | 40 | The model question. All seven parts, timed, closed-book. |
| **4** | **`T10.3`** | 25 | Second KKT rep, with an inequality-constrained max and the `μ = 0` case split. |
| **5** | **`T11.1`** | 10 | Fast pattern drill on the composition rules. |
| **6** | **`D10.1`** | 15 | Compactness and the convex-set proof — Part 4, and cheap marks. |

### If time remains

```
SS24 P7b       KKT with one linear constraint, Slater verified. Short confidence check.
T10.2          Lagrangian with an equality constraint only.
D10.2          Unconstrained with explicit eigenvalues — only if 7a feels rusty.
```

### Skip entirely

```
T11.3   The Hedge Algorithm            ┐
D11.1   Mirror Descent                 │  online optimization
D11.2   Projected Gradient Descent     │  never examined
D11.3   FTRL                           ┘
T10.1   Gradient Descent vs Newton     — read the discussion, don't compute
```

### A three-hour plan

```
0:00 – 0:25   Read Parts 5, 6, 7 of this file. Write the four KKT blocks from
              memory until you can do it without looking.
0:25 – 1:05   D10.3 — the full arc. THE rep that matters.
1:05 – 1:20   T11.2 — Hessian on a restricted domain.
1:20 – 1:35   Re-read Parts 8 and 9. Say out loud: Slater ⟹ global, LICQ ⟹ compare.
1:35 – 2:15   SS25 E7, timed, closed-book, all parts.
2:15 – 2:35   Check it. Fix whatever broke. Re-read Part 12.
2:35 – 3:00   T10.3 part (c), plus T11.1 as a fast cooldown.
```

Drill the **norm proof** (Part 3, Route A) in any spare five minutes — it's memorisable and there
is no exercise for it.

### The 20-minute hedge for the topic you're dropping

Don't walk in blank on matroids/flow/TSP — these facts alone tend to pick up partial credit and
cover a likely multiple-choice item:

```
min cut       forward arcs ONLY;  val(f) = cap(S) proves BOTH optimal
matroids      the three axioms;  a basis is MAXIMAL (not maximum) independent
TSP           Euler = edges (easy);  Hamilton = nodes (NP-complete)
              MST-doubling = 2;  Christofides = 3/2 (matching on ODD-degree vertices)
```
