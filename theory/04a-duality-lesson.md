# Duality — from scratch

Teaching companion to [04-duality](04-duality.md). That file is the reference; **this one
assumes you know nothing** and builds up to the exam question.

Exam slot **E3**, worth **6–19 points** (usually ~15). On every past paper, and the most
procedural question on the whole exam — it has the same three parts nearly every year.

---

# Part 0 — What the exam actually asks

SS25 E3, SS24 P2 and SS23 E1 share the same three parts:

```
a)  Derive the dual linear program (D) corresponding to (P).
b)  State the primal and the dual complementary slackness conditions.
c)  Show that the primal solution x = (…) is NOT optimal.
```

Part (a) is a mechanical transformation. Part (b) is copying a formula. Part (c) is one fixed
procedure. **None of it requires solving an LP.** That's why this is the best points-per-hour
question on the paper.

---

# Part 1 — Where the dual comes from

Most people memorise the dual as a table of rules. Don't — derive it once and the rules become
obvious.

## The question that creates the dual

Take a small LP:
```
(P)  max  3x₁ + 2x₂
     s.t.  x₁ +  x₂ ≤ 4        (constraint 1)
           x₁ + 3x₂ ≤ 6        (constraint 2)
           x₁, x₂ ≥ 0
```

Suppose you can't solve it, but someone asks: **how large could the objective possibly be?**

You can *prove* upper bounds using the constraints themselves. Constraint 1 says
`x₁ + x₂ ≤ 4`. Multiply it by 3:
```
3x₁ + 3x₂ ≤ 12
```
Since `x₂ ≥ 0`, we have `3x₁ + 2x₂ ≤ 3x₁ + 3x₂ ≤ 12`. **So the objective can never exceed 12.**
No solving required.

Can we do better? Try mixing both constraints.

## The general recipe

Give constraint 1 a weight `y₁ ≥ 0` and constraint 2 a weight `y₂ ≥ 0`, then add them:
```
y₁·(x₁ + x₂)  +  y₂·(x₁ + 3x₂)  ≤  4y₁ + 6y₂

collect terms:   (y₁ + y₂)·x₁ + (y₁ + 3y₂)·x₂  ≤  4y₁ + 6y₂
```

**The weights must be non-negative** — multiplying an inequality by a negative number flips it,
and the whole argument collapses. *That is where `y ≥ 0` comes from.*

Now, when does this bound the objective `3x₁ + 2x₂`? If the coefficient on each `xⱼ` is **at
least** the objective's coefficient:
```
y₁ +  y₂ ≥ 3        (coefficient on x₁ must dominate 3)
y₁ + 3y₂ ≥ 2        (coefficient on x₂ must dominate 2)
```
then — because `x₁, x₂ ≥ 0` — we can chain:
```
3x₁ + 2x₂  ≤  (y₁+y₂)x₁ + (y₁+3y₂)x₂  ≤  4y₁ + 6y₂
```

So **`4y₁ + 6y₂` is a valid upper bound** for any weights satisfying those two inequalities.

## The dual is "find the best such bound"

You want the *smallest* upper bound you can prove. So:
```
(D)  min  4y₁ + 6y₂
     s.t.  y₁ +  y₂ ≥ 3
           y₁ + 3y₂ ≥ 2
           y₁, y₂ ≥ 0
```

**That is the dual.** Not a definition to memorise — the answer to "what's the tightest bound
provable from the constraints?"

## Everything now follows

| Feature of the dual | Reason |
|---|---|
| `max` becomes `min` | you want the *tightest* upper bound |
| one dual variable per primal **constraint** | each constraint gets a weight |
| one dual constraint per primal **variable** | each `xⱼ` needs its coefficient dominated |
| `b` becomes the objective | the bound is `Σ bᵢyᵢ` |
| `c` becomes the right-hand side | you must dominate each `cⱼ` |
| the matrix transposes | dual constraint `j` uses **column** `j` of `A` |
| `y ≥ 0` | negative weights would flip the inequalities |
| dual constraints are `≥` | coefficients must *dominate* the objective |
| **weak duality** `cᵀx ≤ bᵀy` | it's the chain above — built in by construction |

Note also: **the dual of the dual is the primal.** "Primal" and "dual" are labels, not
properties.

---

# Part 2 — Deriving the dual mechanically

In the exam you won't re-derive from scratch. Use the recipe:

1. Switch `max` ↔ `min`.
2. One dual variable per primal constraint. **Sign restrictions (`x ≥ 0`) do not count as
   constraints.**
3. One dual constraint per primal variable.
4. Transpose `A`: dual constraint `j` is built from **column** `j` of the primal.
5. Swap `c` and `b`.

## The transpose, concretely

This is the step people fumble. Primal constraints written as rows:
```
        x₁    x₂    x₃
 (1)     1     2     1    ≤ 10
 (2)     2    −1     2    ≥  4
 (3)     1     1    −1    =  3
```

Dual constraint `j` reads **down column `j`**:
```
dual constraint 1  (from the x₁ column):   1y₁ + 2y₂ + 1y₃  ? c₁
dual constraint 2  (from the x₂ column):   2y₁ − 1y₂ + 1y₃  ? c₂
dual constraint 3  (from the x₃ column):   1y₁ + 2y₂ − 1y₃  ? c₃
```

**Read down, not across.** If you find yourself copying a primal row into a dual row, you've
made the classic error.

---

# Part 3 — The SOB table (for mixed constraint types)

> **Provenance.** SOB is taught in the **central exercise**
> (`central exercises/05-linear-programming-duality/ce-04-slides.pdf`, slides 4–5), where it is
> called the "SOB Method". It does **not** appear anywhere in `lecture-07-duality.pdf`. So it is
> legitimate course material and safe to use — but the method, not the name, is what earns
> marks. Show the classification and the translation; don't just write "by SOB".

Part 1's derivation assumed everything was `≤` and `x ≥ 0`. Real exam problems mix `≤`, `≥`,
`=`, and free variables. The SOB table handles all cases.

Classify each variable and each constraint as **S**ensible, **O**dd, or **B**izarre:

| | context | **S** (sensible) | **O** (odd) | **B** (bizarre) |
|---|---|---|---|---|
| **Variable** | any | `x ≥ 0` | `x` free | `x ≤ 0` |
| **Constraint** | in a **max** problem | `≤` | `=` | `≥` |
| **Constraint** | in a **min** problem | `≥` | `=` | `≤` |

Then translate **S→S, O→O, B→B**:

| type | primal **constraint** → dual **variable** | primal **variable** → dual **constraint** |
|---|---|---|
| **S** | `y ≥ 0` | sensible direction |
| **O** | `y` free | `=` |
| **B** | `y ≤ 0` | bizarre direction |

"Sensible direction" in the dual: `≥` when the dual is a **min**, `≤` when it's a **max**.
Bizarre is the opposite.

## Why the "bizarre" sign makes sense

A `≥` row in a max problem is backwards for the bounding argument — to use it as an upper bound
you'd have to flip it, which means a **negative** weight. Hence `y ≤ 0`. The table isn't
arbitrary; it's Part 1 extended.

**Memorise this table cold.** Reproduce it from blank paper before you touch a past paper.

---

# Part 4 — The two duality theorems

**Weak duality.** For *any* feasible `x` in (P) and *any* feasible `y` in (D):
```
cᵀx  ≤  bᵀy
```
Every dual-feasible point gives an upper bound on every primal-feasible point. This is just the
chain from Part 1.

**Strong duality.** If either problem has a finite optimum, so does the other, and:
```
cᵀx*  =  bᵀy*
```
The best provable bound is exactly the true optimum — no gap. (Unlike IPs, where a gap is normal.)

## The 3×3 outcome table

Weak duality restricts which combinations can occur:

| | **D finite** | **D unbounded** | **D infeasible** |
|---|---|---|---|
| **P finite** | ✓ possible | impossible | impossible |
| **P unbounded** | impossible | impossible | ✓ possible |
| **P infeasible** | impossible | ✓ possible | ✓ possible |

Two facts carry it:
- **P unbounded ⇒ D infeasible.** If the primal grows without limit, no finite upper bound can
  exist, so nothing is dual-feasible.
- **P infeasible ⇒ D is unbounded *or* infeasible.** Not necessarily unbounded — the
  both-infeasible corner is real.

That last point is a standard multiple-choice trap (it was SS25 1a).

## Classifying without solving

A recurring sub-question: *"Is (D) unbounded, infeasible, or does it have a finite optimum —
without solving it?"* — a recurring sub-question

```
1. Exhibit one feasible point of (P).  ⇒ by weak duality, (D) cannot be unbounded.
2. Exhibit one feasible point of (D).  ⇒ (D) is not infeasible.
3. Both feasible ⇒ by strong duality, (D) has a finite optimum.
```

Usually `x = 0` or `y = 0` is feasible and the whole answer is two lines.

---

# Part 5 — Complementary slackness

## Where it comes from

At optimality, strong duality says the two ends of Part 1's chain are **equal**:
```
cᵀx*   ≤   (Aᵀy*)ᵀx*   ≤   bᵀy*
 └──────── equal at optimality ────────┘
```
If the ends are equal, **both `≤` signs must actually be `=`**. Look at what each requires:

**First `≤` becomes `=`:** the gap is `Σⱼ ((Aᵀy)ⱼ − cⱼ)·xⱼ`. Every term is `≥ 0`, so the sum is
zero only if **every term is zero**:
```
((Aᵀy*)ⱼ − cⱼ) · xⱼ*  =  0        for each j
```
That is: **for each variable, either `xⱼ = 0` or dual constraint `j` is tight.**

**Second `≤` becomes `=`:** same argument on the other side:
```
((Ax*)ᵢ − bᵢ) · yᵢ*  =  0         for each i
```
That is: **for each constraint, either `yᵢ = 0` or primal constraint `i` is tight.**

## The two statements in plain English

> **If a primal constraint is not tight (slack), its dual variable is zero.**
> **If a primal variable is non-zero, its dual constraint is tight.**

And symmetrically in the other direction. Economically: a resource you aren't fully using has a
shadow price of zero — an extra unit is worth nothing.

## What "state the CS conditions" wants (part b)

Just write them out per row. For SS25's (P):
```
Primal CS conditions:                    Dual CS conditions:
  (x₁ + 2x₂ + x₃ − 10)·y₁ = 0              (y₁ + 2y₂ + y₃ − 3)·x₁ = 0
  (2x₁ − x₂ + 2x₃ − 4)·y₂ = 0              (2y₁ − y₂ + y₃ − 2)·x₂ = 0
  (x₁ + x₂ − x₃ − 3)·y₃ = 0                (y₁ + 2y₂ − y₃ − 1)·x₃ = 0
```
Each is `(constraint expression − RHS) × (the matching variable) = 0`. Mechanical — free points.

---

# Part 6 — Part (c): the refutation procedure

The highest-value part, and it's one fixed procedure. You are given a point `x*` and asked to
show it is **not** optimal — *without solving the LP*.

**The logic:** if `x*` were optimal, strong duality guarantees a dual-feasible `y` satisfying
complementary slackness. So construct the `y` that CS forces, then show it can't be
dual-feasible. Contradiction ⇒ `x*` is not optimal.

```
Step 1  Check x* is primal feasible. If it isn't, stop — you're done immediately.

Step 2  For each primal constraint that is SLACK, CS forces its yᵢ = 0.

Step 3  For each xⱼ* ≠ 0, CS forces dual constraint j to be TIGHT (an equation).
        Substitute the zeros from Step 2 and solve for the remaining yᵢ.

Step 4  Check the recovered y against the dual's OTHER constraints and sign restrictions.
          • all satisfied  → x* IS optimal, and y is the dual optimum
          • any violation  → x* is NOT optimal
```

Step 4 is where the answer lives. Usually the recovered `y` violates a sign restriction
(`y₂ ≤ 0` but you got `y₂ = 3`) or one of the dual constraints you didn't use.

**If you're given a dual point instead**, swap every role: check dual feasibility, slack dual
constraints force `xⱼ = 0`, non-zero `yᵢ` force primal row `i` tight, solve, check primal.

---

# Part 7 — Worked example: SS25 E3, all three parts

```
(P)  Maximize   3x₁ + 2x₂ + x₃
     subject to  x₁ + 2x₂ +  x₃ ≤ 10
                2x₁ −  x₂ + 2x₃ ≥  4
                 x₁ +  x₂ −  x₃  =  3
                 x₁, x₂, x₃ ≥ 0
```

## (a) Derive the dual

Three constraints → three dual variables `y₁, y₂, y₃`. Three variables → three dual constraints.
It's a max problem, so use the max row of the SOB table:

| primal | type | → dual |
|---|---|---|
| constraint 1: `≤` | S | `y₁ ≥ 0` |
| constraint 2: `≥` | B | `y₂ ≤ 0` |
| constraint 3: `=` | O | `y₃` free |
| `x₁, x₂, x₃ ≥ 0` | S | dual constraints get the sensible direction = `≥` (dual is min) |

Now read **down the columns** for the left-hand sides, and `c` becomes the RHS:
```
(D)  Minimize   10y₁ + 4y₂ + 3y₃
     subject to  y₁ + 2y₂ + y₃ ≥ 3          ← x₁ column
                2y₁ −  y₂ + y₃ ≥ 2          ← x₂ column
                 y₁ + 2y₂ − y₃ ≥ 1          ← x₃ column
                 y₁ ≥ 0,  y₂ ≤ 0,  y₃ ∈ ℝ
```

## (b) State the CS conditions

As in Part 5 — six equations, mechanical.

## (c) Show `x = (2,2,1)ᵀ` is not optimal

**Step 1 — primal feasibility.**
```
 2 + 2·2 + 1  =  7  ≤ 10     ✓  (SLACK — strictly less than 10)
2·2 − 2 + 2·1 =  4  ≥  4     ✓  (tight)
 2 + 2 − 1    =  3  =  3     ✓
 2, 2, 1 ≥ 0                 ✓
```
Feasible. So we continue.

**Step 2 — slack constraints force duals to zero.**
Constraint 1 is slack (`7 < 10`), so CS forces **`y₁ = 0`**.

**Step 3 — non-zero variables force dual constraints tight.**
`x₂ = 2 ≠ 0` and `x₃ = 1 ≠ 0`, so dual constraints 2 and 3 must hold with equality:
```
2y₁ −  y₂ + y₃ = 2
 y₁ + 2y₂ − y₃ = 1
```
Substitute `y₁ = 0`:
```
−y₂ +  y₃ = 2
 2y₂ − y₃ = 1
```
Add them: `y₂ = 3`. Then `y₃ = 2 + y₂ = 5`. So CS forces **`y = (0, 3, 5)ᵀ`**.

**Step 4 — check dual feasibility.**
The dual requires `y₂ ≤ 0` (constraint 2 of the primal was a `≥` row in a max problem —
bizarre). But we derived `y₂ = 3 > 0`. **Contradiction.**

> Therefore no dual-feasible point satisfies complementary slackness with `x = (2,2,1)ᵀ`, so by
> the complementary slackness theorem `x` is **not optimal**. ∎

Notice: the LP was never solved.

---

# Part 8 — Traps and drills

## Where points are lost

1. **Counting sign restrictions as constraints.** `x ≥ 0` does **not** get a dual variable.
2. **Reading rows instead of columns** when building dual constraints.
3. **Wrong sign on a `≥` row in a max problem** — that's bizarre, so `y ≤ 0`, not `y ≥ 0`.
4. **Saying "P infeasible ⇒ D unbounded".** It's "unbounded *or* infeasible".
5. **Solving the LP in part (c).** You don't need to, and you'll run out of time.
6. **Stopping at Step 3** without checking dual feasibility — Step 4 *is* the answer.

## Before any past paper

Reproduce from blank paper:
- the SOB table, both halves
- both CS statements, in words and in formulas
- the 3×3 outcome table

## Warm-up ladder (untimed)

1. `D4.1` *Dual Problem* — `[DRILL]` mechanical dualisation, 2–3 reps until automatic
2. `T4.2` *Duality* — `[DRILL]` mixed constraint types and free variables
3. `T4.1` *Duality & Complementary Slackness* — `[EXAM]` the exam's exact shape
4. `S4.2` *Duality & Complementary Slackness* — `[EXAM]` second rep
5. `D4.2` *Primal-Dual* — `[EXAM]` extra if the certificate still feels shaky
6. `T4.3` Rock-Paper-Scissors, `D4.3` Transportation — `[CONCEPT]`, read only

Sheet 4 is `exercises/05-linear-programming-duality/sheet-04-exercises.pdf`; the self-study
section (S4.x) is in the second half of the same file.

## Then the papers (timed, one minute per point)

SS25 E3 (15) · SS24 P2 (16) · SS23 E1 (19)

## Facts that also earn points elsewhere

- `yᵀ = c_Bᵀ B⁻¹` — the optimal duals **are** the shadow prices, and they sit in Row 0 of the
  final simplex tableau under the slack columns. This connects E3 to Wednesday's E2.
  → [03-revised-simplex-and-sensitivity](03-revised-simplex-and-sensitivity.md)
- Weak duality is what makes the **LP relaxation bound** in branch & bound valid.
  → [06a-branch-and-bound-lesson](06a-branch-and-bound-lesson.md)
