# Endterm outline — what Exercises 1–8 will look like

**Format:** 120 minutes, ~120 points, **7 exercises** (SS23, SS24, SS25 all had exactly 7;
the SS21 retake had 8). One point ≈ one minute. Aids: non-programmable calculator, analog
dictionary, ruler. No cheat sheet.

Below: 8 slots. Slots 1–7 are the reliable skeleton. Slot 8 is the rotating one — in a
7-exercise paper it gets folded into slot 6 or 7, or replaces slot 1.

---

## What actually appeared

| # | SS21 endterm | SS21 retake | SS23 | SS24 | SS25 |
|---|---|---|---|---|---|
| 1 | Sensitivity (16) | Graphical LP (7) | Duality (19) | **Multiple choice (14)** | **Multiple choice (16)** |
| 2 | Duality (6) | Simplex (14) | IP modelling (22) | Duality (16) | Simplex/sensitivity (16) |
| 3 | IP modelling (35) | Sensitivity (19) | Branch & Bound (16) | Branch & Bound (16) | Duality (15) |
| 4 | Branch & Bound (13) | Duality + CS (10) | Knapsack DP (14) | IP modelling (22) | IP modelling (21) |
| 5 | Flow + TU (14) | IP modelling (32) | Matroids (14) | Matroids (14) | Branch & Bound (14) |
| 6 | Convex functions (13) | Min-cut + complexity (14) | TSP heuristic (12) | IP + NP-hardness (18) | Max-flow/min-cut (14) |
| 7 | Column generation (23) | Convex functions (12) | Critical points (23) | Nonlinear (20) | Convexity + KKT (20) |
| 8 | — | Lagrange/KKT (12) | — | — | — |

**Never absent:** IP modelling, branch & bound, duality, nonlinear.
**Usually there:** multiple choice (since SS24), simplex/sensitivity, one combinatorial topic.
**Rare:** column generation (SS21 only, but 23 points when it came).

---

## Exercise 1 — Multiple choice · 14–16 pts

**Type.** 7–8 items, 2 pts each, "mark all correct answers (there can be more than one)".
**Partial credit with a penalty:** all correct → 2 pts; exactly one error → 1 pt; more →
0 pts. Selecting nothing = 0. So **never leave one blank** — guessing dominates.

**Topics.** One item per lecture block: duality, Christofides/TSP, cutting planes,
sensitivity, matroids, total unimodularity, simplex cycling, gradient descent / KKT.

**Example** (SS25 1a):
```
Which of the following statements are correct?
 ☐ Every linear program has a dual program.
 ☐ If a primal constraint is not tight, then its dual variable is zero.
 ☐ If the primal is infeasible, then the dual is unbounded.
 ☐ If the primal is unbounded, then the dual is infeasible.
```
→ 1st ✓ (always), 2nd ✓ (complementary slackness), 3rd ✗ (infeasible primal ⇒ dual is
unbounded **or** infeasible), 4th ✓ (weak duality).

**Watch.** The traps are always the "always/never" quantifiers and the 3×3 primal–dual
outcome table. Practice: SS24 P1, SS25 E1.

---

## Exercise 2 — Simplex, tableau forensics, sensitivity · 14–19 pts

**Type.** You're handed an optimal (or corrupted) tableau and asked to read things off it.
Almost never "run simplex from scratch" — it's *reverse* engineering, then a sensitivity
chain of 3–4 sub-questions.

**Topics.** Row 0 = `c_B B⁻¹A − c`, reduced costs, shadow prices, cost ranging, RHS ranging,
degeneracy, pricing a new column.

**Example** (SS25 E2, "Lemonade or Iced Tea"). Given a feasible tableau with **wrong Row 0**:
```
Basis │ x₁  x₂   s₁    s₂   s₃  s₄ │ RHS
Row 0 │  0  −1    2     1    0   0 │   0
  x₁  │  1   0  1/3  −1/3    0   0 │  20
  x₂  │  0   1 −1/6   2/3    0   0 │  10
```
a) What is the correct Row 0?
b) Is the corrected tableau optimal? Why?
c) Range of the Lemonade price `p₁` keeping this basis optimal.
d) Which perturbation of the water capacity makes the BFS degenerate — which basic
   variable hits zero first?
e) New drink uses 3 L water + 1 kg lemon: write its reduced cost and the condition on `p_S`
   for it to enter the basis.

**Watch.** The sensitivity chain in retake-2021 A3 is the canonical version: shadow price →
validity range → violate the range → recompute the new shadow price after a basis change.

---

## Exercise 3 — Duality and complementary slackness · 6–19 pts

**Type.** Three parts, near-verbatim every year: (a) derive the dual, (b) state both sets of
CS conditions, (c) use CS to **disprove** optimality of a given point.

**Topics.** SOB table (max/min × ≤/≥/= × free/≥0), weak & strong duality, the 3×3 outcome
table, occasionally a zero-sum game.

**Example** (SS25 E3):
```
(P)  max 3x₁ + 2x₂ + x₃
     s.t.  x₁ + 2x₂ +  x₃ ≤ 10
          2x₁ −  x₂ + 2x₃ ≥  4
           x₁ +  x₂ −  x₃  =  3
           x₁, x₂, x₃ ≥ 0
```
a) Derive (D). → `y₁ ≥ 0`, `y₂ ≤ 0`, `y₃` free, three `≥` dual constraints.
b) Primal CS: `xⱼ(Aᵀy − c)ⱼ = 0`. Dual CS: `yᵢ(Ax − b)ᵢ = 0`.
c) Show `x = (2,2,1)ᵀ` is **not** optimal — build the dual system CS forces, show it has no
   feasible solution. You never solve the LP.

**Watch.** Part (c) is worth the most and is pure procedure. Also: "is (D) unbounded,
infeasible, or finite — *without solving it*" (retake-2021 A4c).

---

## Exercise 4 — IP modelling · 21–35 pts

**The biggest single block on every paper.** A word problem, 5–7 sub-parts, each asking
you to encode one English sentence as linear (in)equalities. Prompt is verbatim each year:

> *"Formulate the following constraints as linear (in)equalities. You may introduce
> additional variables. If you do so, please also state their intuitive meaning in words."*

**Topics.** Assignment, capacity, logical implication, big-M, either/or, at-most-k,
conditional counting, multi-objective scalarisation.

**Example** (SS25 E4, "School Planning"). `yᵢ ∈ {0,1}` school `i` built, `x_{i,j} ∈ {0,1}`
student `j` → school `i`:

| Sub-part | English | Answer |
|---|---|---|
| a | each student to exactly one school | `Σᵢ x_{i,j} = 1  ∀j` |
| b | capacity | `Σⱼ x_{i,j} ≤ cᵢ yᵢ  ∀i` |
| c | assign only if built | `x_{i,j} ≤ yᵢ  ∀i,j` |
| d | built schools ≥ 5 km apart | `yᵢ + yᵢ' ≤ 1  ∀ i≠i' with d_{i,i'} < 5` |
| e | >200 students in zone A ⇒ ≥300 in zone B | introduce `z ∈ {0,1}`, two big-M rows |
| f | one objective: min cost, max preference | `min Σᵢ fᵢyᵢ − λ Σ_{i,j} s_{i,j} x_{i,j}` |

Part (e) is the graded one — the implication pattern:
```
Σ(zone A) ≤ 200 + M·z        (z = 1 forced if more than 200)
Σ(zone B) ≥ 300·z            (obligation activates only when z = 1)
```

**Watch.** Always name your helper variables in words — undocumented variables score zero.
Past scenarios: beer gardens (SS21), machine scheduling with maintenance windows (retake),
ice cream (SS23), Tour d'Allemagne stages (SS24), schools (SS25).

---

## Exercise 5 — Branch and Bound · 13–16 pts

**Type.** Two flavours. (i) Classic: solve a 2-variable IP by B&B, subproblems solved
graphically on the plot provided. (ii) SS25's variant: the tree is **ink-blotted** and you
reconstruct it from a table of candidate subproblems.

**Topics.** LP relaxation as bound, branching rule, the three pruning rules (integrality /
bound / infeasibility — and their direction flips between max and min), FIFO vs LIFO order,
sometimes a Gomory cut.

**Example** (SS25 E5, "Gondolf the wizard"):
```
max 3x₁ + 2x₂
s.t. 2.4x₁ + x₂ ≤ 9.15
      x₁ +  x₂ ≤ 6
      x₁, x₂ ∈ ℕ₀

P₀: x = (2.25, 3.75), Z₀ = 14.25, branch on x₁
```
a) Match the blotted nodes `P₁,P₂,P₄,P₅,P₆` to rows of the given subproblem table.
b) Recover the missing branching constraints.
c) Optimal integer solution and objective value.
d) In what order would the nodes be explored under (i) FIFO and (ii) LIFO, stopping as soon
   as optimality is confirmed?

**Watch.** Part (d) is free points if you know the two traversal orders cold. Pruning a node
whose bound merely *ties* the incumbent is allowed — say so explicitly.

---

## Exercise 6 — Combinatorial block · 12–18 pts

**Type.** Rotates between five topics. This is where "show or disprove / give a
counterexample" lives.

| Topic | Year | Shape |
|---|---|---|
| Matroids | SS23, SS24 | verify the 3 axioms, or give a set system that violates one |
| Network flow / min cut | SS21, SS25, retake | Ford–Fulkerson, max-flow = min-cut, counterexamples |
| Knapsack DP | SS23 | fill the DP table, `O(nW)` vs `O(nV)` |
| Total unimodularity | SS21, retake | check the 3 sufficient conditions on an incidence matrix |
| TSP / approximation | SS23, SS24 | disprove a heuristic, or run MST-doubling |

**Example A** (SS25 E6, max-flow counterexamples). *"Use the given graph to find a
counterexample and describe why it refutes the statement."*
> a) If all capacities are odd, there is a maximal `s–t` flow `f` with `f(e)` odd for all `e`.
> b) Adding `λ ∈ ℕ` to all capacities does not change the minimal cuts.

Both are false; you draw capacities on the given diamond graph `s→a,b→c→t` and exhibit them.
(b) fails because adding λ penalises cuts with *more* edges — a 2-edge cut and a 1-edge cut
that tie will separate once you shift everything up.

**Example B** (SS23 E4, knapsack DP). 5 vinyl records, capacity 5 kg. `V_max = 12 > W_max = 5`,
so break down **by weight** (`O(nW)` is the smaller bound). Fill the 6×6 table, read off
profit 6 from the corner, backtrack for the item set.

**Example C** (SS24 P6). Min number of ATMs covering every street →
`min Σᵢ xᵢ s.t. xᵢ + xⱼ ≥ 1 ∀(i,j) ∈ E`. Then: *"which NP-hard problem is this?"* → vertex
cover. Then: *"how many for a complete graph on n nodes?"* → `n − 1`.

**Watch.** For TU, reproduce the three numbered sufficient conditions — naming
"Ghouila-Houri" earns nothing, the lecture never names it. For TSP, the papers mislabel
MST-doubling as "Christofides"; execute MST-doubling if the wording matches theirs.

---

## Exercise 7 — Nonlinear optimization · 12–23 pts

**Type.** Two flavours, and some years you get both (that's what makes it slot 7 + 8).

**7a — Unconstrained.** Compute `∇f`, solve `∇f = 0`, classify each critical point via the
Hessian (leading principal minors, eigenvalues as fallback). Frequently parameterised.

**Example** (SS23 E7): for `α > 0`, classify all critical points of
`f_α(x,y) = −¼x⁴ − ¼y⁴ + αxy` **as a function of α**. You get a case split on α, not a
single answer.

**Example** (SS24 P7a): `f(x,y) = x² + y² + e^{2x} − (8x + 6y) + 2xy`. Trace and determinant
of `∇²f` are both positive everywhere ⇒ strictly convex ⇒ at most one minimum, no maxima.

**7b — Constrained: convexity, Slater, KKT.** A modelling story leading to a convex program,
then the four KKT blocks.

**Example** (SS25 E7, "Minimal Circle Enclosure" — smallest circle covering points `pᵢ`):
```
a) Define a convex function. Prove any norm ‖·‖ on ℝᵈ is convex.
b) Write the problem as   min_{r,c₁,c₂} r
                          s.t. ‖pᵢ − c‖₂ − r ≤ 0  ∀i,  r ≥ 0
   Show f and every gᵢ are convex.
c) Show Slater's condition holds — give an explicit strictly feasible (r,c₁,c₂).
d) From L = f + Σλᵢgᵢ − μr, use KKT to derive  Σλᵢ = 1,  c* = Σλᵢpᵢ.
e) What does λᵢ > 0 imply about pᵢ?   → pᵢ lies ON the circle (CS: its constraint is tight).
```

**Watch.** Part (a) — proving convexity straight from the definition — is worth real points
and people skip it. Know the split: **Slater ⇒ KKT point is a global optimum**;
**LICQ ⇒ KKT gives candidates only**.

---

## Exercise 8 — The wildcard · 12–23 pts

Only appears when the paper runs to 8 problems (retake SS21), otherwise merged upward.
Three things have filled it:

1. **Column generation** (SS21 E7, 23 pts). Set up the restricted master problem, count the
   columns of `A`, formulate the pricing problem, argue termination.
   *Example:* `m` friends with luggage `wᵢ`, cars of capacity `W`, some pairs refuse to
   share. Columns of `A` = valid fill patterns. "How many columns does `A` have if every
   pair is feuding?" → exactly `m` (each person alone). "When is column generation
   especially worthwhile?" → when `|S|` is small, so the pattern count explodes.
2. **Lagrange multipliers with nonlinear constraints** (retake SS21 A8) — the constrained
   half of nonlinear split off into its own problem.
3. **Graphical LP** (retake SS21 A1, 7 pts) — read the vertices off a plotted feasible
   region, list them lexicographically. Cheap points, appears as a warm-up.

---

## How to spend the 120 minutes

```
E4  IP modelling      ~22 pts / 25 min   ← largest, do it while fresh
E7  Nonlinear         ~20 pts / 22 min   ← second largest
E1  Multiple choice   ~15 pts / 12 min   ← best pts/min, never blank
E2  Simplex/sens      ~16 pts / 18 min
E3  Duality           ~15 pts / 15 min
E5  Branch & Bound    ~14 pts / 14 min
E6  Combinatorial     ~14 pts / 14 min
```

Scan all problems first, start with your strongest. **Document every approach** — an
undocumented answer scores zero even when it's right.
