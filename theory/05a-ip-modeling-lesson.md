# IP Modelling — from scratch

Teaching companion to [05-ip-modeling](05-ip-modeling.md). That file is the exhaustive catalogue for lookup. **This file assumes you know nothing** and builds up to the exam questions.

Exam slot **E4**, worth **21–35 points** — the largest single block on every past paper.

Work through Parts 0–3 in order. Part 4 onward is the exam material proper.

---

# Part 0 — The vocabulary

An **integer program** is four things:

```
min  40x₁ + 25x₂            ← OBJECTIVE   
s.t. x₁ + x₂ ≥ 3            ← CONSTRAINTS
     2x₁ + 5x₂ ≤ 12
     x₁, x₂ ∈ ℕ₀            ← DOMAINS
```

- A **variable** is a number the solver gets to choose. `x₁` and `x₂` here.
- A **parameter** is a number *given to you* in the problem text. The `40`, `25`, `3`, `12`.
  You never solve for a parameter.
- A **feasible solution** is any assignment of the variables that satisfies every constraint.
- An **optimal solution** is the feasible one with the best objective value.

**"Modelling" means translating a paragraph of English into those four blocks.** That is the
entire E4 task. You are never asked to *solve* the model — only to write it down correctly.

The one thing that makes it an *integer* program is the domain line: variables restricted to
whole numbers (`∈ ℕ₀`, `∈ ℤ`) or to `{0,1}`.

---

# Part 1 — Binary variables: the counting trick

A **binary variable** takes only the values 0 or 1. It encodes a yes/no decision:

```
y_i ∈ {0,1}      y_i = 1  means "we build school i"
                 y_i = 0  means "we don't"
```

There is no third option. This is why we write `∈ {0,1}` and not `0 ≤ y ≤ 1` — the latter would
allow `y = 0.4`, "build 40% of a school", which is meaningless.

## Why sums of binaries count things

Suppose `y₁, y₂, y₃, y₄, y₅ ∈ {0,1}` say which of five dorms get built. Then:

```
y₁ + y₂ + y₃ + y₄ + y₅
```

Each term contributes 1 if built and 0 if not. **So the sum is literally the number built.**
That gives you counting constraints for free:

| English | Constraint |
|---|---|
| build at most 3 | `y₁+y₂+y₃+y₄+y₅ ≤ 3` |
| build exactly 3 | `y₁+y₂+y₃+y₄+y₅ = 3` |
| build at least 1 | `y₁+y₂+y₃+y₄+y₅ ≥ 1` |
| build at most one of 2 and 3 | `y₂ + y₃ ≤ 1` |

## Why sums of binaries × parameters total things

Now attach a cost `c_i` to each dorm. Then:

```
c₁y₁ + c₂y₂ + c₃y₃ + c₄y₄ + c₅y₅
```

Each term is either `c_i` (if built) or `0`. **So the sum is the total cost of what you built.**

That's the whole mechanism. A binary variable multiplied by a parameter is a switch: the
parameter counts, or it doesn't.

| English | Constraint |
|---|---|
| total cost at most €450k | `Σᵢ cᵢyᵢ ≤ 450` |
| total capacity at least 600 | `Σᵢ nᵢyᵢ ≥ 600` |

**Everything else in this file is a variation on these two ideas.** Stop and make sure they feel
obvious before continuing.

---

# Part 2 — Summation notation, properly

This is where most people lose points, so go slowly.

## Two indices means a grid

When a decision involves a *pair* of things — student `j` assigned to school `i` — you need a
variable with two indices:

```
x_{i,j} ∈ {0,1}      = 1 if student j is assigned to school i
```

Picture it as a **grid**: schools down the side, students across the top. With 3 schools and 2 students:

```
              student 1   student 2
school 1        x₁,₁        x₁,₂
school 2        x₂,₁        x₂,₂
school 3        x₃,₁        x₃,₂
```

Now the two summations mean two different things:

- **`Σᵢ x_{i,j}`** — fix a student, add down that student's **column**. = "how many schools is
  student `j` assigned to?"
- **`Σⱼ x_{i,j}`** — fix a school, add across that school's **row**. = "how many students does
  school `i` have?"

Whenever you're unsure which sum to write, draw the grid and ask: *am I adding a row or a column?*

## Free indices and bound indices

Take:
```
Σ_{i∈I} x_{i,j} = 1        ∀j ∈ J
```

- `i` is **bound** — it's the summation index. It gets used up by the sum and does not appear in
  the result.
- `j` is **free** — it appears but is not summed over. So it must be quantified with `∀j ∈ J`.

**The rule: every index that appears in a constraint but is not summed over must carry a `∀`.**
Forget the `∀` and you lose the point, every time.

## What `∀` actually produces

`∀` does not mean "for all" in a vague sense. It means **make one copy of this constraint per
value of the index.** With `I = {1,2,3}` and `J = {1,2}`:

```
Σ_{i∈I} x_{i,j} = 1   ∀j ∈ J

expands to TWO constraints:
    j=1:   x₁,₁ + x₂,₁ + x₃,₁ = 1
    j=2:   x₁,₂ + x₂,₂ + x₃,₂ = 1
```

Each says: *this student's column adds to 1* — assigned to exactly one school. Correct.

Compare, swapping the roles:
```
Σ_{j∈J} x_{i,j} ≤ 1   ∀i ∈ I

expands to THREE constraints:
    i=1:   x₁,₁ + x₁,₂ ≤ 1
    i=2:   x₂,₁ + x₂,₂ ≤ 1
    i=3:   x₃,₁ + x₃,₂ ≤ 1
```

Each says: *this school's row adds to at most 1* — no school gets more than one student.
Completely different meaning.

## The recipe

> **Sum over the thing you are counting. Quantify over the thing you are counting *for*.**

"Each student must be assigned to exactly one school" — you're counting *schools*, for each
*student*. So sum over `i` (schools), quantify over `j` (students):
`Σ_{i∈I} x_{i,j} = 1 ∀j ∈ J`. ✓

If you ever write a constraint and can't say how many copies it expands into, you've made a
mistake. Check it.

---

# Part 3 — Your first model, built from nothing

The lecture's own example. Read the paragraph, then build it line by line.

> TUM is opening a new campus. Five dormitories can be built, with 100, 500, 400, 300 and 50
> places. Construction costs are €100k, €400k, €360k, €275k, €75k. 600 students are expected.
> For each student who does **not** get a place, the city pays €950. Minimise total cost.

## Step 1 — Index sets

There is one family of things: dormitories.
```
I = {1,2,3,4,5}        set of possible dormitories
```

## Step 2 — Parameters

Everything the text *gives* you:
```
n_i = capacity of dorm i     n = (100, 500, 400, 300, 50)
c_i = cost of dorm i         c = (100, 400, 360, 275, 75)   in €k
D   = 600                    students expected
p   = 950                    penalty per unhoused student
```

## Step 3 — Decision variables

What is actually being *decided*? Two things:
```
y_i ∈ {0,1}    = 1 if dorm i is built                    (the real decision)
s   ∈ ℕ₀       = number of students who DO get a place    (a consequence — but we need
                                                           it as a variable to write the
                                                           objective linearly)
```

That second variable is the one beginners miss. The cost depends on how many students are
housed, so "how many are housed" has to be nameable.

## Step 4 — Constraints

*You cannot house more students than you have places:*
```
s ≤ Σ_{i∈I} n_i y_i
```
Here `Σ nᵢyᵢ` is total capacity built (Part 1). If no dorm is built the right side is 0, forcing
`s = 0`. Correct.

*You cannot house more students than exist:*
```
s ≤ 600
```

## Step 5 — Objective

Two costs: construction, and penalties for the `600 − s` unhoused students.
```
min  Σ_{i∈I} c_i y_i  +  950·(600 − s)
```

## The finished model

```
min   Σ_{i∈I} c_i y_i + 950(600 − s)
s.t.  s ≤ Σ_{i∈I} n_i y_i
      s ≤ 600
      y_i ∈ {0,1}    ∀i ∈ I
      s ∈ ℕ₀
```

That is a complete, correct IP. **Notice the shape** — it's the shape of every answer you will
write: index sets, parameters, variables with domains, objective, constraints with quantifiers.

## Extra conditions (lecture slide 16)

The lecture then adds conditions one at a time. Each is one line. Try each before reading on:

| English | Constraint | Why |
|---|---|---|
| Dorms 2 and 3 share a site — at most one | `y₂ + y₃ ≤ 1` | sum of two binaries ≤ 1 |
| Total cost at most €450k | `Σᵢ cᵢyᵢ ≤ 450` | parameter × binary = total |
| At most 3 dorms | `Σᵢ yᵢ ≤ 3` | sum of binaries = count |
| Dorm 2 only if dorm 4 is built | `y₂ ≤ y₄` | see below |
| Dorm 4 only if all students housed | `600y₄ ≤ s` | if y₄=1 then s ≥ 600 |

## Why `y₂ ≤ y₄` means "2 only if 4"

Enumerate all four cases. This is the lecture's own table:

| `y₂` | `y₄` | `y₂ ≤ y₄`? | allowed? | matches "2 ⇒ 4"? |
|---|---|---|---|---|
| 0 | 0 | `0 ≤ 0` ✓ | yes | yes — didn't build 2, fine |
| 0 | 1 | `0 ≤ 1` ✓ | yes | yes — built 4 only, fine |
| 1 | 0 | `1 ≤ 0` ✗ | **no** | yes — this is the forbidden case |
| 1 | 1 | `1 ≤ 1` ✓ | yes | yes — built both, fine |

The inequality forbids exactly one combination: built 2 without 4. That is precisely the
logical implication `y₂ ⇒ y₄`.

**This is the technique for checking any binary constraint you're unsure about: enumerate the
cases and compare against the English.** Do it in the exam when you have doubts.

## The four logical operators (lecture slide 15)

| Logic | Written as |
|---|---|
| `x₁ ∨ x₂ ∨ … ∨ xₙ` (OR — at least one) | `x₁ + x₂ + … + xₙ ≥ 1` |
| `x₁ ∧ x₂ ∧ … ∧ xₙ` (AND — all) | `x₁ ≥ 1; x₂ ≥ 1; …` (separate rows) |
| `x₁ ⇒ x₂` (implication) | `x₁ ≤ x₂` |
| `x₁ ⇔ x₂` (equivalence) | `x₁ = x₂` |
| `¬A` (negation) | `1 − A` |

Negation is worth dwelling on: if `A ∈ {0,1}` means "it rains", then `1 − A` means "it doesn't
rain" — it's 1 exactly when `A` is 0. You will use `1 − A` constantly.

---

# Part 4 — Why integer programs are hard

## Rounding does not work

The natural question: why not solve it as an ordinary LP (allowing fractions) and round?

The lecture's counterexample:
```
max  x₁ + x₂
s.t.        2x₂ ≤ 7
     7x₁ + 16x₂ ≥ 56
     4x₁ +  3x₂ ≤ 20
            x₁, x₂ ∈ ℕ₀
```

The LP's corner points are `(0, 3.5)`, `(2.375, 3.5)`, `(3.54, 1.95)`. The **only** feasible
integer point in the whole region is `(2,3)` — and it is not the rounding of any of them. Round
and you land outside the feasible region entirely.

The lecture's framing, worth reproducing in a multiple-choice justification:

> Rounding is often fine for large production quantities with low marginal costs. It fails for
> **a few costly decisions**, and "rounding" a binary decision is meaningless — you get poor or
> infeasible solutions.

Geometrically: the LP feasible region is a solid polyhedron; the IP feasible region is only the
**lattice points inside it**. Scattered points have no corners and no edges, so simplex — which
walks from corner to corner — has nothing to walk on. Hence branch & bound (exam slot E5).

## The taxonomy

| | Form | Variables |
|---|---|---|
| **LP** | `max{cᵀx : Ax ≤ b, x ≥ 0}` | all continuous |
| **MIP** | `max cᵀx + hᵀy`, `Ax + Gy ≤ b`, `x ≥ 0`, `y ∈ ℕ₀` | mixed |
| **IP** | all structural variables in `ℕ₀` | all integer |
| **BIP** | `max cᵀx`, `Ax ≤ b`, `x ∈ {0,1}` | all binary |

## LP relaxation — the idea that links E4 to E5

**Take your IP and delete every integrality requirement** (`x ∈ {0,1}` becomes `0 ≤ x ≤ 1`).
What's left is an ordinary LP called the **LP relaxation**.

Because you deleted restrictions, the relaxation's feasible region **contains** the IP's. More
room to move means you can do at least as well, so for a max problem:

```
z_LP  ≥  z_IP        the relaxation is always an UPPER bound
```

(For a min problem it's a lower bound. Same reasoning.)

That bound is the entire engine of branch & bound — **so your modelling choices in E4 have
consequences in E5.** Which is what "formulation strength" means:

> Between two correct formulations of the same IP, the one whose **LP-relaxation region is
> smaller** is **stronger**. Tighter bound → more pruning → faster B&B.

Concrete example you'll meet in Part 6. Two ways to say "you can only assign students to a
school you built":
```
disaggregated:  x_{i,j} ≤ y_i           ∀i,j      ← stronger
aggregated:     Σ_j x_{i,j} ≤ M·y_i     ∀i        ← weaker
```
For *integer* values these allow exactly the same solutions. But in the relaxation, the
aggregated version permits `y_i = 0.01` with one student assigned — a fractional fifth of a
school — which the disaggregated version forbids. Same IP, worse bound.

Asked "which formulation is stronger and why": *equal integer feasible sets, smaller relaxation
region.*

## Complexity — small, cheap, and directly examined

- **P** — solvable by a deterministic machine in polynomial time `O(nᵏ)`.
- **NP** — solvable by a *nondeterministic* machine in poly time. Equivalently, and more
  usefully: a proposed solution can be **verified** in poly time.
- **Polynomial reduction** — A reduces to B if instances of A convert into instances of B in
  poly time and solutions convert back. Then B is at least as hard as A.
- **NP-hard** — every problem in NP reduces to it. **Need not itself be in NP.**
- **NP-complete** — in NP **and** NP-hard.

That last distinction is a standard multiple-choice trap. Optimisation versions ("find the
cheapest tour") are typically NP-**hard**; decision versions ("is there a tour under 100km?")
are NP-**complete**.

**The result to memorise:**

> It is **NP-hard to decide whether an IP has a feasible solution** — not merely to optimise it.

Proof sketch (SAT reduces to binary IP), three reproducible lines:
```
For each boolean variable xᵢ, introduce vᵢ ∈ {0,1} with vᵢ = 1 ⟺ xᵢ = TRUE.
Each clause C = x₁ ∨ ¬x₂ ∨ … ∨ xᵢ  becomes  v₁ + (1 − v₂) + … + vᵢ ≥ 1.
A satisfying assignment exists ⟺ the IP is feasible.
```
Note that the clause translation is just the OR rule from Part 3, with `¬x₂` written `1 − v₂`.

Cook (1971) proved SAT NP-complete. Karp (1972) added TSP, scheduling, colouring, **knapsack,
bin packing, set cover, integer programming**.

**LP side, also examinable.** Simplex is **exponential in the worst case** — Klee–Minty cubes
are warped `n`-cubes with an ascending path through all `2ⁿ` corners. Yet LP itself is **in P**:
Khachiyan's ellipsoid method (1979, `O(n⁴L)`, impractical) and Karmarkar's interior-point method
(1984, `O(n³·⁵L)`, competitive). Simplex stays the default because it supports sensitivity
analysis and warm-starts — which matters inside B&B, where thousands of near-identical LPs get
re-solved. Interior-point methods are poor at that.

**So: LP ∈ P, IP is NP-hard. Integrality is the whole difficulty.**

## Standard templates — memorise these outright

Exams ask "which known problem is this?" directly. SS24 P6b: *"To which NP-hard problem learned
in the lecture does this correspond?"* → vertex cover. Two free points.

**Assignment problem** — `n` jobs to `n` machines, minimum cost:
```
min Σᵢ Σⱼ cᵢⱼ xᵢⱼ    s.t.  Σᵢ xᵢⱼ = 1 ∀j,   Σⱼ xᵢⱼ = 1 ∀i,   x ∈ {0,1}
```
Both a row constraint and a column constraint — every job gets one machine, every machine gets
one job. There are `n!` candidate assignments, yet it is **solvable in polynomial time**. It's
maximum matching in a weighted bipartite graph, a special case of the transportation problem.

*Bridge to E6:* its constraint matrix is **totally unimodular**, so the LP relaxation's corners
are already integral — just solve the LP. Being an IP does not make a problem hard; TU is
exactly the condition under which it isn't. (The Hungarian method itself has never been
examined; the *fact* that assignment is in P has.)

**Generalized Assignment (GAP)** — add capacities, and it becomes NP-hard:
```
min Σᵢ Σⱼ cᵢⱼ xᵢⱼ    s.t.  Σᵢ xᵢⱼ = 1 ∀j,   Σⱼ dᵢⱼ xᵢⱼ ≤ sᵢ ∀i,   x ∈ {0,1}
```

**0-1 Knapsack:**
```
max Σⱼ pⱼ xⱼ   s.t.  Σⱼ wⱼ xⱼ ≤ W,   x ∈ {0,1}
```
Lecture variants: multiple (`m` knapsacks), bounded (several copies per item), multiple-choice
(items in `k` classes, exactly one per class).

**Multiple Knapsack:**
```
max Σᵢ Σⱼ pⱼ xᵢⱼ   s.t.  Σⱼ wⱼ xᵢⱼ ≤ Wᵢ ∀i,   Σᵢ xᵢⱼ ≤ 1 ∀j
```
Note `≤ 1`, not `= 1`: items may be left behind.

**Bin Packing** — minimise bins used:
```
min Σᵢ yᵢ    s.t.  Σᵢ xᵢⱼ = 1 ∀j,   Σⱼ dⱼ xᵢⱼ ≤ s·yᵢ ∀i,   x, y ∈ {0,1}
```
Here `= 1`: everything must be packed. The second constraint is the capacity-linked-to-opening
pattern in its native habitat — capacity `s` only available if bin `i` is opened.
Multidimensional version (server consolidation): `Σⱼ u_{j,k,t} xᵢⱼ ≤ s_{i,k} yᵢ ∀i,k,t` —
resources `K` and time `T` become extra index dimensions.

**Set covering / partitioning / packing** — the same matrix `A` with `aᵢⱼ = 1` if set `j`
contains element `i`; only the relation changes:
```
covering:      min cᵀx,  Ax ≥ 1,  x binary      "cover every element at least once"
partitioning:  min cᵀx,  Ax = 1,  x binary      "exactly once"
packing:       max cᵀx,  Ax ≤ 1,  x binary      "at most once"
```

---

# Part 5 — The core difficulty: you cannot write "if"

Everything so far was direct. The hard exam points come from conditional requirements:

> "**If** more than 200 students are assigned to zone A, **then** at least 300 must go to zone B."

There is no "if" in linear algebra. **An inequality is always active** — you can't switch it on
and off. So you have to fake it.

## The fake: make a constraint say nothing

Look at what happens when you add a huge number to the right-hand side of a constraint:

```
X ≤ 200 + M·t          with t ∈ {0,1}
```

- **`t = 0`** → `X ≤ 200`. The constraint bites normally.
- **`t = 1`** → `X ≤ 200 + M`. If `M` is big enough that `200 + M` exceeds anything `X` could
  ever be, this restricts nothing. **The constraint has been switched off.**

That's it. That's big-M. A number large enough to make a row vacuous.

## Choosing M, with actual numbers

In the school problem there are 800 students, so `X` — students in zone A — can never exceed 800.

- Pick `M = 800`. Then `t = 1` gives `X ≤ 1000`. Since `X ≤ 800` anyway, no restriction. ✓
- Pick `M = 50`. Then `t = 1` gives `X ≤ 250`. That **still restricts** `X` — the model now
  forbids solutions that should be legal. **The model is wrong.** ✗

**Rule: M must be at least as large as the biggest amount the switched-off constraint could
need to allow.** Here that's `800 − 200 = 600`, so `M = 800` is safe.

Two things the examiners check:
1. **M is a constant, not a variable.** Never write `M` in the domain list.
2. **You justify its size.** One clause: *"M = 800 works, since there are only 800 students."*
   SS24's own solution says *"for M we can choose any fixed number greater or equal 4."*

Read M off another constraint whenever you can: `M = |J|`, `M = cᵢ`, `M = Σⱼ wⱼ`.

---

# Part 6 — Indicator variables and the two directions

This is the single most examinable thing in the topic. Read it twice.

## The setup

You want a binary `t` that is **1 exactly when `X > K`**. That phrase hides *two separate
requirements*, and each needs its own inequality.

```
(A)  X ≤ K + M·t        "if X > K then t must be 1"     — forces t UP
(B)  X ≥ (K+1)·t        "if t = 1 then X must be > K"   — forces t DOWN
```

## Check (A) with numbers

`K = 200`, `M = 800`. Suppose `X = 500`.
- Try `t = 0`: constraint reads `500 ≤ 200`. **Violated.** So `t = 0` is impossible.
- Therefore `t = 1` is forced. ✓ **(A) does its job: a large `X` forces `t` on.**

## Check (B) with numbers

`(B)` reads `X ≥ 201·t`. Suppose `t = 1`.
- Constraint reads `X ≥ 201`. So `t = 1` is only allowed when `X` really is above 200. ✓
- If `t = 0` it reads `X ≥ 0` — no restriction, correctly.

## Why you often need both — the failure modes

**(A) alone** permits `t = 1` while `X = 0`: the constraint reads `0 ≤ 1000`, satisfied. So a
solver could switch `t` on for free. Whether that matters depends on the objective — if `t`
being 1 *costs* something, the solver will never do it, and (A) alone is enough.

**(B) alone** permits `X = 800` with `t = 0`: reads `800 ≥ 0`, satisfied. So a huge `X` fails to
trigger `t`. Again — harmless only if the objective *rewards* `t = 1`.

## How to decide

| Wording in the question | Write |
|---|---|
| "indicates whether…", "if and only if", "⟺" | **both** (A) and (B) |
| "if more than K …, then …" | (A), plus the consequence keyed on `t` |
| you're not sure | **both** — you are never penalised for both |

## The `K+1` detail

`(B)` uses `(K+1)`, not `K`, because "more than 200" for a **whole number** of students means
"at least 201". This only works because `X` is integer.

For a **continuous** `X`, "`t = 1` if and only if `X > K`" is genuinely **impossible** to model
exactly — the set `{X > K}` isn't closed, so it isn't a polytope. Worth one sentence if a
question baits you with it.

---

# Part 7 — The answer template (this is literally the rubric)

Both the SS24 and SS25 official solutions are written in exactly this shape. Copy it:

```
We introduce z ∈ {0,1}:  z = 1  ⟺  <meaning, in a full sentence>

    <linking constraint(s) that tie z to reality>

Then we can formulate the constraint:

    <the actual requirement, keyed on z>     ∀ i ∈ <explicit range>
```

Three things score **independently**: the **meaning in words**, the **linking constraints**, the
**`∀` with an explicit range**. The exam prompt says it every year —

> *"You may introduce additional variables. If you do so, please also state their intuitive
> meaning in words."*

That sentence is a rubric line, not politeness.

## Which parts need an auxiliary variable

| Kind | Who supplies it | Example |
|---|---|---|
| **Decision** | The exam hands them to you | `x_{i,j}` = student `j` → school `i` |
| **Indicator** | **You invent these** | `t` = 1 if more than 100 students at school 1 |
| **Shorthand** | You, for readability | `X_i = Σⱼ x_{i,j}` |

**A sub-question worth more than 2 points is almost always asking you to invent an indicator.**
Use the point value as your hint.

---

# Part 8 — The patterns, ranked by exam frequency

The full catalogue is in [05-ip-modeling](05-ip-modeling.md). These are the ones that have
actually appeared on papers.

## Tier 1 — on literally every paper

```
Exactly one          Σ_{i∈I} x_{i,j} = 1                    ∀j ∈ J
At most once         Σ_{j∈J} x_{i,j} ≤ 1                    ∀i ∈ I
At most k            Σ_i z_i ≤ k
Capacity             Σ_{j∈J} x_{i,j} ≤ c_i                  ∀i ∈ I
Only-if / linking    x_{i,j} ≤ y_i                          ∀i ∈ I, j ∈ J
```

All five are Part 1 + Part 2 mechanics. If any looks unfamiliar, go back — don't memorise it.

Use the **disaggregated** linking form (`x_{i,j} ≤ y_i`) by default; it's stronger (Part 4).

**Don't merge sub-questions.** SS25 asked capacity in part (b) and linking in part (c). The
combined constraint `Σⱼ x_{i,j} ≤ cᵢ·yᵢ` covers both at once — but answer the part you were asked.

## Tier 2 — where the points are

**Implication between binaries.** `A ⇒ B` is `A ≤ B` (Part 3). Generalised to several premises:
```
"if W and R both hold, then P or C must hold"
    W_t + R_t − 1 ≤ P_t + C_t                ∀t

General shape:  Σ(premises) − (#premises − 1) ≤ Σ(conclusions)
```
Sanity check: if `W = R = 1` the left side is 1, forcing `P + C ≥ 1` — at least one conclusion.
If either premise is 0 the left side is ≤ 0 and nothing is forced. ✓

**Pairwise conflict** — SS25 E4d, built schools must be ≥ 5 km apart:
```
Direct, when d is a known number at modelling time:
    y_i + y_{i'} ≤ 1        ∀ i≠i' with d_{i,i'} < 5

Via an auxiliary, when d is a symbolic parameter (the official solution):
    1 + z_{i,i'} ≥ y_i + y_{i'}       ∀i≠i'      (both open ⇒ z = 1)
    d_{i,i'}·z_{i,i'} ≥ 5·z_{i,i'}    ∀i≠i'
```
Check the first auxiliary row: if `yᵢ = y_{i'} = 1` it reads `1 + z ≥ 2`, forcing `z = 1`. ✓

**Adjacency in a sequence** — SS24 P4d, consecutive stages within 200 km:
```
    x_{i,j} + x_{i',j+1} ≤ 1 + y_{i,i'}    ∀i,i' ∈ I, j ∈ J\{21}
    y_{i,i'}·d_{i,i'} ≤ 200                ∀i,i' ∈ I

one-liner alternative:
    (x_{i,j} + x_{i',j+1} − 1)·d_{i,i'} ≤ 200
```
Note `J\{21}` — stage 21 has no successor. **Truncated ranges are graded.** Same for lookbacks:
a 3-year history condition runs `∀t ∈ {4,…,15}`, not `∀t ∈ T`.

**Either-or (disjunction)** — at least one of two constraints must hold:
```
    f₁(x) ≤ b₁ + M·z
    f₂(x) ≤ b₂ + M·(1 − z)
```
`z = 0` switches off the second; `z = 1` switches off the first. Exactly one is off, so at least
one is on. ✓

**XOR (exactly one, "not in both")** — SS24 P4e:
```
"more than 3 mountain routes in either the first or the last 7 stages, not both"

    ΣΣ_{j=1..7}   m_i x_{i,j} ≤ 3 + M·z        ┐ z = 1 ⟺ first block exceeds 3
    ΣΣ_{j=1..7}   m_i x_{i,j} ≥ 4z             ┘   (both directions — it's an "iff")
    ΣΣ_{j=15..21} m_i x_{i,j} ≤ 3 + M(1−z)     ┐ mirrored with (1−z)
    ΣΣ_{j=15..21} m_i x_{i,j} ≥ 4(1−z)         ┘
```
The `z` / `1−z` mirror **is** the exclusivity — no separate XOR row needed. The two-binary
version with `z₁ + z₂ = 1` also scores. Note `mᵢ` is a 0/1 *parameter*, so `Σ mᵢ x_{i,j}`
counts only the mountain routes.

**Ratio / percentage.** Move everything to the left and clear denominators so every coefficient
is a constant:
```
"beer type w is at most 40% of all barrels sold"
    Σ_b w_b ≤ 0.4·Σ_b (w_b + h_b + s_b + a_b)
  → 0.6 Σ_b w_b − 0.4 Σ_b (h_b + s_b + a_b) ≤ 0
```

**Rolling window.** "At most 5 in any 7 consecutive days":
```
    Σ_{t=k}^{k+6} x_t ≤ 5        ∀k ∈ {1,…,T−6}
```
`k` is the window's start; the window covers `k` through `k+6`, which is 7 days. It must stop at
`T−6` or the window runs past the end. **The `∀k` and its explicit upper limit are the points.**

**Product linearisation.** Never leave `x_k·x_l` in a model — that isn't linear.
```
binary × binary:      Y ≤ x_k,  Y ≤ x_l,  Y ≥ x_k + x_l − 1
binary × continuous:  w ≤ a,  w ≤ M·z,  w ≥ a − M(1−z),  w ≥ 0
```
Check the binary case: if either `x` is 0, the first two rows force `Y = 0`. If both are 1, the
third reads `Y ≥ 1`, forcing `Y = 1`. ✓ In a **max** problem where `Y` is rewarded, `Y ≤ x_k`
and `Y ≤ x_l` suffice (the objective pushes `Y` up on its own). Where `Y` is penalised,
`Y ≥ x_k + x_l − 1` suffices. All three is always safe — and saying *which* direction the
objective handles earns the understanding point.

**Fixed charge / minimum lot size:**
```
    x ≤ M·y,  x ≥ q·y,  y ∈ {0,1}      "produce nothing, or at least q"
    objective: min f·y + c·x           f is paid once if any x > 0
```

**Startup detection:** `y_t ≥ x_t − x_{t−1}` for `t ≥ 2`. If the machine was off at `t−1`
(`x=0`) and is on at `t` (`x=1`), the right side is 1, forcing `y_t = 1`. In a min problem with
positive setup cost that inequality alone suffices — the objective pushes `y` down. **Say so**;
it earns the point.

## Tier 3 — objectives

**Weighted-sum scalarisation** — SS25 E4f. Two goals pulling opposite ways. Flip the sign of
whichever disagrees with your chosen sense:
```
"minimize construction cost while maximizing preference"
    min  Σ_i f_i y_i  −  Σ_i Σ_j s_{i,j} x_{i,j}
```
Minimising a negative preference term = maximising preference. State that weights are strictly
positive; their ratio encodes the trade-off ("twice as important" → weight 2 vs 1).

**Position-dependent coefficients** — SS24 P4a. Towns pay `pᵢ`, *double* for the first or last
stage:
```
    max  Σ_{i∈I} p_i · ( 2·x_{i,1} + Σ_{j=2}^{20} x_{i,j} + 2·x_{i,21} )
```
The middle sum runs `j = 2..20`, not `1..21`, because stages 1 and 21 are already counted at the
doubled rate. Splitting the index range correctly is the whole point of the part.

---

# Part 9 — Worked example: SS25 E4, start to finish

> 15 sites, capacity `cᵢ`, cost `fᵢ`. 800 students, preference `s_{i,j}`.
> Given: `yᵢ` = build site `i`, `x_{i,j}` = assign student `j` to school `i`.

**(a) Each student must be assigned to exactly one school.**

Draw the grid: schools down, students across. "Each student to one school" = each **column**
sums to 1. Summing a column means summing over `i`. The free index is `j`:
```
Σ_{i∈I} x_{i,j} = 1        ∀j ∈ J          (800 constraints)
```

**(b) Students at a school cannot exceed its capacity.**

Now it's a **row** sum — over `j`, free index `i`:
```
Σ_{j∈J} x_{i,j} ≤ c_i      ∀i ∈ I          (15 constraints)
```

**(c) A student may be assigned to a school only if the school is built.**

"Only if" = implication = `A ≤ B` (Part 3). Assignment implies built:
```
x_{i,j} ≤ y_i              ∀i ∈ I, j ∈ J   (12,000 constraints)
```
Check: if `yᵢ = 0` then every `x_{i,j} ≤ 0`, so nobody is assigned there. ✓ Disaggregated form —
stronger relaxation.

**(d) Built schools must be at least 5 km apart** (`d_{i,i'}` given).

Worth more points → auxiliary needed. We need "both built" as something we can talk about:
```
Introduce z_{i,i'} ∈ {0,1}:  z_{i,i'} = 1 if schools i and i' are both built.
    1 + z_{i,i'} ≥ y_i + y_{i'}          ∀i,i' ∈ I, i ≠ i'
    d_{i,i'}·z_{i,i'} ≥ 5·z_{i,i'}       ∀i,i' ∈ I, i ≠ i'
```
The second row is vacuous when `z = 0`, and reads `d_{i,i'} ≥ 5` when `z = 1`. So if the distance
is under 5, `z` can't be 1, so both can't be built. ✓

**(e) If more than 200 students go to zone A, at least 300 must go to zone B.**
(`Aᵢ ∈ {0,1}` marks zone-A sites.)

The threshold-indicator pattern from Part 6, then the consequence:
```
Introduce t ∈ {0,1}:  t = 1 if more than 200 students are assigned to zone-A schools.
    Σ_i Σ_j A_i x_{i,j} ≤ 200 + M·t          (A: forces t up)
    Σ_i Σ_j A_i x_{i,j} ≥ 201·t              (B: forces t down)
Then:
    Σ_i Σ_j (1−A_i) x_{i,j} ≥ 300·t
M = 800 works, since there are only 800 students.
```
Note `(1 − Aᵢ)` is the negation from Part 3 — it selects zone-**B** sites. And the final row is
vacuous when `t = 0`, correctly imposing nothing when the trigger hasn't fired.

**(f) A single objective: minimise cost, maximise preference.**
```
min  Σ_i f_i y_i − Σ_i Σ_j s_{i,j} x_{i,j}
```

---

# Part 10 — Traps and drills

## The six ways people lose points

1. **Missing `∀` range**, or the wrong one — `j ∈ J\{21}`, `t ∈ {4,…,15}`.
2. **Unnamed auxiliary variable.** The prompt demands words. No words, no points.
3. **M treated as a variable**, or no justification for its size.
4. **Only one direction of an indicator** when the wording said "iff".
5. **A product left non-linear.**
6. **Answering a different sub-question** than the one asked (merging b and c).

## If you're stuck in the exam

1. Which index is free? That tells you the `∀`.
2. Am I summing a row or a column? Draw the grid.
3. Is this conditional? If yes → invent a binary, name it in words, write both directions.
4. Enumerate the 0/1 cases and check against the English (Part 3).
5. Write *something* structured — partial credit is real on E4.

## Drill order — timeboxed at one minute per point

1. SS25 E4 *School Planning* (21 min)
2. SS24 P4 *Tour d'Allemagne* (22 min)
3. SS23 E2 *Ice cream production* (22 min)
4. SS21 A3 *Biergärten* (35 min) — stamina
5. retake-2021 A5 (32 min) — stamina

## End-of-day test — from blank paper

1. exactly-one assignment
2. capacity linked to an open/build decision
3. "only if" linking, disaggregated
4. pairwise mutual exclusion from a distance parameter
5. **"if A > k then B ≥ m"** — both indicator directions plus the consequence
6. two goals in one objective, with the sign flip explained

Number 5 decides whether you get 15/22 or 22/22.

## Three sentences worth points elsewhere on the paper

1. The LP relaxation of a max IP gives an **upper** bound; equal integer sets with a smaller
   relaxation region means a **stronger** formulation.
2. It is NP-hard to decide whether an IP is even **feasible**; SAT reduces to binary IP.
3. **TU + integral `b` ⇒ the LP relaxation's vertices are already integral** ⇒ the IP is solvable
   in polynomial time. That's why assignment and network flow are easy, and GAP and bin packing
   are not. → [08-total-unimodularity-and-matroids](08-total-unimodularity-and-matroids.md)
