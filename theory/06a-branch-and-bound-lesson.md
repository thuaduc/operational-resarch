# Branch and Bound — from scratch

Teaching companion to [06-branch-and-bound-and-cuts](06-branch-and-bound-and-cuts.md). That
file is the reference; **this one assumes you know nothing** and builds up to the exam question.

Exam slot **E5**, worth **13–16 points**. On every past paper. Pure mechanics — no proofs, no
modelling, no creativity. The best points-per-hour question after duality.

---

# Part 0 — What the exam actually asks

Two formats. The first is far more common:

**Classic — solve it graphically** (SS21 A4, SS23 E3, SS24 P3). A 2-variable IP, and **the paper
hands you a plot** of the LP relaxation with the objective line already drawn. Their wording:

> SS24 P3: *"Note: In the following coordinate systems, the feasible set of the LP relaxation of
> (P) and the objective function are shown. You can use them to solve the subproblems
> graphically."*
>
> SS23 E3: *"In the following coordinate system, the feasible region of the LP relaxation of (P)
> as well as the objective function is plotted. You can use this to solve the subproblems."*

The task is always the same sentence: *"Solve the integer linear program (P) using the
branch-and-bound method. Specify the optimal values for x₁ and x₂, as well as the optimal value
of the objective function."*

**Reconstruction** (SS25 E5). No plot — instead the tree is drawn but **ink-blotted**, and you
get a table of every LP relaxation considered, decoys included. You recover which node is which,
give the optimum, and state the FIFO and LIFO exploration orders.

## What this means for your prep

| | Appears in | Priority |
|---|---|---|
| Solving subproblems **graphically** | SS21, SS23, SS24 | **highest** — three of four papers |
| Pruning rules, correct direction | all four | **highest** |
| Tree reconstruction from a table | SS25 only | high — it's the most recent |
| FIFO / LIFO ordering | **SS25 only** | moderate — cheap to learn, rarely asked |

**You never run simplex inside this question.** The problems are deliberately two-dimensional so
that every subproblem is a picture. Bring your ruler — it's on the allowed list for exactly this.

---

# Part 1 — Why we need this at all

An IP's feasible set is the **lattice points** inside a polyhedron. There are no corners to walk
between, so simplex doesn't apply. And you cannot enumerate — 50 binary variables is 2⁵⁰ ≈ 10¹⁵
candidates.

Branch & bound is the way out. Two ideas:

1. **Bound.** Solve the easy relaxed problem to learn how good things *could* get.
2. **Branch.** Split the problem into smaller pieces, and throw away whole pieces as soon as the
   bound proves nothing good can be hiding in them.

The throwing-away is the point. You never enumerate; you prove entire regions worthless.

---

# Part 2 — The bound

**LP relaxation:** take the IP and delete the integrality requirements (`x ∈ ℕ₀` becomes
`x ≥ 0`). What's left is an ordinary LP you can solve.

Because you **removed restrictions**, the relaxation has **more feasible points than the IP** 
—> every integer solution is still allowed, plus fractional ones. More room can only help, so:

```
maximisation:   OPT(IP)  ≤  Z_LP        the relaxation is an UPPER bound
minimisation:   OPT(IP)  ≥  Z_LP        the relaxation is a LOWER bound
```

**This is the single most important fact in the topic.** It's what lets you prune.

## The rounding refinement

If all objective coefficients are integers and all variables are integers, the objective value
must itself be an integer. So for a max problem you can strengthen:

```
OPT(IP)  ≤  ⌊Z_LP⌋
```

`Z_LP = 14.25` with integer coefficients means `OPT(IP) ≤ 14`. Occasionally worth a point.

---

# Part 3 — Branching

The relaxation gives a fractional answer, say `x₁ = 2.25`. That's not a solution to the IP. So
**split on that variable**:

```
      x₁ = 2.25  is fractional
             │
      ┌──────┴──────┐
   x₁ ≤ 2        x₁ ≥ 3
```

In general, for `xᵢ = f` fractional:
```
child 1:   xᵢ ≤ ⌊f⌋        (round down)
child 2:   xᵢ ≥ ⌈f⌉        (round up)
```

## Why this is valid — the key insight

**No integer point is lost.** There is no integer between 2 and 3, so every integer solution
satisfies either `x₁ ≤ 2` or `x₁ ≥ 3`. The two children cover everything the parent covered.

**But the fractional optimum is destroyed.** `x₁ = 2.25` satisfies neither branch. So each child's
relaxation must return something different — and weakly worse.

That's the engine: you keep cutting away fractional optima without ever losing an integer point,
so eventually a relaxation comes back integral.

**Each child inherits every constraint from its ancestors.** A node three levels down carries
three branching constraints plus the original LP. This is what the exam's "constraints added"
table records.

---

# Part 4 — The incumbent and the three pruning rules

## The incumbent `Z*`

The value of the **best integral solution found so far**. Starts at `−∞` for a max problem
(`+∞` for min) because you haven't found one yet.

For a max problem, `Z*` is a **lower** bound on the true optimum — you have an actual solution
achieving it. Combined with the relaxation upper bound, you're always bracketing:

```
Z*   ≤   OPT(IP)   ≤   Z'          (Z' = best bound among still-open nodes)
```

When those meet, you're done and can prove it.

## The three ways to close a node

| # | Rule | Maximisation | Minimisation |
|---|---|---|---|
| 1 | **Integrality** | LP solution is integral → it's a candidate; update `Z*` if `Z > Z*` | same; update if `Z < Z*` |
| 2 | **Infeasibility** | LP relaxation has no feasible point | same |
| 3 | **Bound** | `Z_node ≤ Z*` | `Z_node ≥ Z*` |

**Rule 3 is the one that matters** and the one whose direction people get wrong.

The reasoning for max: `Z_node` is the *best possible* value anywhere in that node's subtree
(it's a relaxation bound). If even that optimistic value doesn't beat what you already have,
nothing down there can help. Close it unexamined.

**For a min problem every comparison flips.** Write down which you're in before you start —
losing the direction costs the whole question.

Note rule 1 also closes the node: an integral relaxation solution is the best that subtree can
do, so there's nothing left to explore.

**Ties.** `Z_node = Z*` still prunes — you already have a solution that good. Say so explicitly
if you prune on a tie; it shows you know the rule.

---

# Part 5 — Solving a node on the plot

This is the mechanical skill three of the four papers actually test. Every subproblem is the
original feasible region plus the branch constraints accumulated down the path — and every
branch constraint is a **straight vertical or horizontal line**.

```
1. Start from the plotted LP relaxation. It's already drawn for you.

2. Draw each branch constraint on the path to this node:
       x₁ ≤ 2   → vertical line at x₁ = 2, keep the LEFT side
       x₁ ≥ 3   → vertical line at x₁ = 3, keep the RIGHT side
       x₂ ≤ 1   → horizontal line at x₂ = 1, keep BELOW
       x₂ ≥ 2   → horizontal line at x₂ = 2, keep ABOVE

3. Constraints ACCUMULATE. A depth-3 node carries three lines, all still in force.
   Shade what survives all of them.

4. Empty region?  → prune by infeasibility. No arithmetic needed at all.

5. Otherwise slide the objective line in the improving direction until it LAST
   touches the shaded region. Read the vertex off, and compute Z from it.
```

Two things make this fast in the exam:

- **Infeasible nodes are free.** You see the empty region instantly; there's nothing to compute.
  In SS25's tree two of the six nodes were infeasible.
- **The optimum is always at a vertex**, and after branching the vertices are usually
  intersections of a branch line with an original constraint — easy to read off, or to solve as
  a 2×2 system if the picture is ambiguous.

## Which variable to branch on

When more than one variable is fractional, the choice is yours. Only SS25 states a rule:

> *"whenever a branching choice was ambiguous, he chose to branch on the first variable"*

The classic papers say nothing, so **any choice is acceptable — but state which you made and
stay consistent.** Undocumented choices lose marks; unconventional ones don't.

---

# Part 6 — A complete worked tree

Use SS25's problem:
```
max  3x₁ + 2x₂
s.t. 2.4x₁ + x₂ ≤ 9.15
       x₁ + x₂ ≤ 6
       x₁, x₂ ∈ ℕ₀
```

**Node P₀ — the root.** Solve the relaxation: `x = (2.25, 3.75)`, `Z₀ = 14.25`.
Fractional. Incumbent `Z* = −∞`. We know `OPT ≤ 14.25`, so in fact `OPT ≤ 14`.

Branch on `x₁ = 2.25`:

**Node P₁ — add `x₁ ≤ 2`.** Relaxation: `x = (2.00, 4.00)`, `Z = 14.00`.
**Integral!** Rule 1 fires. Update the incumbent: `Z* = 14`, with `x = (2,4)`. Node closed.

**Node P₂ — add `x₁ ≥ 3`.** Relaxation: `x = (3.00, 1.95)`, `Z = 12.90`.
Fractional — but check rule 3 first. Is `12.90 ≤ 14`? **Yes.** Prune by bound.

Nothing in that entire subtree can beat 14, so we never explore it. Node closed.

**No open nodes remain.** Return the incumbent:
```
x* = (2, 4),   Z* = 14
```

Three nodes. That's the whole method — and notice how the bound did the work: an entire half of
the tree was dismissed with one comparison.

---

# Part 7 — FIFO vs LIFO

The rules above say *whether* to close a node. They don't say **which open node to pick next**.
That's the node-selection strategy.

> **How much this matters.** Only **SS25** has ever asked for it. SS21, SS23 and SS24 don't
> mention FIFO or LIFO at all — in those you explore in whatever order you like. So this is
> worth about twenty minutes, not an afternoon. Learn it because SS25 is the most recent paper
> and it's cheap, not because it's the core of the topic.

| | **FIFO** | **LIFO** |
|---|---|---|
| Also called | breadth-first | depth-first |
| Structure | **queue** — take the oldest | **stack** — take the newest |
| Behaviour | finish a whole level before descending | dive to the bottom, then backtrack |
| First incumbent | may come late | found quickly |
| Warm start | re-solve from scratch | reoptimise with dual simplex |

**Why LIFO is the practical default:** diving finds an integral solution fast, which gives you a
real incumbent early, which makes rule 3 start pruning sooner. FIFO explores a lot of nodes
before it has any incumbent to prune with.

## Reading the order off a tree

1. Start at `P₀`.
2. **FIFO** — process level by level, left to right.
3. **LIFO** — always continue with the most recently created node; when a node closes, backtrack
   to the deepest unexplored sibling.
4. **Stop as soon as the incumbent is confirmed optimal** — i.e. every remaining open node is
   prunable. The exam says "stopped as soon as the optimal solution was confirmed", so an early
   STOP is expected and rewarded.

---

# Part 8 — Reconstructing an obscured tree (the SS25 format)

You get a tree with blank boxes and a table of candidate subproblems including decoys. Procedure:

```
1. P₀ is the row with "–" in the constraints column — no constraints added.
2. Read the parent's fractional variable and value; the two edge labels must be
   xᵢ ≤ ⌊f⌋ and xᵢ ≥ ⌈f⌉.
3. A node's constraint set = its parent's set PLUS its own edge label.
   Find the table row matching that exact set.
4. Sanity checks:
     • a child's Z is never better than its parent's
     • contradictory constraints ⇒ that row says "Infeasible"
     • decoy rows have constraint sets that don't sit anywhere on the drawn tree
```

The exam also states the tie-breaking convention — SS25: *"whenever a branching choice was
ambiguous, he chose to branch on the first variable"* and *"he assigned greater-than-or-equal
constraints to right-hand nodes"*. **Read those sentences.** They determine which child is which.

---

# Part 9 — Worked example: SS25 E5, all four parts

Given: `P₀: x = (2.25, 3.75)`, `Z₀ = 14.25`, branch on `x₁`. And `P₃: x = (3.4, 1)`,
`Z₃ = 12.19`, branch on `x₁`. Edges `P₂→P₃` is `x₂ ≤ 1`, `P₂→P₄` is `x₂ ≥ 2`.

Relevant table rows:
```
P_A  –                    2.25  3.75  14.25
P_B  x₁ ≤ 2               2.00  4.00  14.00
P_C  x₁ ≥ 3               3.00  1.95  12.90
P_H  x₁ = 3, x₂ ≤ 1       3.00  1.00  11.00
P_J  x₁ ≥ 3, x₂ ≤ 1       3.40  1.00  12.19
P_L  x₁ ≥ 3, x₂ ≥ 2             Infeasible
P_M  x₁ ≥ 4, x₂ ≤ 1             Infeasible
```

## (a) The missing edge constraints

`P₀` branches on `x₁ = 2.25` → `⌊2.25⌋ = 2`, `⌈2.25⌉ = 3`. With `≥` on the right:
```
C₁ (P₀→P₁):  x₁ ≤ 2
C₂ (P₀→P₂):  x₁ ≥ 3
```
`P₃` branches on `x₁ = 3.4` → `⌊3.4⌋ = 3`, `⌈3.4⌉ = 4`:
```
C₅ (P₃→P₅):  x₁ ≤ 3    (equivalently x₁ = 3, since P₃ already carries x₁ ≥ 3)
C₆ (P₃→P₆):  x₁ ≥ 4
```

## (b) Match the nodes

Accumulate constraints down each path and look them up:
```
P₁ = {x₁ ≤ 2}                       → P_B
P₂ = {x₁ ≥ 3}                       → P_C
P₄ = {x₁ ≥ 3, x₂ ≥ 2}               → P_L   (Infeasible)
P₅ = {x₁ ≥ 3, x₂ ≤ 1, x₁ ≤ 3}       → P_H   (listed as x₁ = 3, x₂ ≤ 1)
P₆ = {x₁ ≥ 3, x₂ ≤ 1, x₁ ≥ 4}       → P_M   (Infeasible)
```
`P₅` is the one that catches people: `x₁ ≥ 3` **and** `x₁ ≤ 3` collapses to `x₁ = 3`, which is
how the table writes it.

## (c) The optimal integer solution

`P₁` returns `(2, 4)` with `Z = 14.00`, integral. Every other node is either infeasible or has a
bound below 14. So:
```
x* = (2, 4),   Z = 14
```

## (d) Exploration order

**FIFO** — process `P₀`, then the level below in order:
```
P₀, P₁, P₂, STOP
```
Why STOP there: `P₁` is integral with `Z = 14`, which exceeds `P₂`'s bound of 12.90. Since
12.90 bounds everything in `P₂`'s subtree, the optimum is already confirmed. Nothing else needs
solving.

**LIFO** — dive, taking the newest node each time, backtracking on closure:
```
P₀, P₂, P₄, P₃, P₆, P₅, P₁
```
Trace it: from `P₀` push `P₁`, `P₂`; take `P₂` (newest). `P₂` is fractional → push `P₃`, `P₄`;
take `P₄` (infeasible, closed); take `P₃`, fractional → push `P₅`, `P₆`; take `P₆` (infeasible);
take `P₅` → integral, `Z = 11`, incumbent `Z* = 11`; finally `P₁` → integral, `Z = 14 > 11`,
incumbent updated to 14. Stack empty.

Note LIFO here visits **every** node and FIFO visits three. That's not typical — it's an
artefact of the good solution sitting on the left branch. Worth a sentence if you have room.

---

# Part 10 — Gomory cuts, briefly

Cuts are the *other* way to handle fractional relaxations, and they show up as multiple-choice
items rather than as a full question. What you need:

- A **valid inequality** holds for every integer-feasible point. Adding one never removes an
  integer solution.
- A **cutting plane** is a valid inequality that is *violated by the current fractional LP
  optimum* — so it slices that vertex off while keeping all integer points.
- The **ideal formulation** is the convex hull of the integer feasible points. If you had it,
  plain simplex would solve the IP, because all its vertices are integral.
- Cuts and B&B **combine** (branch & cut) and the combination is faster than either alone.

The MC trap: *"cutting planes remove integer solutions from the LP relaxation"* — **false**, by
definition they never do.

Deriving a Gomory cut is in [06](06-branch-and-bound-and-cuts.md) §Procedures. It has never been
a full exam question; don't spend the afternoon on it.

---

# Part 11 — Traps and drills

## Where points are lost

1. **Pruning in the wrong direction.** Write "MAX" or "MIN" at the top of your answer and check
   rule 3 against it. SS24 P3 is a **minimisation** — don't get caught.
2. **Forgetting that constraints accumulate.** A node carries every ancestor's branch, and on
   the plot that means every ancestor's line is still cutting.
3. **Branching on an integer-valued variable.** Only branch on a *fractional* one.
4. **Not saying which variable you branched on** when several were fractional. Any choice is
   fine; an undocumented one is not.
5. **Missing the early STOP** in the SS25 format. "Stopped as soon as the optimal solution was
   confirmed" — running to the end loses the point.
6. **Ignoring the stated tie-break convention** for which child goes left.
7. **Not updating the incumbent** when an integral solution beats it.

## The answer format that earns full marks

For **every** node, write four things:
```
node │ constraint added │ vertex read off the plot │ Z │ which rule closed it
```
That table *is* the documented approach. A correct final answer with no node record scores badly;
a fully recorded tree with one arithmetic slip scores well.

## Before any past paper

From blank paper:
- the three pruning rules, **for max and for min**
- why `xᵢ ≤ ⌊f⌋ | xᵢ ≥ ⌈f⌉` loses no integer point
- FIFO and LIFO order on a 7-node tree *(SS25 format only — 20 minutes, no more)*

## Warm-up ladder (untimed)

1. `D6.1` *Branch-and-Bound* — `[EXAM]` a full worked tree. Do this first and slowly; it's the
   template for everything else.
2. `T6.1` *Knapsack Branch-and-Bound* — `[EXAM]` B&B on a knapsack, so the relaxation is solved
   by the greedy ratio rule rather than graphically. Useful second angle.
3. `S6.1` *Branch-and-Bound* — `[EXAM]` third rep. Stop here if pruning feels solid.
4. `T6.3` *Staff Scheduling* — `[DRILL]` an IP model; doubles as Day 1 revision.
5. Skip `T6.2`, `S6.2`, `D6.2`, `D6.3` — all Gomory cuts, dropped.

Sheet 6 is `exercises/07-integer-programming-solution-methods/sheet-06-exercises.pdf`; the
self-study section (S6.x) is in the second half of the same file.

## Then the papers (timed, one minute per point)

Do the **graphical** ones first — that's three of the four formats:

- **SS23 E3** (16) — max, plot provided. The cleanest example of the classic format.
- **SS24 P3** (16) — **minimisation**, plot provided. Deliberately do this one second, while the
  max version is fresh, so you feel the pruning rule flip.
- **SS21 A4** (13) — max, solve graphically.
- **SS25 E5** (14) — the ink-blot reconstruction. Different skill: no plot, table lookup, plus
  the FIFO/LIFO part.

## Connections to the rest of the paper

- The relaxation bound is valid because of **weak duality**.
  → [04a-duality-lesson](04a-duality-lesson.md)
- A **stronger formulation** gives a tighter bound and so a smaller tree — which is why the
  disaggregated linking constraint from Day 1 matters here.
  → [05a-ip-modeling-lesson](05a-ip-modeling-lesson.md) Part 4
- If the constraint matrix is **totally unimodular**, the relaxation is already integral and B&B
  terminates at the root. → [08-total-unimodularity-and-matroids](08-total-unimodularity-and-matroids.md)
