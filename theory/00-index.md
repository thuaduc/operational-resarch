# Operations Research — theory index

Eleven cheat sheets distilled from the lecture decks and the central-exercise theory recaps,
pruned to what the past papers actually test. Every file has the same six sections:

**Definitions** · **Procedures** · **Formula box** · **Traps** · **True/false facts**

- **Procedures** = the step-lists you execute under time pressure. Drill these.
- **True/false facts** = your multiple-choice prep. SS24 and SS25 open with 14–16 points of it; across all eleven files that's ~120 pre-written statements.
- **Formula box + Traps** = the Day-14 read. Nothing else on the last day.

---

## The eleven files

| # | File | Topic |
|---|---|---|
| 01 | [lp-modeling-and-geometry](01-lp-modeling-and-geometry.md) | Standard form, polyhedra, vertices, graphical solution, the four outcomes |
| 02 | [simplex](02-simplex.md) | Tableau, pivoting, degeneracy, Big-M, two-phase, tableau forensics |
| 03 | [revised-simplex-and-sensitivity](03-revised-simplex-and-sensitivity.md) | `B⁻¹`, Row 0, reduced costs, shadow prices, cost/RHS ranging |
| 04 | [duality](04-duality.md) | SOB table, weak/strong duality, complementary slackness, zero-sum games |
| 04a | [duality-lesson](04a-duality-lesson.md) | **Teaching companion to 04, assumes no prior knowledge** — where the dual comes from (the bounding argument), the transpose, SOB, CS derived from strong duality, the refutation procedure, worked SS25 E3 |
| 05 | [ip-modeling](05-ip-modeling.md) | The full modelling-trick catalogue |
| 05a | [ip-modeling-lesson](05a-ip-modeling-lesson.md) | **Teaching companion to 05, assumes no prior knowledge** — binaries, summation notation, a first model built from scratch, then relaxation/complexity/templates, big-M derived, indicators, patterns by exam frequency, worked SS25 E4 |
| 06 | [branch-and-bound-and-cuts](06-branch-and-bound-and-cuts.md) | Bounds, branching, pruning, FIFO/LIFO, Gomory cuts |
| 06a | [branch-and-bound-lesson](06a-branch-and-bound-lesson.md) | **Teaching companion to 06, assumes no prior knowledge** — why the relaxation bounds, why branching loses nothing, the three pruning rules, a full worked tree, FIFO/LIFO traced, worked SS25 E5 |
| 07 | [column-generation](07-column-generation.md) | RMP, pricing problem, termination |
| 08 | [total-unimodularity-and-matroids](08-total-unimodularity-and-matroids.md) | Knapsack DP + FPTAS, TU, matroids + greedy |
| 09 | [network-flow](09-network-flow.md) | Conservation, residual networks, Ford-Fulkerson, max-flow/min-cut |
| 10 | [tsp-and-approximation](10-tsp-and-approximation.md) | Euler/Hamilton, SEC + MTZ, heuristics, MST-doubling vs Christofides |
| 11 | [nonlinear-and-convex-kkt](11-nonlinear-and-convex-kkt.md) | Hessians, convexity, Lagrange, KKT, Slater vs LICQ |

Chapter numbers do **not** match the central-exercise numbers. CE-7 is column generation only;
CE-8 bundles TU + matroids + network flow; CE-10 includes KKT; CE-11 is online optimization.

---

## Recurring question formats

| Format | Where it shows up |
|---|---|
| "Mark all correct answers" (7–8 items × 2 pts, **negative marking in SS25**) | SS24 P1, SS25 E1 |
| "Formulate the following constraints as linear (in)equalities; you may introduce additional variables, state their meaning in words" | SS21, SS24, SS25 — verbatim |
| "Solve using B&B; the LP relaxation is plotted, solve subproblems graphically" | SS21, SS23, SS24 |
| "The tree is ink-blotted — recover the missing nodes and give FIFO vs LIFO order" | SS25 E5 |
| "Classify all critical points as a function of the parameter α" | SS23 E7 |
| "What is the correct Row 0?" / "Reconstruct the LP from this tableau" | SS25 E2a, retake A2c |
| "Show or disprove / give a counterexample" | matroids, TSP heuristics, max-flow, convexity |
| "Show `x*` is not optimal" via a CS certificate rather than solving | SS24 P2, SS25 E3, retake A4 |
| Sensitivity chain: shadow price → validity range → range violated → new shadow price | retake A3 |

---

## Day-14 recall list

Out loud, without notes:

1. Standard form; what slack/surplus/artificial variables do.
2. Simplex optimality criterion and unboundedness criterion.
3. The SOB primal→dual table (max/min × ≤/≥/= × free/≥0).
4. Complementary slackness, both directions.
5. B&B pruning rules — for **max** and for **min**.
6. TU + integral `b` ⇒ integral polyhedron ⇒ IP in P.
7. Knapsack DP: `O(nW)` vs `O(nV)` — run whichever bound is smaller.
8. Matroid axioms; all bases are equicardinal.
9. Max-flow = min-cut; cut capacity counts **forward** arcs only.
10. Subtour elimination: SEC (exponential, tight) vs MTZ (polynomial, weak).
11. Christofides = **3/2** with odd-degree matching; MST-doubling = **2**. Both need the triangle inequality.
12. Hessian classification, and the eigenvalue fallback when leading principal minors are inconclusive.
13. KKT's four blocks; Slater ⇒ KKT ⟺ global; LICQ ⇒ candidates only.

---

## Three errors in the source material

Verified, and flagged in the relevant files:

1. **Midterm SS26 P2d** gives `{λ₁n₁+λ₂n₂ : λᵢ>0}` as the `c` making the LP *unbounded*. That cone is exactly where it is **bounded** — `c=(−1,−1)` is inside it and bounded; `c=(1,1)` is unbounded and outside it. → `01` Traps.
2. **retake-2021 A7 and ce-09-demo D9.1** both call MST-doubling "Christofides' 2-approximation". Christofides is the 3/2 algorithm with the odd-degree matching. If an exam phrases it their way, **execute MST-doubling** — that is what gets graded. → `10` Traps #1.
3. **The TU sufficient condition is never named in the lecture.** Reproduce the three numbered conditions; citing "Ghouila-Houri" earns nothing. → `08`.

---

## Deliberately not covered

In the course material, on no past paper: the **Hungarian method** (lecture-12 pp. 10–22, sheet
T8.3), the **DGS auction** (lecture-12 pp. 23–42, CE D8.2), and **online optimization** —
mirror descent, projected gradient, FTL/BTL/FTRL (CE-11), which survives only as a half-page
appendix in file 11.
