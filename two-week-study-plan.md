# Operations Research — endterm plan

**Exam: Friday 7 August 2026.** Study days: Fri 24 Jul → Thu 6 Aug.

## How to use this plan

Each `theory/` file has three sections: **Definitions**, **Procedures**, **Formula box**.

- **Read the theory file, then go straight to the exercise sheet.** Solutions only after a real attempt.
- **Procedures** = the step-lists you execute under time pressure. Drill these.
- **Formula box** = your cheat sheet for quick recall.
- Go back to a lecture PDF only if a theory file leaves you genuinely stuck.

## Priorities

Every endterm has: **IP modelling**, **nonlinear/convex (KKT)**, **duality**, **branch-and-bound**, and a **multiple-choice** block. Most years also: network flow, TU/matroids, TSP, simplex & sensitivity. Column generation appears occasionally.

Nonlinear optimization is the second-largest block — give it two full days.

## Corrections

- **Dynamic programming is NOT dropped.** SS23 E4 is a full 14-pt DP question. Covered in `theory/08`.
- **Christofides = 3/2-approximation** (matching on odd-degree MST vertices). **MST-doubling = 2-approximation.** Past exams mislabel MST-doubling as "Christofides" — if so, execute MST-doubling.
- **Midterm SS26 P2d sample solution is wrong.** The cone it gives for unbounded `c` is actually where it's bounded.

## Notes

- `theory/` chapter numbers ≠ central-exercise numbers. CE-7 = column generation; CE-8 = TU + matroids + network flow; CE-10 = KKT.
- **Don't open SS24 before Day 11 or SS25 before Day 13** — they're your mocks.
- Not covered (never examined): Hungarian method, DGS auction, online optimization.

---

## Day 1 — Fri 24 Jul — exam format + LP modelling

- [x] Skim `endterm-2023-solutions.pdf` to see the exam shape.
- [x] `theory/01` — standard form, feasible region, vertices, the four outcomes.
- [x] Sheet 1: T1.1, T1.3, S1.2, S1.3.
- [x] Memorise Procedure A (model an LP in 5 steps).
- [x] Exam drill: retake-2021 A1 (graphical solution, 7 pts).

## Day 2 — Sat 25 Jul — simplex

- [x] `theory/02` — tableau, pivot, optimality, unboundedness, degeneracy.
- [x] Sheet 2: T2.1, T2.2, S2.1, S2.2, S2.3.
- [x] Drill: "Read a tableau — check order", Forensics A (reconstruct LP), Forensics B (parameter ranges).
- [x] Exam drill: retake-2021 A2 (pivot parameter ranges + reconstruct LP, 14 pts).

## Day 3 — Sun 26 Jul — revised simplex and sensitivity

- [ ] `theory/03` — `B⁻¹`, reduced costs, Row 0, shadow prices, cost/RHS ranging, new columns.
- [ ] Sheet 3: T3.1, T3.2, T3.3, S3.2, S3.3.
- [ ] Exam drill: retake-2021 A3 (shadow price → range → violated → new shadow price, 19 pts).
- [ ] Exam drill: endterm-2021 A1 (read optimal tableau, price/RHS range, new product, 16 pts).
- [ ] Memorise `Row0 = c_B B⁻¹A − c` and reduced cost of a new column.

## Day 4 — Mon 27 Jul — duality

- [ ] `theory/04` — SOB table, weak/strong duality, 3×3 outcome table, complementary slackness.
- [ ] Sheet 4: T4.1, T4.2, T4.3, S4.1, S4.2.
- [ ] Exam drill: SS23 E1 (derive dual, boundedness, optimal value, 19 pts).
- [ ] Exam drill: retake-2021 A4 (CS conditions + optimality check, 10 pts).
- [ ] Reproduce SOB table from memory, then the 4-step optimality certificate.

## Day 5 — Tue 28 Jul — IP modelling I

- [ ] `theory/05` — full trick catalogue in the Formula box.
- [ ] Sheet 5: T5.1, T5.2, T5.3, S5.1.
- [ ] Exam drill: SS23 E2 (ice cream production, 22 pts — iff-indicator + pairwise overlap).
- [ ] Exam drill: endterm-2021 A3 (beer distribution, 35 pts — the longest IP problem).

## Day 6 — Wed 29 Jul — IP modelling II + branch-and-bound

- [ ] Sheet 5: S5.2, S5.3.
- [ ] Exam drill: retake-2021 A5 (screw factory, 32 pts — startup costs, consecutive days, maintenance).
- [ ] `theory/06` — LP relaxation bounds, branching, three pruning rules (max and min), FIFO/LIFO, Gomory cuts.
- [ ] Sheet 6: T6.1, T6.2, S6.1, S6.2.
- [ ] Exam drill: SS23 E3 (B&B, 16 pts). Exam drill: endterm-2021 A4 (B&B, 13 pts).

## Day 7 — Thu 30 Jul — knapsack DP, TU, matroids

- [ ] `theory/08` — knapsack DP + FPTAS, total unimodularity, matroids + greedy.
- [ ] Exam drill: SS23 E4 (knapsack DP, 14 pts).
- [ ] Sheet 8: T8.1, S8.1, S8.3. Skip T8.3 (Hungarian), S8.2 (DGS).
- [ ] Exam drill: SS23 E5 (matroids, 14 pts).
- [ ] Exam drill: retake-2021 A6a (TU proof with row partition, 14 pts).
- [ ] Reproduce the three TU sufficient conditions verbatim.

## Day 8 — Fri 31 Jul — network flow, column generation, TSP

- [ ] `theory/09` — Ford-Fulkerson, max-flow/min-cut, five modelling transformations.
- [ ] Sheet 8: T8.2.
- [ ] Exam drill: endterm-2021 A5 (min-cost flow + TU, 14 pts).
- [ ] `theory/07` — pricing problem and termination test.
- [ ] Sheet 7: T7.1, S7.1, S7.2.
- [ ] Exam drill: endterm-2021 A7 (column generation carpooling, 23 pts).
- [ ] `theory/10` — SEC, MTZ, nearest-neighbour, MST-doubling vs Christofides.
- [ ] Sheet 9: T9.1, T9.2, S9.1.
- [ ] Exam drill: retake-2021 A7 (nearest-insertion + Kruskal + MST-doubling + lower bound, 12 pts).
- [ ] Exam drill: SS23 E6 (TSP heuristic counterexample, 12 pts).

## Day 9 — Sat 1 Aug — nonlinear I: unconstrained

- [ ] `theory/11` first half — gradient, Hessian, definiteness (minors + eigenvalues), critical points.
- [ ] Sheet 10: T10.1.
- [ ] Exam drill: SS23 E7 (classify critical points as function of parameter, 23 pts).
- [ ] Exam drill: endterm-2021 A6 (convexity on restricted domain via Hessian, 13 pts).

## Day 10 — Sun 2 Aug — nonlinear II: convexity, Lagrange, KKT

- [ ] `theory/11` second half — convex sets/functions, Slater vs LICQ, KKT system, decision flowchart.
- [ ] Sheet 10: T10.2, T10.3. Sheet 11: T11.1, T11.2.
- [ ] Exam drill: retake-2021 A8 (Lagrange with equality constraint, 12 pts).
- [ ] Drill: prove convexity from definition; Lagrangian → KKT → tight/not-tight case split.

## Day 11 — Mon 3 Aug — Mock 1: SS24 endterm

- [ ] `endterm-2024-solutions.pdf`, 120 min, closed-book.
- [ ] Mark same day. For each mistake write the replacement action.

## Day 12 — Tue 4 Aug — repair

- [ ] Redo your two worst SS24 problems from blank paper.
- [ ] Redo SS24 P1 (multiple choice) closed-book, writing *why* for each option.
- [ ] Re-read every **Formula box** section across all eleven files in one sitting.

## Day 13 — Wed 5 Aug — Mock 2: SS25 endterm

- [ ] `endterm-2025-exam.pdf`, 120 min, closed-book.
- [ ] Mark against solutions. Compare per problem against Mock 1.

## Day 14 — Thu 6 Aug — final rehearsal

- [ ] Re-solve your three most-missed questions.
- [ ] Read only the **Formula box** of all eleven files.
- [ ] Recall without notes: standard form; simplex criteria; SOB table; CS both directions; B&B pruning; TU ⇒ integral; knapsack DP complexity; max-flow = min-cut; SEC; Christofides 3/2 vs MST-doubling 2; Hessian test; KKT + Slater vs LICQ.
- [ ] No new topics. Pack calculator, ruler, dictionary, ID. Sleep.

## Exam day — Fri 7 Aug

- [ ] Allowed: non-programmable calculator, analog dictionary, ruler. No cheat sheet.
- [ ] Scan all 7 problems first, start with your strongest.
- [ ] Always document the approach — undocumented answers score zero.
- [ ] Never leave a multiple-choice question blank.
