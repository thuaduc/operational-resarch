# Sensitivity analysis — from scratch

Teaching companion to [03-revised-simplex-and-sensitivity](03-revised-simplex-and-sensitivity.md).
That file is the reference; **this one assumes you know nothing**.

Exam slot **E2**, worth **14–19 points**. You are never asked to *run* simplex — you're handed a
finished tableau and asked what happens when the data changes.

**One example runs through this entire file.** Parts 1–8 use the same little factory throughout,
with real numbers you can check by hand. Only at Part 10 do we switch to the actual exam question.

---

# Part 0 — What sensitivity analysis even is

You've solved an LP. You know the best plan and the best profit. Then the boss asks:

- *"We could buy more wood. Is it worth it? How much would you pay per unit?"*
- *"What if the price of tables drops — do we change what we make?"*
- *"A supplier offers a new product line. Should we take it?"*

**Re-solving the LP from scratch for each question would be madness.** Sensitivity analysis
answers all of them by reading numbers off the *final tableau you already have*.

That's the whole subject. Not new theory — extracting more value from a solved problem.

---

# Part 1 — The running example

A workshop makes **tables** and **chairs**.

```
each table  uses 1 unit of wood, 1 hour of labour, earns €3
each chair  uses 1 unit of wood, 2 hours of labour, earns €4

available:  4 units of wood,  6 hours of labour
```

Write `x₁` = tables made, `x₂` = chairs made:

```
max  3x₁ + 4x₂                    ← profit
s.t.  x₁ +  x₂ ≤ 4                ← wood
      x₁ + 2x₂ ≤ 6                ← labour
      x₁, x₂ ≥ 0
```

## Solving it (just this once)

Add **slack variables** — the unused amount of each resource:

```
x₁ +  x₂ + s₁     = 4        s₁ = leftover wood
x₁ + 2x₂     + s₂ = 6        s₂ = leftover labour
```

Check the corners of the feasible region:

| plan | wood used | labour used | profit |
|---|---|---|---|
| `(0,0)` | 0 | 0 | 0 |
| `(4,0)` | 4 | 4 | 12 |
| `(0,3)` | 3 | 6 | 12 |
| **`(2,2)`** | **4** | **6** | **14** ← best |

**Optimum: 2 tables, 2 chairs, profit €14.** Both resources are fully used — `s₁ = s₂ = 0`.

---

# Part 2 — Where the tableau comes from

The variables that are non-zero at the optimum are **basic**: here `x₁` and `x₂`. The zero ones
(`s₁`, `s₂`) are **non-basic**.

**`B` is the matrix of the basic columns** — take the `x₁` and `x₂` columns out of the
constraints:

```
        x₁  x₂                    ⎡ 1   1 ⎤
wood     1   1        so    B  =  ⎢       ⎥
labour   1   2                    ⎣ 1   2 ⎦
```

Its inverse (det = 2·1 − 1·1 = 1, so no fractions appear):

```
        ⎡  2   −1 ⎤
B⁻¹  =  ⎢         ⎥
        ⎣ −1    1 ⎦
```

Now three quantities, each with a plain meaning:

**The plan.** `b' = B⁻¹b` — how much of each basic variable to make:
```
B⁻¹ · (4, 6)ᵀ  =  (2·4 − 1·6,  −1·4 + 1·6)  =  (2, 2)      ✓ 2 tables, 2 chairs
```

**The prices.** `yᵀ = c_Bᵀ B⁻¹`, where `c_B = (3, 4)` are the profits of the basic variables:
```
y₁ = 3·2 + 4·(−1) = 2
y₂ = 3·(−1) + 4·1 = 1          so  y = (2, 1)
```

**The profit.** `z = yᵀb = 2·4 + 1·6 = 14` ✓ — matches the table above.

## The finished tableau

```
 Basis │ x₁  x₂   s₁   s₂ │ RHS
───────┼──────────────────┼─────
 Row 0 │  0   0    2    1 │  14      ← prices, and total profit
───────┼──────────────────┼─────
   x₁  │  1   0    2   −1 │   2      ┐ the plan
   x₂  │  0   1   −1    1 │   2      ┘
```

Every number in it you just computed:

- the **RHS column** `(2, 2)` is `b' = B⁻¹b` — the plan
- the **body under `s₁, s₂`** is exactly `B⁻¹` — because the slack columns form an identity matrix
- **Row 0 under `s₁, s₂`** is `(2, 1) = y` — the prices
- **Row 0 under `x₁, x₂`** is `(0, 0)` — always zero under basic columns

**Keep this tableau in view. Everything from here on reads off it.**

---

# Part 3 — Row 0 and the sign convention

```
Row0_j  =  yᵀa_j − c_j
```

For a **max** problem:

```
optimal              ⟺  every Row-0 entry ≥ 0
worth bringing in    ⟺  Row-0 entry < 0
```

Our Row 0 is `(0, 0, 2, 1)` — all `≥ 0`, so the tableau is optimal. ✓

> **Warning.** Many textbooks define the reduced cost as `c_j − yᵀa_j`, the exact negation, and
> read optimality as "all `≤ 0`". **This course uses the version above.** Write the convention at
> the top of your answer so the marker knows which you mean.

Two facts drop out:

- **Under a basic column Row 0 is always 0** — `c_BᵀB⁻¹B − c_Bᵀ = 0`.
- **Under a slack column Row 0 is exactly `yᵢ`** — a slack has `c_j = 0` and `a_j = eᵢ`, so
  `Row0 = yᵀeᵢ − 0 = yᵢ`. Nothing to subtract.

That second fact is why you can read shadow prices straight off the tableau.

---

# Part 4 — Shadow prices, and proof they mean what I say

**`yᵢ` = how much extra profit one more unit of resource `i` would earn you.**

From our Row 0:
```
y₁ = 2      one more unit of WOOD is worth €2
y₂ = 1      one more hour of LABOUR is worth €1
```

## Let's verify that

Re-solve with **5 units of wood** instead of 4:

```
max 3x₁ + 4x₂   s.t.  x₁ + x₂ ≤ 5,   x₁ + 2x₂ ≤ 6
```
New plan: `B⁻¹(5,6)ᵀ = (2·5 − 6, −5 + 6) = (4, 1)`. Profit `= 3·4 + 4·1 = 16`.

```
14  →  16      exactly +2, exactly y₁      ✓
```

The shadow price is not an analogy. It's the literal derivative: `z = yᵀb`, so `∂z/∂bᵢ = yᵢ`.

**So you'd pay up to €2 per extra unit of wood** — above that it isn't worth it. That's the
business answer sensitivity analysis exists to give.

## This is Day 2's duality

`y` **is** the optimal dual solution — the same numbers you'd get solving (D). And the
complementary-slackness rules reappear as tableau facts:

```
resource not fully used  (sᵢ > 0, sᵢ basic)      ⟹  yᵢ = 0
resource fully used      (sᵢ = 0, non-basic)     ⟹  yᵢ ≥ 0
```

Here both resources are exhausted, so both prices are positive. Had we 100 units of wood, `s₁`
would be basic and `y₁ = 0` — spare wood is worth nothing at the margin.

---

# Part 5 — The idea that organises everything

Two halves of the tableau. **Each kind of change breaks only one of them:**

```
Change a RESOURCE amount b   →  the PLAN moves     (x_B = B⁻¹b)
                                Row 0 doesn't
                                ⟹ only FEASIBILITY can break
                                   (a basic variable might go negative)

Change a PRICE c             →  ROW 0 moves
                                the plan doesn't
                                ⟹ only OPTIMALITY can break
                                   (a Row-0 entry might go negative)
```

Both ranging procedures below are just: **push until the half at risk breaks.**

---

# Part 6 — RHS ranging: how much wood before the plan changes?

The shadow price `y₁ = 2` is only valid while the current basis stays optimal. Beyond some point
the plan itself must change. Where?

```
1. Take the tableau COLUMN of the slack s₁ — that column is B⁻¹e₁.
2. New plan:  x_B(δ) = b' + δ · (that column)
3. Require every entry ≥ 0.
4. Intersect → δ range.
```

From our tableau, the `s₁` column is `(2, −1)`:

```
x₁(δ) = 2 + 2δ  ≥ 0    ⟹   δ ≥ −1
x₂(δ) = 2 −  δ  ≥ 0    ⟹   δ ≤  2
```

**`δ ∈ [−1, 2]`, so wood ∈ [3, 6].** Inside that range:

```
z(δ) = 14 + 2δ           the shadow price of 2 stays valid
```

## What happens at the endpoints

```
δ = −1   (wood = 3)   →  x₁ = 0   the workshop stops making tables
δ = +2   (wood = 6)   →  x₂ = 0   the workshop stops making chairs
```

Check `δ = 2`: with wood 6 and labour 6, `B⁻¹(6,6)ᵀ = (6, 0)` — six tables, no chairs, profit
`3·6 = 18 = 14 + 2·2` ✓.

**Those endpoints are exactly where the basis becomes *degenerate*** — a basic variable sitting at
zero. That's Part 8, and it's what SS25 E2d asks.

---

# Part 7 — Cost ranging: how much can a price move?

Now Row 0 is at risk. **Two cases, and they behave differently.**

## Case A — the variable is BASIC (like our `x₁`)

Changing a basic variable's profit disturbs **all** of Row 0, because `y = c_BᵀB⁻¹` depends on it.

```
1. Take the tableau ROW of x₁ under the non-basic columns → (2, −1)
2. New Row 0 = old Row 0 + Δ · (that row)
3. Require ≥ 0.
```

Table profit `3 → 3 + Δ`:
```
under s₁:   2 + 2Δ ≥ 0   ⟹   Δ ≥ −1
under s₂:   1 −  Δ ≥ 0   ⟹   Δ ≤  1
```

**`Δ ∈ [−1, 1]`, so the table price can range over `[€2, €4]`** with the plan `(2,2)` staying
optimal. Inside the range the plan is unchanged and only the profit moves:
```
z(Δ) = 14 + Δ·x₁ = 14 + 2Δ
```

**Sanity check at €5** (`Δ = 2`, outside the range): Row 0 under `s₂` becomes `1 − 2 = −1 < 0`, so
`s₂` enters and the basis changes. Does that make sense? At €5 a table, making 4 tables and no
chairs gives `5·4 = 20`, beating `(2,2)`'s `5·2 + 4·2 = 18`. Yes — the plan really should change. ✓

## Case B — the variable is NON-BASIC

Only its own Row-0 entry moves:
```
c'_j − Δ ≥ 0    ⟹    c_j ≤ yᵀa_j
```
One-sided, and **neither the plan nor the profit changes inside the range** — you're making none
of it anyway, so its price is irrelevant until it becomes attractive enough to enter.

| | basic | non-basic |
|---|---|---|
| what moves | **all** of Row 0, via that variable's row | **one** Row-0 entry |
| range | two-sided | one-sided |
| `z` inside | `z + Δ·x_k` | unchanged |
| plan inside | unchanged | unchanged |

---

# Part 8 — A new product

*"A stool needs 1 wood and 1 labour and would sell for `p`. Worth making?"*

```
opportunity cost  =  yᵀa_new  =  2·1 + 1·1  =  3
```

Those resources are **already earning €3** in the current plan. So:

```
Row-0 entry:  c'_new = yᵀa_new − c_new = 3 − p
Enters  ⟺  c'_new < 0  ⟺  p > 3
```

**Make stools only if they sell above €3.** No new computation — just the `y` you already have.

If making one also costs `k` in labour charges, the break-even price is `p ≥ yᵀa_new + k`.

## Degeneracy, in one line

A basic feasible solution is **degenerate** when a basic variable equals **zero** in the RHS
column. From Part 6: the endpoints of an RHS range are precisely the perturbations that make the
basis degenerate, because that's where a basic variable hits zero.

---

# Part 9 — The sensitivity chain

The second recurring exam format links four steps:

```
1. What is the shadow price of resource i?
      → Row 0 under sᵢ                                    [Part 4]

2. Over what range of bᵢ is it valid?
      → RHS ranging                                       [Part 6]

3. Push bᵢ PAST the range. Now what?
      → the basic variable that hit zero LEAVES
      → entering variable by the DUAL ratio test: in the leaving row,
        among entries < 0, pick the one minimising |c'_j / row_j|
      → one pivot

4. New shadow price and its new range?
      → read the new Row 0; re-run step 2 on the new basis
```

**Sanity check for step 4:** in a max problem, *reducing* a resource can only *raise* its shadow
price. Scarcer means more valuable at the margin. If yours moved the other way, you've slipped.

---

# Part 10 — The exam question: SS25 E2

Now the real thing. **It looks strange, and here's why:**

```
max 0x₁ + 0x₂                      ← every profit is ZERO
s.t. 4x₁ + 2x₂ ≤ 100    (water)
     1x₁ + 2x₂ ≤  40    (sugar)
     1x₁      ≤  30    (lemons)
          0.5x₂ ≤ 20    (tea leaves)
```

The café gives its drinks away free, so this is a **feasibility problem** — "find any workable
plan", not "maximise". Since `c_B = 0`, everything price-related collapses:

```
y = c_BᵀB⁻¹ = 0        every shadow price is zero
Row 0 = 0              every entry
```

**Don't let that confuse you** — it's the same machinery as our workshop, with the prices switched
off. The questions still probe the structure.

Given tableau, basis `{x₁, x₂, s₃, s₄}`, with deliberately **wrong** Row 0:

```
 Basis │ x₁  x₂    s₁     s₂   s₃  s₄ │ RHS
 Row 0 │  0  −1     2      1    0   0 │   0     ← sabotaged
   x₁  │  1   0   1/3   −1/3    0   0 │  20
   x₂  │  0   1  −1/6    2/3    0   0 │  10
   s₃  │  0   0  −1/3    1/3    1   0 │  10
   s₄  │  0   0  1/12   −1/3    0   1 │  15
```

**(a) Correct Row 0.** All costs are zero, so `y = 0`, so every entry is `yᵀa_j − c_j = 0`:
```
 Row 0 │  0   0    0    0    0   0 │  0
```

**(b) Optimal?** Yes — every entry is `≥ 0`. Nothing is attractive to enter.

**(c) Range of the Lemonade price `p₁`.** Now `c_B = (p₁, 0, 0, 0)`. `x₁` is basic, so this is
**Case A** — use the `x₁` row under the non-basic columns `(s₁, s₂)`, which is `(1/3, −1/3)`:
```
under s₁:   p₁/3  ≥ 0   ⟹   p₁ ≥ 0
under s₂:  −p₁/3  ≥ 0   ⟹   p₁ ≤ 0
```
Both together force **`p₁ = 0`**. So no — you can't charge for Lemonade and keep this basis, i.e.
keep giving Iced Tea away free.

**(d) Perturbing the water capacity until the BFS is degenerate.** Water is constraint 1, so use
the **`s₁` column** — exactly Part 6:
```
x₁ :  20 + δ/3   ≥ 0   ⟹   δ ≥ −60
x₂ :  10 − δ/6   ≥ 0   ⟹   δ ≤  60
s₃ :  10 − δ/3   ≥ 0   ⟹   δ ≤  30
s₄ :  15 + δ/12  ≥ 0   ⟹   δ ≥ −180
```
**`δ ∈ [−60, 30]`**, i.e. water between 40 and 130. At the endpoints:
```
δ = −60  (water 40)   →  x₁ hits zero
δ = +30  (water 130)  →  s₃ hits zero
```

**(e) New drink** using 3 L water and 1 kg lemon at price `p_S`. Exactly Part 8, with `y = 0`:
```
c'_S = yᵀa_new − p_S = 0 − p_S = −p_S
```
Enters iff `c'_S < 0`, i.e. **`p_S > 0`**. Any positive price works — with all current prices at
zero, the resources have no opportunity cost.

---

# Part 11 — Traps and drills

## Where points are lost

1. **Wrong sign convention.** Course: `Row0 = yᵀa_j − c_j`, optimal ⟺ all `≥ 0`.
2. **Wrong tableau slice.** RHS ranging uses the slack's **column**; cost ranging for a basic
   variable uses that variable's **row**. Column vs row.
3. **Using `Δz = yᵢδ` outside the validity range.** The price is only valid while the basis is.
4. **Mixing up basic and non-basic cost ranging.** Basic → whole Row 0, two-sided. Non-basic →
   one entry, one-sided.
5. **Not naming the variable that hits zero** at a range endpoint.
6. **Trying to run simplex.** You're not asked to.

## The six lines to have cold

```
yᵀ = c_Bᵀ B⁻¹                        shadow prices
Row0_j = yᵀa_j − c_j                  optimal ⟺ all ≥ 0 (max)
Row 0 under slack sᵢ  =  yᵢ
x_B(δ) = b' + δ·(column of sᵢ) ≥ 0    RHS ranging
z(δ) = z + yᵢ·δ
c'_new = yᵀa_new − c_new              enters iff c_new > yᵀa_new
```

## Test yourself on the workshop

Cover the answers and redo it from the tableau alone:

```
 Basis │ x₁  x₂   s₁   s₂ │ RHS
 Row 0 │  0   0    2    1 │  14
   x₁  │  1   0    2   −1 │   2
   x₂  │  0   1   −1    1 │   2
```

1. What is one extra hour of labour worth? *(→ `y₂ = 1`)*
2. Over what range of **labour** does that stay valid? *(use the `s₂` column `(−1, 1)`)*
3. A new product needs 2 wood and 1 labour. Minimum viable price? *(→ `2·2 + 1·1 = 5`)*
4. How far can the **chair** price move before the plan changes? *(Case A, `x₂` row `(−1, 1)`)*

Answers to 2 and 4: labour `∈ [4, 8]`; chair price `∈ [3, 6]`.

## Warm-up ladder (untimed)

1. `T3.3` *Sensitivity* — `[DRILL]` ranging in isolation, no story. Start here.
2. `T3.2` *Lemonade Production* — `[EXAM]` closest sheet match to SS25 E2.
3. `D3.2` *Waldgeist Distillery* — `[EXAM]` **your main source for the full chain.**
4. `S3.5` *PopCo* — `[EXAM]` second rep of the chain.
5. `D3.1` / `T3.1` / `S3.2` *Revised Simplex* — `[CONCEPT]` you need the Row 0 formula, not the
   algorithm. No endterm has asked for a revised-simplex iteration.

## Then the papers (timed)

- **SS25 E2** (16) — the wrong-Row-0 format. Current.
- **SS21 A1** *Backmischung* (16) — includes RHS ranging.
- **Midterm SS25 P5** and **SS26 P4** — the midterms carry more sensitivity than the endterms.
