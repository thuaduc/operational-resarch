# Sensitivity analysis — from scratch

Teaching companion to [03-revised-simplex-and-sensitivity](03-revised-simplex-and-sensitivity.md).
That file is the reference; **this one assumes you know nothing** beyond what a simplex tableau
looks like.

Exam slot **E2**, worth **14–19 points**. On most papers. You are never asked to *run* simplex —
you're handed a finished tableau and asked what happens when the data changes.

---

# Part 0 — What the exam actually asks

**SS25 E2** *Lemonade or Iced Tea* is the current format. You get a tableau that is **feasible
but has deliberately wrong Row 0**, then:

```
a) What is the correct Row 0?
b) After the correction, is the tableau optimal? Why?
c) Derive the range of the price p₁ for which the current basis stays optimal.
d) Under what perturbation of the water capacity does the BFS become degenerate?
   Show which basic variable first hits zero.
e) A new drink uses 3 L water and 1 kg lemon. Write its reduced cost and state
   the condition on p_S for it to enter the basis.
```

Five sub-questions, all read off one tableau. **No LP is ever solved.**

The other recurring shape is the **sensitivity chain**: shadow price → validity range → push
past the range → recompute the new shadow price.

---

# Part 1 — Anatomy of a tableau

```
 Basis │  x₁   x₂    s₁     s₂    s₃   s₄ │ RHS
───────┼─────────────────────────────────┼──────
 Row 0 │   0    0    ...   ...    0    0 │   z      ← the PRICES
───────┼─────────────────────────────────┼──────
   x₁  │   1    0   1/3   −1/3    0    0 │  20      ┐
   x₂  │   0    1  −1/6    2/3    0    0 │  10      │ the PLAN
   s₃  │   0    0  −1/3    1/3    1    0 │  10      │
   s₄  │   0    0  1/12   −1/3    0    1 │  15      ┘
```

**The Basis column** lists which variables are currently *in use* (non-zero). Everything not
listed is **non-basic** and equals **zero**. So this tableau says: make 20 of `x₁`, 10 of `x₂`,
and `s₁ = s₂ = 0`.

**The RHS column** is the plan: `b' = B⁻¹b`. Those are the actual values of the basic variables.
A basis is **feasible** exactly when every RHS entry is `≥ 0`.

**Row 0** is the price information — reduced costs and shadow prices. It's what tells you whether
you can do better.

**Under each basic column** you see a column of the identity matrix. That's what "basic" means.

---

# Part 2 — The one idea that organises everything

Two halves of the tableau, and **each kind of change breaks only one of them**:

```
Change the RHS b       →  the PLAN moves  (x_B = B⁻¹b)
                          Row 0 is untouched
                          ⟹ only FEASIBILITY can break

Change a cost c        →  ROW 0 moves
                          the plan b' is untouched
                          ⟹ only OPTIMALITY can break
```

That's it. Every ranging procedure below is just "push until the half that can break, breaks."

- **RHS ranging** → keep pushing `δ` until some `b'ᵣ` would go negative.
- **Cost ranging** → keep pushing `Δ` until some Row-0 entry would go negative.

If you remember nothing else, remember which half moves.

---

# Part 3 — Row 0 and the sign convention

**The course's convention, and the exam grades it:**

```
Row0_j  =  c_Bᵀ B⁻¹ a_j  −  c_j   =   yᵀa_j − c_j
```

Consequences for a **max** problem:

```
optimal          ⟺   every Row-0 entry ≥ 0
attractive to enter  ⟺   Row-0 entry < 0     (most negative enters)
```

> **Warning.** Many textbooks define the reduced cost the other way round
> (`c̄_j = c_j − yᵀa_j`), so their rule is "optimal ⟺ all ≤ 0". **The course uses the negated
> one.** Use the course sign and you'll match the mark scheme.

Two facts that follow immediately:

- **Under a basic column, Row 0 is always 0.** Because `c_Bᵀ B⁻¹B − c_Bᵀ = 0`.
- **Under a slack column, Row 0 is exactly `yᵢ`.** A slack has `c_j = 0` and `a_j = eᵢ`, so
  `Row0 = yᵀeᵢ − 0 = yᵢ`. Nothing to subtract.

That second one is how you read shadow prices off a tableau — no computation at all.

---

# Part 4 — Shadow prices

```
yᵀ = c_Bᵀ B⁻¹
```

**The shadow price `yᵢ` is the marginal value of one more unit of resource `i`.** Increase `bᵢ`
by 1 and the objective improves by `yᵢ` — as long as the basis stays optimal.

Why: `z = yᵀb`, so `∂z/∂bᵢ = yᵢ`. That's the whole content.

**Where to find them:** Row 0, under the slack variables. `y₁` sits under `s₁`, `y₂` under `s₂`,
and so on.

## This is Day 2's duality, seen from the other side

`y` **is the optimal dual solution.** The same numbers you'd get by solving (D) directly. And the
complementary-slackness statements you learned then reappear here as facts about tableaux:

```
constraint i is SLACK (sᵢ > 0, so sᵢ is basic)   ⟹   yᵢ = 0
constraint i is BINDING (sᵢ = 0, non-basic)      ⟹   yᵢ ≥ 0, usually > 0
```

A resource you aren't fully using is worth nothing at the margin. Same sentence as Sunday.

---

# Part 5 — RHS ranging: how far can a capacity move?

**Question:** `bᵢ → bᵢ + δ`. For which `δ` does the current basis stay optimal?

Row 0 doesn't move, so optimality is safe. Only feasibility can break — some basic variable might
be driven negative.

```
1. Take the tableau COLUMN OF THE SLACK sᵢ. That column is B⁻¹eᵢ.
2. New plan:   x_B(δ) = b' + δ · (that column)
3. Require every entry ≥ 0. That gives one inequality per row.
4. Intersect them → δ ∈ [δ_min, δ_max].
5. Inside the range:  z(δ) = z + yᵢ·δ
6. At each endpoint, name the basic variable that hits exactly zero.
```

Step 1 is the trick worth memorising: **the slack's column *is* `B⁻¹eᵢ`**, already sitting in the
tableau. You never invert anything.

Step 6 is asked explicitly (SS25 E2d) — see Part 9.

---

# Part 6 — Cost ranging: how far can a price move?

**Question:** `c_k → c_k + Δ`. Now the plan is safe and Row 0 is at risk. Two cases, and they
behave differently.

## Case A — `x_k` is BASIC

Changing a basic variable's cost disturbs **all of Row 0**, because `y = c_Bᵀ B⁻¹` depends on it.

```
1. Take the tableau ROW of x_k.
2. New Row 0:   c'_j(Δ) = c'_j + Δ · (row of x_k)_j     for every non-basic j
3. Require all ≥ 0 → intersect → Δ interval.
4. Inside:  z(Δ) = z + Δ · x_k      (the plan is unchanged, so z moves with the amount made)
5. Outside: the first Row-0 entry to go negative names the entering variable — one pivot
   gives the new plan.
```

## Case B — `x_j` is NON-BASIC

Only *its own* Row-0 entry moves:

```
c'_j(Δ) = c'_j − Δ ≥ 0    ⟹    Δ ≤ c'_j    ⟹    c_j ≤ yᵀa_j
```

One-sided. And **neither `z` nor `x*` changes inside the range** — you're not making any of `x_j`
anyway, so its price is irrelevant until it becomes attractive enough to enter.

| | basic `x_k` | non-basic `x_j` |
|---|---|---|
| what moves | **all** of Row 0, via the `x_k` row | **one** Row-0 entry |
| range | two-sided | one-sided, `c_j ≤ yᵀa_j` |
| `z` inside | `z + Δ·x_k` | unchanged |
| `x*` inside | unchanged | unchanged |

---

# Part 7 — Pricing a new column

**Question:** a new product needs `a_new` of the resources and sells at `c_new`. Worth making?

```
Opportunity cost  =  yᵀa_new        what those resources are already worth to you
Row-0 entry       =  c'_new = yᵀa_new − c_new

Enters the basis  ⟺   c'_new < 0   ⟺   c_new > yᵀa_new
```

In words: **make it only if it sells for more than the resources it consumes are already earning
you.** If there's also a production cost `k`, the minimum viable price is `p ≥ yᵀa_new + k`.

That's SS25 E2e, and it needs no new computation — just the `y` you already read off Row 0.

---

# Part 8 — Degeneracy

A basic feasible solution is **degenerate** when some basic variable equals **zero** in the RHS
column.

Why it matters: it's the boundary case of RHS ranging. Push `δ` to an endpoint of the validity
range and, by construction, some basic variable hits exactly 0 — **the endpoints of the range are
precisely the perturbations that make the BFS degenerate.**

That's what SS25 E2d is really asking, in disguise.

---

# Part 9 — Worked example: SS25 E2

```
max 0x₁ + 0x₂                          ← note: a FEASIBILITY problem, all costs zero
s.t. 4x₁ + 2x₂ ≤ 100    (water)
     1x₁ + 2x₂ ≤  40    (sugar)
     1x₁      ≤  30    (lemons)
          0.5x₂ ≤  20    (tea leaves)
```

Given tableau, basis `{x₁, x₂, s₃, s₄}`, with **wrong Row 0**:

```
 Basis │ x₁  x₂    s₁     s₂   s₃  s₄ │ RHS
 Row 0 │  0  −1     2      1    0   0 │   0     ← wrong
   x₁  │  1   0   1/3   −1/3    0   0 │  20
   x₂  │  0   1  −1/6    2/3    0   0 │  10
   s₃  │  0   0  −1/3    1/3    1   0 │  10
   s₄  │  0   0  1/12   −1/3    0   1 │  15
```

**(a) Correct Row 0.** All objective coefficients are zero, so `c_B = (0,0,0,0)` and therefore
`yᵀ = c_BᵀB⁻¹ = 0`. Every Row-0 entry is `yᵀa_j − c_j = 0 − 0 = 0`:

```
 Row 0 │  0   0    0     0    0   0 │  0
```

**(b) Optimal?** Yes — every Row-0 entry is `≥ 0` (all zero), so no variable is attractive to
enter. Optimal.

**(c) Range of `p₁` (price of Lemonade) keeping this basis optimal.**

Now `c_B = (p₁, 0, 0, 0)` since `x₁` is basic. `x₁` is basic, so this is **Case A** — all of
Row 0 moves, via the `x₁` row. The `x₁` row under `(s₁, s₂)` is `(1/3, −1/3)`, so:

```
c'_{s₁} = p₁/3        c'_{s₂} = −p₁/3
```

Optimality needs both `≥ 0`:
```
p₁/3  ≥ 0   ⟹   p₁ ≥ 0
−p₁/3 ≥ 0   ⟹   p₁ ≤ 0
```

Together: **`p₁ = 0` and nothing else.** So no, you cannot charge for Lemonade and keep this
basis. Answering the question as asked: it is *not* possible to sell Lemonade at a positive price
while keeping Iced Tea free.

**(d) Perturbation of water capacity making the BFS degenerate.**

Water is constraint 1, so take the **`s₁` column** and add `δ` times it to the RHS:

```
 x₁ :  20 + δ/3   ≥ 0   ⟹   δ ≥ −60
 x₂ :  10 − δ/6   ≥ 0   ⟹   δ ≤  60
 s₃ :  10 − δ/3   ≥ 0   ⟹   δ ≤  30
 s₄ :  15 + δ/12  ≥ 0   ⟹   δ ≥ −180
```

Intersecting: **`δ ∈ [−60, 30]`**, i.e. water capacity between 40 and 130.

At the endpoints the BFS goes degenerate:
```
δ = −60  (water = 40)   →  x₁ hits zero
δ = +30  (water = 130)  →  s₃ hits zero
```

Notice the binding constraints: `x₁` is the tightest going down, `s₃` the tightest going up. That
pair *is* the answer to "which basic variable first hits zero".

**(e) New drink, 3 L water and 1 kg lemon, price `p_S`.**

The column is `a_new = (3, 0, 1, 0)`. Since `y = 0`:

```
c'_S  =  yᵀa_new − p_S  =  0 − p_S  =  −p_S
```

Enters iff `c'_S < 0`, i.e. **`p_S > 0`**. Any positive price makes it worth introducing — which
makes sense, since with all current prices at zero the resources have no opportunity cost.

---

# Part 10 — The sensitivity chain

The other recurring format, in four linked steps:

```
1. What is the shadow price of resource i?
      → read Row 0 under sᵢ

2. Over what range of bᵢ does that price stay valid?
      → RHS ranging (Part 5)

3. Now push bᵢ past the range. What happens?
      → the basic variable that hit zero LEAVES the basis
      → find the entering variable by the DUAL ratio test: in the leaving row,
        among entries < 0, pick the one minimising |c'_j / row_j|
      → one pivot

4. What is the new shadow price, and its new range?
      → read the new Row 0; re-run Part 5 on the new basis
```

Sanity check for step 4: in a **max** problem, decreasing `bᵢ` can only *increase* `yᵢ`. Scarcer
resource, higher marginal value. If your new shadow price moved the other way, you've slipped.

---

# Part 11 — Traps and drills

## Where points are lost

1. **Wrong sign convention.** The course's Row 0 is `yᵀa_j − c_j`, so **optimal ⟺ all ≥ 0**.
   Textbooks often use the negation. Write the convention at the top of your answer.
2. **Using the wrong tableau slice.** RHS ranging uses the *slack's column*; cost ranging for a
   basic variable uses that variable's *row*. Column vs row.
3. **Applying `Δz = yᵢδ` outside the validity range.** The shadow price is only valid while the
   basis is optimal. Past the endpoint it changes.
4. **Confusing basic and non-basic cost ranging.** Basic → whole Row 0, two-sided. Non-basic →
   one entry, one-sided.
5. **Forgetting to name the variable that hits zero** at a range endpoint.
6. **Trying to run simplex.** You're not asked to. Everything is read off.

## Say these without looking

```
yᵀ = c_Bᵀ B⁻¹                       shadow prices
Row0_j = yᵀa_j − c_j                 optimal ⟺ all ≥ 0 (max)
Row 0 under slack sᵢ = yᵢ
x_B(δ) = b' + δ·(column of sᵢ) ≥ 0   RHS ranging
z(δ) = z + yᵢ·δ
c'_new = yᵀa_new − c_new             enters iff c_new > yᵀa_new
```

## Warm-up ladder (untimed)

1. `T3.3` *Sensitivity* — `[DRILL]` ranging in isolation, no story. Start here.
2. `T3.2` *Lemonade Production* — `[EXAM]` closest sheet match to SS25 E2 — a lemonade factory,
   an optimal tableau handed over, questions read off it.
3. `D3.2` *Waldgeist Distillery* — `[EXAM]` **your main source for the full chain** now that the
   retake is out of scope. Do it properly.
4. `S3.5` *PopCo* — `[EXAM]` second rep of the chain.
5. `D3.1` / `T3.1` / `S3.2` *Revised Simplex* — `[CONCEPT]` **you need the Row 0 formula, not the
   algorithm.** No endterm has asked for a revised-simplex iteration; SS25 merely *cites*
   `Row0 = c_BᵀB⁻¹A − c`. Read for the formula, don't drill iterations.

Sheet 3 is `exercises/03-linear-programming-revised-simplex/sheet-03-exercises.pdf`; the
self-study section (S3.x) is in the second half of the same file.

## Then the papers (timed, one minute per point)

- **SS25 E2** (16) — the wrong-Row-0 format. Current.
- **SS21 A1** *Backmischung* (16) — sensitivity from an optimal tableau, including RHS ranging.
- **Midterm SS25 P5** and **midterm SS26 P4**, both *Sensitivity Analysis* — the midterms carry
  more of this than the endterms do, and it's the same task.

## Connections

- `y` **is** the optimal dual solution from Day 2. E2 and E3 are the same numbers from two sides.
  → [04a-duality-lesson](04a-duality-lesson.md)
- Slack constraint ⟹ zero shadow price **is** complementary slackness.
