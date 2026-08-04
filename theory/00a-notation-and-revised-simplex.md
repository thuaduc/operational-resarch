# Notation, and the minimum revised simplex you need

Every symbol used across the LP chapters, defined once, with a concrete instance beside it.
Then the smallest amount of revised simplex that gets you through E2.

Companion to [03a-sensitivity-lesson](03a-sensitivity-lesson.md), which uses the same example.

---

# Part 1 — The running example

Same workshop as the sensitivity lesson. Keep it visible while reading.

```
each table  uses 1 wood, 1 labour, earns €3
each chair  uses 1 wood, 2 labour, earns €4
available:  4 wood, 6 labour
```

```
max  3x₁ + 4x₂
s.t.  x₁ +  x₂ ≤ 4        (wood)
      x₁ + 2x₂ ≤ 6        (labour)
      x₁, x₂ ≥ 0
```

Optimum: **2 tables, 2 chairs, €14.**

---

# Part 2 — The symbols you are given

These come from the problem. **You never solve for them.**

### `x` — the decision variables

What you choose. A column vector, one entry per variable.
```
x = (x₁, x₂)ᵀ        x₁ = tables, x₂ = chairs
```
At the optimum, `x* = (2, 2)ᵀ`.

### `c` — the objective coefficients ("costs" or profits)

One number per variable: what each unit is worth.
```
c = (3, 4)ᵀ          €3 per table, €4 per chair
```
The objective is `cᵀx = 3x₁ + 4x₂`.

> The letter is `c` for "cost" even in a max problem where it's really profit. Don't read
> anything into it.

### `b` — the right-hand sides ("resources")

One number per **constraint**: how much you have.
```
b = (4, 6)ᵀ          4 wood, 6 labour
```

### `A` — the constraint matrix

One **row per constraint**, one **column per variable**. Entry `a_{ij}` = how much of resource
`i` one unit of variable `j` consumes.

```
              x₁   x₂
  wood    ⎡   1    1  ⎤
  labour  ⎣   1    2  ⎦        A ∈ ℝ^{m×n}
```

**Column `j` of `A`, written `a_j`, is the recipe for product `j`.** `a₁ = (1,1)ᵀ` says "a table
eats 1 wood and 1 labour". You'll use `a_j` constantly — every reduced-cost formula takes a column.

### Sizes — worth fixing now

```
m = number of constraints  = 2      (wood, labour)
n = number of variables    = 2      (before slacks)

A is m×n,   b is m×1,   c is n×1,   x is n×1
```

---

# Part 3 — Slack variables and standard form

Simplex needs **equalities**, not inequalities. Add a **slack variable** per `≤` row measuring
what's left over:

```
x₁ +  x₂ + s₁      = 4        s₁ = unused wood
x₁ + 2x₂      + s₂ = 6        s₂ = unused labour
```

Now `A` gains two columns:

```
        x₁  x₂  s₁  s₂
wood  ⎡  1   1   1   0 ⎤
labr  ⎣  1   2   0   1 ⎦
```

Two things to internalise:

- **The slack columns form an identity matrix.** That's what makes them convenient.
- **Slacks have objective coefficient 0.** They earn nothing. So `c = (3, 4, 0, 0)ᵀ`.

Standard form is then `max cᵀx s.t. Ax = b, x ≥ 0`.

**Reading a slack's value:**
```
sᵢ > 0  →  resource i has leftover  →  constraint NOT binding
sᵢ = 0  →  resource i fully used    →  constraint BINDING (tight)
```

---

# Part 4 — Basis and non-basis

With `m` constraints, exactly `m` variables are allowed to be non-zero. Those are the **basic**
variables. Everything else is **non-basic** and equals **zero**.

At our optimum `x₁ = 2, x₂ = 2, s₁ = 0, s₂ = 0`:

```
BASIC     {x₁, x₂}       — non-zero, listed in the tableau's Basis column
NON-BASIC {s₁, s₂}       — forced to zero
```

### `B` — the basis matrix

The columns of `A` belonging to the basic variables:
```
        x₁  x₂           ⎡ 1   1 ⎤
   B =              =    ⎢       ⎥        always m×m, always invertible
                         ⎣ 1   2 ⎦
```

### `N` — the non-basic columns

```
        s₁  s₂           ⎡ 1   0 ⎤
   N =              =    ⎢       ⎥        m × (n−m)
                         ⎣ 0   1 ⎦
```

### `c_B` and `c_N` — costs split the same way

```
c_B = (3, 4)ᵀ        objective coefficients of x₁, x₂
c_N = (0, 0)ᵀ        objective coefficients of s₁, s₂  (slacks earn nothing)
```

> **`c_B` is the single most important thing to get right.** Pick the wrong variables and every
> price downstream is wrong. Read it off the Basis column of the tableau, in that order.

---

# Part 5 — The derived quantities

Everything below is *computed*, not given.

### `B⁻¹` — the inverse basis

```
        ⎡  2   −1 ⎤
B⁻¹  =  ⎢         ⎥          det B = 2·1 − 1·1 = 1
        ⎣ −1    1 ⎦
```

For a 2×2 `[[a,b],[c,d]]` the inverse is `1/det · [[d,−b],[−c,a]]` — swap the diagonal, negate
the off-diagonal, divide by the determinant.

**In the exam you almost never compute this.** It's already inside the tableau (Part 6).

### `b'` — the plan (also written `x_B`)

```
b' = B⁻¹b = (2·4 − 1·6, −1·4 + 1·6) = (2, 2)ᵀ
```
**The actual amounts to produce.** The RHS column of the tableau.
```
b' ≥ 0  ⟺  the basis is FEASIBLE
```

### `N'` — the tableau body

```
N' = B⁻¹N
```
Here `N = I`, so `N' = B⁻¹` exactly. In general it's the columns under the non-basic variables.

### `y` — the shadow prices (also `π`, also "dual variables", also "simplex multipliers")

```
yᵀ = c_Bᵀ B⁻¹

y₁ = 3·2 + 4·(−1) = 2        one more wood is worth €2
y₂ = 3·(−1) + 4·1 = 1        one more labour hour is worth €1
```

**One `yᵢ` per constraint.** These are the same numbers as the optimal dual solution.

### `z` — the objective value

```
z = c_Bᵀ b' = 3·2 + 4·2 = 14
  = yᵀb     = 2·4 + 1·6 = 14        ✓ two routes, same answer
```

### `Row 0` — the reduced costs

```
Row0_j  =  yᵀa_j − c_j
```
One entry per column. `Row0 = (0, 0, 2, 1)` here.

---

# Part 6 — Where the tableau comes from

```
 Basis │ x₁  x₂   s₁   s₂ │ RHS
───────┼──────────────────┼─────
 Row 0 │  0   0    2    1 │  14
───────┼──────────────────┼─────
   x₁  │  1   0    2   −1 │   2
   x₂  │  0   1   −1    1 │   2
```

Map every region back to a symbol:

| where | what it is | here |
|---|---|---|
| Basis column | which variables are basic, in row order | `x₁, x₂` |
| RHS column, body rows | `b' = B⁻¹b` — the plan | `(2, 2)` |
| RHS column, Row 0 | `z` | `14` |
| under basic columns | identity matrix | `[[1,0],[0,1]]` |
| under non-basic columns | `N' = B⁻¹N` | `[[2,−1],[−1,1]]` |
| Row 0 under basic | always `0` | `0, 0` |
| Row 0 under slack `sᵢ` | `yᵢ` | `2, 1` |

**The last row of that table is why you rarely need `B⁻¹`.** Shadow prices are sitting in Row 0,
and `B⁻¹` (or the part of it you need) is sitting in the body.

---

# Part 7 — The one derivation worth seeing

Why is `Row0_j = yᵀa_j − c_j`, and why does `≥ 0` mean optimal? Three lines, and then the sign
convention stops being arbitrary.

Split the objective and the constraints by basis:
```
z   = c_Bᵀx_B + c_Nᵀx_N
Ax = b   ⟹   Bx_B + Nx_N = b   ⟹   x_B = B⁻¹b − B⁻¹N x_N
```

Substitute the second into the first:
```
z = c_Bᵀ(B⁻¹b − B⁻¹N x_N) + c_Nᵀ x_N
  = c_BᵀB⁻¹b  −  (c_BᵀB⁻¹N − c_Nᵀ) x_N
  =     z₀     −      Row 0        · x_N
```

So for a non-basic `x_j` currently at zero:

> **Raising `x_j` by one unit changes the objective by `−Row0_j`.**

Everything follows:
```
Row0_j < 0   →  raising x_j INCREASES z   →  bring it in
Row0_j ≥ 0   →  raising x_j can't help    →  leave it out
all ≥ 0      →  nothing helps             →  OPTIMAL
```

That's the course's sign convention, derived rather than memorised.

> **Warning.** Many textbooks define `c̄_j = c_j − yᵀa_j`, the exact negation, and read optimality
> as "all `≤ 0`". **This course uses the version above.** State your convention in the exam.

---

# Part 8 — Revised simplex, the minimum

## What it is

Ordinary simplex updates the whole tableau each pivot. **Revised simplex keeps only `B⁻¹`** and
computes what it needs on demand. Same path, less bookkeeping.

## One iteration

```
1. PRICES        yᵀ = c_Bᵀ B⁻¹

2. REDUCED COSTS Row0_j = yᵀa_j − c_j  for each NON-BASIC j
                 all ≥ 0  →  STOP, optimal

3. ENTERING      pick the most negative Row0_j  →  x_j enters

4. DIRECTION     d = B⁻¹a_j
                 d ≤ 0  →  STOP, UNBOUNDED

5. LEAVING       ratio test:  min { b'ᵢ / dᵢ : dᵢ > 0 }
                 the argmin row leaves

6. UPDATE        swap the two variables between B and N; recompute B⁻¹. Repeat.
```

Steps 1 and 2 are the ones you actually need. **Steps 3–6 have never been asked on an endterm.**

## What is and isn't examined

| | Endterm | Midterm |
|---|---|---|
| The formula `yᵀ = c_BᵀB⁻¹`, `Row0 = yᵀa_j − c_j` | **yes** — SS25 E2 cites it | yes |
| Reading `b'`, `N'`, `y` off a given tableau | **yes** — every sensitivity question | yes |
| Executing an iteration (steps 3–6) | **never**, 5 papers | **yes** — midterm SS25 P4, 12 pts |

So: **know steps 1–2 cold, understand 3–6, drill nothing.** If you want insurance for the
iteration, midterm-2025 P4 is a 12-minute drill with `B⁻¹` supplied.

---

# Part 9 — Symbol quick-reference

| Symbol | Name | Size | Given or derived |
|---|---|---|---|
| `x` | decision variables | `n×1` | chosen |
| `c` | objective coefficients | `n×1` | given |
| `b` | right-hand sides / resources | `m×1` | given |
| `A` | constraint matrix | `m×n` | given |
| `a_j` | column `j` of `A` — the recipe for variable `j` | `m×1` | given |
| `sᵢ` | slack of constraint `i` | scalar | derived |
| `B` | basis matrix (basic columns of `A`) | `m×m` | derived |
| `N` | non-basic columns | `m×(n−m)` | derived |
| `c_B` | costs of the basic variables | `m×1` | derived |
| `B⁻¹` | inverse basis | `m×m` | derived |
| `b'` = `x_B` | the plan | `m×1` | `B⁻¹b` |
| `N'` | tableau body | `m×(n−m)` | `B⁻¹N` |
| `y` = `π` | shadow prices / duals | `1×m` | `c_BᵀB⁻¹` |
| `z` | objective value | scalar | `c_Bᵀb'` = `yᵀb` |
| `Row0_j` = `c'_j` | reduced cost of column `j` | scalar | `yᵀa_j − c_j` |

---

# Part 10 — Letters that mean different things in different chapters

The course reuses symbols. **Check the chapter before assuming.**

| Letter | In LP / simplex | Elsewhere |
|---|---|---|
| `c` | objective coefficients | **network flow: `c(e)` is the arc CAPACITY** in the max-flow chapter, but the *cost* per unit in min-cost flow. Read the setup. |
| `b` | right-hand sides | network flow: `b(v)` is supply (`>0`) or demand (`<0`) at a node |
| `u` | — | network flow: `u(e)` arc capacity |
| `x` | decision variables | network flow uses `f(e)` for flow instead |
| `y` | dual variables / shadow prices | KKT: the multipliers are `λ` (inequalities) and `μ` (equalities) — **same idea, different letters** |
| `λ` | — | KKT multiplier **and** the convex-combination weight in `λx + (1−λ)y`. Two unrelated uses, sometimes on the same page |
| `N` | non-basic columns | matroids: `N` sometimes a ground set |
| `m`, `n` | constraints, variables | graphs: often nodes, edges |

The `c`-for-capacity clash is the one that bites: SS25 E6 says *"integral capacities `c(e) ∈ ℕ`"*
while `theory/09` writes capacity as `u(e)`. Same object, two letters, both in your materials.

---

# Part 11 — The eight lines to have cold

```
c_B     = objective coefficients of the BASIC variables, in Basis-column order
yᵀ      = c_Bᵀ B⁻¹                      shadow prices, one per constraint
Row0_j  = yᵀa_j − c_j                    optimal ⟺ all ≥ 0  (max problem)
Row 0 under a basic column = 0           structural
Row 0 under slack sᵢ       = yᵢ          how you read prices off a tableau
b'      = B⁻¹b                           the plan; feasible ⟺ b' ≥ 0
z       = c_Bᵀb' = yᵀb                   two routes, same number
c'_new  = yᵀa_new − c_new                enters iff c_new > yᵀa_new
```

If those eight are automatic, every E2 sub-question is a lookup plus arithmetic.
Next: [03a-sensitivity-lesson](03a-sensitivity-lesson.md) for what to *do* with them.
