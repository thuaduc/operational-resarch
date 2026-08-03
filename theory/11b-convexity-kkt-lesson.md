# Convexity, Slater and KKT — from scratch

Teaching companion to [11-nonlinear-and-convex-kkt](11-nonlinear-and-convex-kkt.md), second half.
That file is the reference; **this one assumes you know nothing**. Day 3's unconstrained half is
[11a](11a-nonlinear-unconstrained-lesson.md).

Exam slot **E7b**. This is where the recent papers put their weight: **SS25's entire 20-point
nonlinear question was constrained**, with no gradient/Hessian classification at all.

---

# Part 0 — What the exam actually asks

**SS25 E7** *Minimal Circle Enclosure* is the model question. Its parts, in order:

```
a) State the definition of a convex function.
b) Prove that any norm ‖·‖ on ℝᵈ is convex.
c) Formulate the problem as a convex optimisation problem; show f and gᵢ are convex.
d) Show Slater's condition holds — give an explicit strictly feasible point.
e) State the KKT conditions.
f) Use KKT to show 1 = Σλᵢ and c* = Σλᵢpᵢ.
g) Suppose λᵢ > 0. What does that imply about pᵢ?
```

That's the full arc: **define → prove convex → formulate → verify a constraint qualification →
write KKT → extract a conclusion → interpret a multiplier.** SS24 P7b and retake A8 are shorter
versions of the same arc.

Notice how much of it is *definitions and verification* rather than calculation. Parts (a), (b),
(d) and (g) need almost no arithmetic — and they're the ones people skip.

---

# Part 1 — Two different things called "convex"

Blurring these costs marks. They are separate ideas and you usually need both.

## A convex SET — about the domain

```
C is convex  ⟺  for all x, y ∈ C and all λ ∈ [0,1]:   λx + (1−λ)y ∈ C
```

In words: **take any two points in the set; the whole straight line between them stays inside.**
No dents, no holes.

`λx + (1−λ)y` as `λ` runs from 0 to 1 traces exactly that segment: `λ=1` gives `x`, `λ=0` gives
`y`, `λ=½` gives the midpoint.

| Convex | Not convex |
|---|---|
| halfspace `{x : aᵀx ≤ b}` | a circle *curve* `x² + y² = r²` |
| hyperplane `{x : aᵀx = b}` | a ring / annulus |
| ball, closed disk | a union of two disjoint disks |
| any **intersection** of convex sets | unions generally |

The circle case is the classic trap: the **curve** `x²+y²=r²` is compact but **not convex** (the
chord between two points cuts through the middle, which isn't on the curve). The **closed disk**
`x²+y² ≤ r²` is both.

## A convex FUNCTION — about the graph

```
f convex  ⟺  f(λx₁ + (1−λ)x₂)  ≤  λ f(x₁) + (1−λ) f(x₂)     ∀ λ ∈ [0,1]
```

In words: **the chord between any two points on the graph lies above the graph.** Left side = the
function at an intermediate point; right side = the straight-line interpolation.

- **Strictly convex** — `<` for all `x₁ ≠ x₂`, `λ ∈ (0,1)`. `x²` is; `|x|` is convex but *not*
  strictly (it's linear on each side).
- **Concave** — reverse the inequality. Equivalently `−f` is convex.

## Why any of this matters

```
convex objective + convex feasible set  ⟹  every local minimum is a GLOBAL minimum
```

That is the entire payoff. For a general nonlinear problem you can only find candidates and hope;
for a convex one, finding a local solution *finishes* the problem. Everything in Parts 5–7 is
machinery for exploiting that.

---

# Part 2 — Proving a function convex

Four routes. Pick by what the question gives you.

## Route A — straight from the definition

Use when `f` is non-differentiable or abstract — like a norm. **This is SS25 E7b**, and it's the
part people skip.

> **Claim.** Any norm `‖·‖` on `ℝᵈ` is convex.
>
> **Proof.** Let `x, y ∈ ℝᵈ` and `λ ∈ [0,1]`.
> ```
> ‖λx + (1−λ)y‖  ≤  ‖λx‖ + ‖(1−λ)y‖         (triangle inequality)
>                =  |λ|·‖x‖ + |1−λ|·‖y‖      (homogeneity)
>                =  λ‖x‖ + (1−λ)‖y‖          (λ ∈ [0,1], so both are ≥ 0)
> ```
> which is the definition of convexity. ∎

Three lines. Note the last step is doing real work — you must say **why** the absolute values come
off, namely that `λ` and `1−λ` are non-negative because `λ ∈ [0,1]`. Skipping that is the usual
mark lost.

## Route B — the Hessian (the computational route)

On a **convex domain**:
```
H_f(x) ⪰ 0  everywhere   ⟺   f convex
H_f(x) ≻ 0  everywhere   ⟹   f strictly convex        (⟸ is FALSE — see x⁴)
H_f(x) ≺ 0  everywhere   ⟹   f strictly concave
```

This is the Day 3 machinery reused ([11a](11a-nonlinear-unconstrained-lesson.md) Part 5): compute
minors, or det/trace for a 2×2. **Say that the domain is convex** — the Hessian condition alone
isn't enough without it.

## Route C — composition rules (the fast route)

Convexity is preserved by:

```
α·f            for α ≥ 0
f + g          sums of convex functions
f(Ax + b)      affine substitution into a convex function
max{f₁,…,fₘ}   pointwise maximum
h ∘ g          h convex AND non-decreasing, g convex
```

Also: **every affine function `aᵀx + b` is both convex and concave** (its chord *is* the graph).

This is how SS25 E7c disposes of the constraint functions in one line:
> `gᵢ(r, c₁, c₂) = ‖M − pᵢ‖₂ − r` is, by part (b), **the sum of two convex functions**, hence convex.

(`‖M − pᵢ‖₂` is a norm of an affine function of `M` — convex by (b) plus the affine rule — and
`−r` is affine, hence convex. Sum of convex is convex.)

## Route D — disproof

To show something is **not** convex, exhibit **one** triple `(x, y, λ)` violating the inequality.
A single counterexample is a complete answer.

---

# Part 3 — The constrained problem, in standard form

Everything below assumes this shape. **Convert to it first**, always:

```
min  f(x)
s.t. gᵢ(x) ≤ 0        i = 1,…,m        inequalities, written "≤ 0"
     hⱼ(x) = 0        j = 1,…,p        equalities
```

Two conversions you'll need:
- A `≥` constraint: `g(x) ≥ 0` becomes `−g(x) ≤ 0`.
- A maximisation: `max f` becomes `min −f`.

Getting a sign wrong here propagates through everything, so do it explicitly and write it down.

**Active vs inactive** at a point `x*`:
```
gᵢ(x*) = 0   →  ACTIVE (tight, binding)   — the constraint is "pressing"
gᵢ(x*) < 0   →  INACTIVE (slack)          — there's room to spare
```
Same vocabulary as LP duality, and it does the same job.

---

# Part 4 — The Lagrangian

The Lagrangian folds the constraints into the objective, each weighted by a **multiplier**:

```
L(x, λ, μ)  =  f(x)  +  Σᵢ λᵢ gᵢ(x)  +  Σⱼ μⱼ hⱼ(x)          (for a MIN problem)
```

```
λᵢ  multiplier of inequality i   —  SIGN-CONSTRAINED,  λᵢ ≥ 0
μⱼ  multiplier of equality j     —  FREE in sign
```

**Why `λ ≥ 0` but `μ` free.** An inequality has a direction: violating `gᵢ ≤ 0` means `gᵢ > 0`, and
the penalty `λᵢgᵢ` must *increase* the objective you're minimising — so `λᵢ` can't be negative. An
equality can be violated in either direction, so its multiplier needs both signs available.

This is exactly the SOB pattern from LP duality: `≤` rows get sign-restricted duals, `=` rows get
free ones. **`λ` is a shadow price** — `λ* ≈ ∂(optimal value)/∂b`, the same interpretation as
`yᵢ*` on Day 2.

For a **max** problem the convention flips to `L = f − Σλᵢgᵢ − Σμⱼhⱼ`. Simpler: convert to a min
and never worry about it.

Note SS25 wrote `L = f + Σλᵢgᵢ − μr`. The `−μr` handles the separate constraint `r ≥ 0`, i.e.
`−r ≤ 0`, with multiplier `μ ≥ 0. The minus sign is the `≥`→`≤` conversion, already applied.

---

# Part 5 — The four KKT blocks

Memorise these as four labelled blocks; the exam asks you to "state the KKT conditions" and
scores each block.

```
1. STATIONARITY            ∇f(x*) + Σᵢ λᵢ∇gᵢ(x*) + Σⱼ μⱼ∇hⱼ(x*) = 0
                           (equivalently ∇ₓL = 0)

2. PRIMAL FEASIBILITY      gᵢ(x*) ≤ 0     for all i
                           hⱼ(x*) = 0     for all j

3. DUAL FEASIBILITY        λᵢ ≥ 0         for all i
                           (μⱼ free)

4. COMPLEMENTARY SLACKNESS λᵢ · gᵢ(x*) = 0    for all i
```

**Stationarity** generalises `∇f = 0`. Unconstrained, the gradient must vanish. Constrained, the
gradient may be non-zero — but it must be exactly balanced by the constraint gradients pushing
back. You're allowed to be on a slope, provided a wall is holding you there.

**Complementary slackness** is the same statement as in LP duality:
```
λᵢ · gᵢ(x*) = 0    ⟺    for each i:   λᵢ = 0   OR   gᵢ(x*) = 0
```
> **An inactive constraint has multiplier zero. A non-zero multiplier means the constraint is
> active.**

Economically identical to Day 2: a resource you aren't fully using has zero shadow price. If you
know LP complementary slackness, you already know this.

---

# Part 6 — Solving a KKT system: the case split

Complementary slackness is what makes the system solvable. Each inequality gives you a fork:

```
for each i:    either  λᵢ = 0        (constraint inactive)
               or      gᵢ(x*) = 0    (constraint active)
```

With `m` inequalities that's up to `2ᵐ` cases — but most collapse instantly.

```
1. Convert to standard form.
2. Write the Lagrangian.
3. Write all four blocks.
4. Enumerate the cases from complementary slackness.
5. Solve each case's system.
6. DISCARD a case if:
      λᵢ < 0          → violates dual feasibility
      gᵢ(x*) > 0      → violates primal feasibility
7. Survivors are KKT points.
```

Step 6 is where most cases die, and **saying why you discarded each one is worth marks**.

## Shortcuts that kill cases fast

SS25's part (f) shows the best one — argue a constraint is active *before* enumerating:

> Since there exist two distinct distribution centres, the enclosing circle can't have radius
> zero, so `r > 0`. Complementary slackness gives `μr = 0`, hence **`μ = 0`**.

One sentence removes a whole branch. Look for facts in the problem statement that force a
variable positive or a constraint tight — the hints usually point at exactly this.

---

# Part 7 — Slater vs LICQ: what each one buys you

KKT conditions are only meaningful under a **constraint qualification**. Which one holds decides
how strong your conclusion is — this is the most examinable distinction in the topic.

## Slater's condition

```
Requires:  f and all gᵢ CONVEX,  all hⱼ AFFINE,
           and ∃ x̄ with  gᵢ(x̄) < 0  strictly, for all i   (and hⱼ(x̄) = 0)
```

The point `x̄` must satisfy the inequalities **strictly** — that's what "strictly feasible" means.

```
Slater holds  ⟹  a KKT point IS a GLOBAL optimum.
                  You are finished. No comparison needed.
```

**How to verify it in the exam:** exhibit an explicit point. SS25 E7d asks for exactly this, and
constructs one:
```
c̄ = the centroid of the points,      r̄ = maxᵢ ‖M̄ − pᵢ‖₂ + 1
```
Then `gᵢ = ‖M̄ − pᵢ‖₂ − r̄ ≤ −1 < 0` for every `i` — strictly feasible, with room to spare. The
`+1` is the trick: it manufactures the strictness.

## LICQ (linear independence constraint qualification)

```
Requires:  { ∇gᵢ(x*) : i ACTIVE } ∪ { ∇hⱼ(x*) }   linearly independent
```

```
LICQ holds  ⟹  KKT gives CANDIDATES only.
                You must evaluate f at each and COMPARE.
```

## The distinction, in one table

| | Slater | LICQ |
|---|---|---|
| Needs convexity? | **yes** | no |
| Checked where? | globally, one strictly feasible point | at the candidate point `x*` |
| Conclusion | KKT point is a **global optimum** | KKT points are **candidates** |
| Must compare `f` values? | **no** | **yes** |

The MC trap (it was SS25 1h): *"For a constrained optimization problem, a KKT point is always a
local optimum"* — **false**. And *"for a constrained convex problem a KKT point is always a global
minimizer if Slater's condition holds"* — **true**.

---

# Part 8 — The four-step recipe

For any constrained problem, in this order:

```
1. EXISTENCE   Is the feasible set compact (closed AND bounded, by Heine–Borel) and f
               continuous?  ⟹ a global min and max EXIST (Weierstrass).
               Without this, "the optimum is …" is unjustified.

2. CQ CHECK    Convex problem with a strictly feasible point?  → SLATER
               Otherwise check LICQ at the candidate.

3. SOLVE       Lagrangian → four KKT blocks → case split on complementary slackness
               → discard infeasible cases.

4. COMPARE     Evaluate f at every surviving candidate; largest = max, smallest = min.
               SKIP this step only if Slater held — KKT already certified globality.
```

Step 1 is cheap and frequently forgotten. Typical compact sets: a circle, a closed disk, a closed
polytope, a closed box. Typical **non**-compact: `x + y ≤ 2` alone (unbounded), `x²+y² < 1` (open).

---

# Part 9 — Worked example: SS25 E7, end to end

> `n` distribution centres at `pᵢ = (xᵢ, yᵢ) ∈ ℝ²`, at least two distinct. Find the centre
> `M = (c₁,c₂)` and radius `r ≥ 0` of the **smallest circle enclosing all of them**.

**(a) Definition.**
> `f : ℝⁿ → ℝ` is convex if for all `x, y ∈ ℝⁿ` and `λ ∈ [0,1]`:
> `f(λx + (1−λ)y) ≤ λf(x) + (1−λ)f(y)`.

**(b) Any norm is convex.** The three-line proof from Part 2, Route A.

**(c) Formulate, and show convexity.** Minimise the radius subject to every point being inside:
```
min_{r,c₁,c₂}  r
        s.t.   ‖M − pᵢ‖₂ − r ≤ 0     ∀i
               r ≥ 0,  c₁, c₂ ∈ ℝ
```
- `f(r,c₁,c₂) = r` is **linear, hence convex**.
- `gᵢ = ‖M − pᵢ‖₂ − r` is by (b) a **sum of two convex functions**, hence convex.

Note how (b) is *used* in (c) — the parts are built to chain.

**(d) Slater.** Take the centroid and inflate the radius:
```
c̄₁ = (1/n)Σxᵢ ,   c̄₂ = (1/n)Σyᵢ ,   M̄ = (c̄₁,c̄₂) ,   r̄ = maxᵢ‖M̄ − pᵢ‖₂ + 1
```
Then `gᵢ(r̄, c̄) = ‖M̄ − pᵢ‖₂ − r̄ ≤ −1 < 0` for all `i`. Strictly feasible ⟹ **Slater holds**, so
any KKT point is a global optimum.

**(e) KKT conditions.** With `L = f + Σλᵢgᵢ − μr`, `λᵢ, μ ≥ 0`:
```
Stationarity:            ∇L = 0
Primal feasibility:      gᵢ ≤ 0,   −r ≤ 0
Complementary slackness: λᵢgᵢ = 0,  μr = 0
Dual feasibility:        λᵢ ≥ 0,   μ ≥ 0
```

**(f) Derive `Σλᵢ = 1` and `c* = Σλᵢpᵢ`.**

*Kill `μ` first.* At least two points are distinct, so the enclosing circle has positive radius:
`r > 0`. Complementary slackness gives `μr = 0`, hence **`μ = 0`**.

*Stationarity in `r`:*
```
∂L/∂r  =  1 − Σᵢ λᵢ − μ  =  0        with μ = 0   ⟹   Σᵢ λᵢ = 1     ✓
```

*Stationarity in `c₁`:*
```
∂L/∂c₁  =  Σᵢ λᵢ · (c₁ − xᵢ)/‖pᵢ − M‖₂  =  0
```
Now use complementary slackness again: for each `i`, either `λᵢ = 0` (the term vanishes) or
`gᵢ = 0`, i.e. **`‖pᵢ − M‖₂ = r`**. So every surviving denominator equals `r`:
```
Σᵢ λᵢ (c₁ − xᵢ)/r = 0    ⟹    Σᵢ λᵢ(c₁ − xᵢ) = 0    ⟹    c₁·Σλᵢ = Σλᵢxᵢ
```
and since `Σλᵢ = 1`, **`c₁* = Σλᵢxᵢ`**. Same for `c₂`. ∎

With `λᵢ ≥ 0` summing to 1, that says the optimal centre is a **convex combination** of the
distribution centres.

**(g) What does `λᵢ > 0` mean?** By complementary slackness, `λᵢ > 0` forces `gᵢ = 0`, i.e.
`‖pᵢ − M‖₂ = r`. So **`pᵢ` lies exactly ON the boundary circle** — it's one of the points the
circle is actually touching.

---

# Part 10 — Traps and drills

## Where points are lost

1. **Not converting to standard form**, then getting a sign wrong throughout.
2. **`λ` free / `μ` sign-restricted** — it's the other way round.
3. **Skipping the definition or the convexity proof** because they "aren't real work". They're
   4–6 points on SS25.
4. **Forgetting `|λ| = λ`** needs `λ ∈ [0,1]` in the norm proof.
5. **Confusing convex sets with convex functions.** A circle *curve* is not convex; the disk is.
6. **Claiming Slater without exhibiting a point.** The point *is* the proof.
7. **Comparing objective values after Slater** — unnecessary; or **not comparing after LICQ** —
   required.
8. **Not discarding cases explicitly.** Say why each dead branch died.
9. **Skipping the existence step** (compact + continuous ⟹ Weierstrass) before asserting an
   optimum exists.

## The two statements to have cold

```
Slater  ⟹  KKT point is a GLOBAL optimum          (convexity required; done, no comparison)
LICQ    ⟹  KKT points are CANDIDATES only         (no convexity needed; must compare)
```

## Warm-up ladder (untimed)

1. `T11.1` *Convex Functions – Examples* — `[DRILL]` decide convexity for a list.
2. `T11.2` *Convex functions* — `[EXAM]` **prove convexity from the definition.** This is
   SS25 E7b's task. Drill until the `f(λx+(1−λ)y) ≤ …` chain is automatic.
3. `D10.1` *Topological Properties and Convex Sets* — `[CONCEPT]` sets vs functions, compactness.
   Skim; know the distinction, don't grind the topology.
4. `T10.2` *Lagrange multipliers* — `[DRILL]` build the Lagrangian, equalities only.
5. `D10.3` *KKT-conditions* — `[EXAM]` full system with inequalities and the case split. Core rep.
6. `T10.3` *Non-linear optimization – KKT conditions* — `[EXAM]` second rep.
7. Skip `T11.3` *Hedge Algorithm* and all of CE-11 — online optimization, never examined.

Sheet 10 and 11 are in `exercises/11-nonlinear-convex-optimization/`; CE-10 is in
`central exercises/11-nonlinear-convex-optimization/`.

## Then the papers (timed, one minute per point)

- **SS25 E7** *Minimal Circle Enclosure*, all parts. The model question — do it properly.
- **SS24 P7b** — KKT with a single linear constraint `x + y − 2 ≤ 0`, Slater verified. Shorter.
- **retake A8** — Lagrange with nonlinear constraints.

## Why this day matters more than its point count suggests

SS25 put its **entire** nonlinear question here and none in the unconstrained half. SS24 split
20 points across both. If SS26 tracks SS25's format — as it has elsewhere — this is the half that
pays.
