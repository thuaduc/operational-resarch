# Operations Research — retake plan (14 days, one topic per day)

**Context: endterm 2026 (Fri 7 Aug) not passed.** The paper is in
[exams/endterm/endterm-2026.pdf](exams/endterm/endterm-2026.pdf) with the official
solution next to it. This plan is built from what that paper actually asked.

## What the endterm taught us

The 2026 paper, problem by problem:

| # | Topic | Credits | Old plan's verdict |
|---|---|---|---|
| P1 | Multiple choice (all topics) | 16 | "free-rides on the rest" — still true |
| P2 | IP/LP modeling (glass production) | 21 | covered ✓ |
| P3 | Duality + complementary slackness | 15 | covered ✓ |
| P4 | Branch & bound (knapsack) | 15 | covered ✓ |
| P5 | **Gomory cuts, full procedure** | 15 | **dropped — "MC only". Wrong.** |
| P6 | **Max flow: Ford–Fulkerson, residual net, min cut** | 17 | reduced to "mechanics only" |
| P7 | Convexity + Slater + KKT case analysis | 22 | covered, but it's the hardest block |

Two lessons carried into this plan:

1. **Nothing is dropped.** The chair rotates topics; betting on absence cost ~32 credits
   of preparation this time. Every examinable procedure gets a day.
2. **Six-day cramming was the real problem, not the triage.** With 14 days there is no
   need to gamble. One topic per day, every day ends with a timed exam problem.

## Daily rhythm (same every day, ~3–4 h)

1. **Theory (45–60 min).** The `a`-lesson file first if one exists, then the Procedures +
   Formula box of the main file. Definitions are reference, not reading.
2. **Warm-up ladder (60–90 min, untimed).** Sheet + central-exercise demos for the topic.
   Look at solutions when stuck — this phase is for understanding.
3. **Timed exam problem (point count = minutes, no solution until the clock stops).**
   Preferably the 2026 endterm problem for that topic — you now own the paper *and* its
   solution, which makes it the best-calibrated mock in the repo. Then past papers.
4. **Post-mortem (15 min).** Compare against the official solution *line by line*.
   "Answers are only accepted if the solution approach is documented" — check that your
   write-up would score, not just that the number matches.

Exercise notation as before: `T5.1`/`S5.1` = training/self-study on the sheet in
`exercises/<topic>/`, `D5.2` = demo in `central exercises/<topic>/`. Folder names match
topics; sheet numbers don't. Ratings as before: `[SAME]` appeared verbatim on a paper ·
`[EXAM]` same level/type · `[DRILL]` one component skill · `[CONCEPT]` read only.
Three ratings are **upgraded** from the old plan because the 2026 endterm proved them
wrong: Gomory cuts (`T6.2`/`S6.2`/`D6.2`/`D6.3`), max flow (`T8.2`), and knapsack B&B
(`T6.1`) are all `[EXAM]` or better now.

---

## Week 1 — LP core, then the money topics

### Day 1 (Sat 6 Sep) — LP modeling & geometry
Foundation for P2 and half the MC bank.
- [ ] [theory/01-lp-modeling-and-geometry.md](theory/01-lp-modeling-and-geometry.md)

**Warm-up ladder — sheet 1 + CE-01, untimed**
- [ ] `S1.1` *LP mapping* — `[DRILL]` match algebraic LPs to their pictures. Fast geometry
      check: feasible region, objective direction, where x* sits.
- [ ] `S1.3` *Graphical solution of an LP* — `[DRILL]` read a model back off a drawing.
- [ ] `T1.1` *Mining company* — `[EXAM]` short word problem, full model + graphical solve.
- [ ] `T1.3` *Vitamin tablets* — `[EXAM]` generic model with index sets (i, k, a_ik) —
      the same abstraction level as P2's `Σ a_ki x_i`. Document the derivation in steps.
- [ ] `S1.2` *Forecast planning* — `[EXAM]` inventory balance across months; hardest on
      the sheet. Only if the first four went smoothly.
- [ ] `T1.2` *Study hours* — `[CONCEPT]` skim the model, skip the Gurobi part.
- [ ] `D1.x` from `central exercises/01-.../ce-01-demo.pdf` — one demo as a spare rep.

**Timed**
- [ ] One modeling problem from a midterm paper (`midterm/`), point count = minutes.
- Done when: you can go from prose → decision variables → objective → constraints without
  touching the solution, and can name vertices/extreme points/basic feasible solutions.

### Day 2 (Sun 7 Sep) — Simplex
The tableau mechanics that P5 (Gomory) reads from — this is a prerequisite, not nostalgia.
- [ ] [theory/02-simplex.md](theory/02-simplex.md)

**Warm-up ladder — sheet 2 + CE-02, untimed**
- [ ] `T2.2` *Simplex algorithm (tableau reading)* — `[EXAM]` given tableaus: name all
      legal pivots, decide solvable/unbounded, state **all** optimal solutions. This is
      the P5a skill (read x* off a tableau) plus half the simplex MC bank. Do it first.
- [ ] `S2.3` *Simplex tableau special cases* — `[DRILL]` six tableaus: degenerate,
      unbounded, alternative optima, infeasible-Big-M — recognize each on sight.
- [ ] `T2.1` *Simplex algorithm (full run)* — `[EXAM]` graphical → standard form →
      canonical form → full tableau iterations → degeneracy and uniqueness discussion.
      One complete rep of the whole pipeline.
- [ ] `S2.4` *General and special cases* — `[DRILL]` one canonical-form transform + one
      iteration. Quick.
- [ ] `S2.1` *Simplex with parameters α, β* — `[EXAM]` midterm-style "for which α…" —
      only if time allows.
- [ ] `S2.2` *Big M method* — `[CONCEPT]` skim; a from-scratch Big-M run has never been
      asked (the exam hands you the tableau).

**Timed**
- [ ] Full simplex run from a past midterm; then read every number in the final tableau
  aloud (what is basic, what the reduced costs mean, where B⁻¹ sits).
- Done when: given an optimal tableau you can extract solution, objective, basis, and
  reduced costs in under two minutes — P5a was exactly this for 2 credits.

### Day 3 (Mon 8 Sep) — Revised simplex & sensitivity
- [ ] [theory/03-revised-simplex-and-sensitivity.md](theory/03-revised-simplex-and-sensitivity.md) and [theory/03a-sensitivity-lesson.md](theory/03a-sensitivity-lesson.md)

**Warm-up ladder — sheet 3 + CE-03, untimed**
- [ ] `T3.3` *Sensitivity* — `[DRILL]` ranging in isolation. Start here.
- [ ] `T3.2` *Lemonade Production* — `[EXAM]` closest sheet match to SS25 E2's format.
- [ ] `D3.2` *Waldgeist Distillery* — `[EXAM]` the full sensitivity chain in one problem.
- [ ] `S3.5` *PopCo* — `[EXAM]` second rep of the full chain.
- [ ] `D3.1` / `T3.1` / `S3.2` *Revised simplex* — `[CONCEPT]` know the formulas
      (B⁻¹, reduced costs); don't grind iterations.

**Timed**
- [ ] Midterm SS26 sensitivity problem, then SS21 A1 *Backmischung* (16) if time.
  (Note: midterm SS26 P2d's sample solution is wrong — the cone it gives for unbounded
  `c` is where the LP is bounded.)
- Done when: you can do a RHS range, a cost range, and interpret a shadow price, stating
  in each case *which* basis stays optimal and why.

### Day 4 (Tue 9 Sep) — Duality & complementary slackness *(P3, 15 cr)*
- [ ] [theory/04-duality.md](theory/04-duality.md) + [theory/04a-duality-lesson.md](theory/04a-duality-lesson.md); reproduce the
  SOB table from memory before touching an exercise.

**Warm-up ladder — sheet 4 + CE-04, untimed**
- [ ] `D4.1` *Dual Problem* — `[DRILL]` mechanical dual derivation. Repeat until the SOB
      table is automatic.
- [ ] `T4.2` *Duality* — `[DRILL]` same, with **mixed ≤/≥/= constraints and free
      variables** — exactly the P3a shape (max with ≤, ≥, = rows in one program).
- [ ] `T4.1` *Duality & Complementary Slackness* — `[EXAM]` the exam's exact shape:
      derive dual → state CS → use it.
- [ ] `S4.2` *Duality & Complementary Slackness* — `[EXAM]` second rep of that shape.
- [ ] `D4.2` *Primal-Dual* — `[EXAM]` extra rep if the CS certificate still feels shaky.
- [ ] `T4.3` *Rock-Paper-Scissors* · `D4.3` *Transportation* — `[CONCEPT]` read only.

**Timed**
- [ ] **Endterm 2026 P3 complete (15 min)**, then SS25 E3 (15) and SS24 P2 (16).
- [ ] Memorize the 4-step optimality certificate: assume optimal → CS forces which duals
  are zero → solve the reduced dual system → show consistency or a violation.
- Done when: you can write the dual of any mixed-constraint LP from the transformation
  table without hesitation, state CS conditions explicitly, and *use* CS to test a
  primal/dual pair for joint optimality (P3c: plug in, find the violated condition).

### Day 5 (Wed 10 Sep) — IP modeling *(P2, 21 cr — biggest block every year)*
- [ ] [theory/05-ip-modeling.md](theory/05-ip-modeling.md) + [theory/05a-ip-modeling-lesson.md](theory/05a-ip-modeling-lesson.md) — copy the trick catalogue out by hand again

**Warm-up ladder — sheet 5 + CE-05, untimed**
- [ ] `S5.1` *Logic* — `[DRILL]` pure "translate this logical statement into
      inequalities". Start here.
- [ ] `T5.1` *Modelling Tricks – OR* — `[DRILL]` the either-or / big-M disjunction alone.
- [ ] `T5.2` *Caffeine* — `[EXAM]` short word problem, full model.
- [ ] `S5.2` *Party Planning* — `[EXAM]` word problem with logical side conditions.
- [ ] `S5.3` *Dutch Petroleum* — `[EXAM]` **piecewise-linear pricing — upgraded from
      optional: P2j (Boron anomaly) was exactly a threshold-triggered piecewise model.**
- [ ] `D5.2` *School Planning* — `[SAME]` was SS25 E4 word-for-word; do it properly.
- [ ] `D5.1` *Organic Farmer* — `[EXAM]` hardest index-range work in the course; spare.
- [ ] `T5.3` *Exam Preparation* — `[EXAM]` spare rep.

**Timed**
- [ ] **Endterm 2026 P2 complete (21 min)**. It contains the full catalogue:
  linking constraints (x ≤ y), minimum-lot-size (x ≥ 0.02·y), at-most-k (Σy ≤ 6),
  implication (BaO ⇒ Al₂O₃: y_B ≤ y_A), mutual exclusion (y_Li + y_K ≤ 1),
  **linearizing a product w = z·x** (w ≤ z, w ≤ x, w ≥ x − (1−z)), and a
  threshold-triggered piecewise model with a binary z.
- Done when: every pattern above is written from memory on one A4 sheet.

### Day 6 (Thu 11 Sep) — Branch & bound *(P4, 15 cr)*
- [ ] [theory/06-branch-and-bound-and-cuts.md](theory/06-branch-and-bound-and-cuts.md) + [theory/06a-branch-and-bound-lesson.md](theory/06a-branch-and-bound-lesson.md) (B&B half)

**Warm-up ladder — sheet 6 + CE-06, untimed**
- [ ] `D6.1` *Branch-and-Bound* — `[EXAM]` a full worked tree. First and slowly; it is
      the template for everything else.
- [ ] `T6.1` *Knapsack Branch-and-Bound* — **`[SAME-TYPE]` — upgraded: endterm 2026 P4
      was precisely knapsack B&B with the greedy-ratio LP relaxation.** The core rep.
- [ ] `S6.1` *Branch-and-Bound* — `[EXAM]` third rep; stop when the pruning rules stick.
- [ ] `T6.3` *Staff Scheduling* — `[DRILL]` an IP model — doubles as Day 5 revision.

**Timed**
- [ ] **Endterm 2026 P4 complete (15 min)**, then SS25 E5 (the ink-blot reconstruction)
  and SS24 P3.
- P4a is a *reasoning* part: why an unbounded relaxation (P_free) gives no useful bound
  while P_≥0 does. Practice writing the bounding/pruning logic in words, not just trees.
- Done when: you can run B&B on a knapsack with the greedy-fractional LP hint, prune
  correctly by bound/integrality/infeasibility, and *say why* at each node.

### Day 7 (Fri 12 Sep) — Gomory cuts *(P5, 15 cr — the topic that burned us)*
- [ ] [theory/06-branch-and-bound-and-cuts.md](theory/06-branch-and-bound-and-cuts.md) (cuts half)

**Warm-up ladder — sheet 6 + CE-06, untimed — the exact items the old plan skipped,
all upgraded to `[EXAM]`**
- [ ] `T6.2` — first cut derivation, slowly, with the solution open next to you.
- [ ] `D6.2` — a full worked demo; treat it as the grading template.
- [ ] `D6.3` — second demo, done without peeking.
- [ ] `S6.2` — closed-book rep. If this one is clean, the topic is banked.

**Timed**
- [ ] **Endterm 2026 P5 complete (15 min)**.
  The full chain: read x* off the tableau → pick the fractional row → split every
  coefficient into ⌊·⌋ + fraction → write the fractional cut Σ f_j x_j ≥ f₀ →
  plug x* in to show violation → append the cut to the original LP relaxation.
- Done when: you can derive a cut from *any* tableau row, in both the ≥-fractional and
  ≤-integer forms, and know why the cut never removes an integer-feasible point.

---

## Week 2 — the rest of the syllabus, then rehearsal

### Day 8 (Sat 13 Sep) — TU, matroids, knapsack DP (polynomial-time solvability)
Mostly MC fodder (P1a, P1e were TU/complexity), occasionally a full problem.
- [ ] [theory/08-total-unimodularity-and-matroids.md](theory/08-total-unimodularity-and-matroids.md) + both lesson files ([08a](theory/08a-matroids-lesson.md), [08b](theory/08b-tu-and-knapsack-dp-lesson.md))

**Warm-up ladder — sheets 7/8 + CE-07/08, untimed**
- [ ] `D8.1` *Unimodularity* — `[EXAM]` prove/disprove TU; the zero-free 2×2 with
      |det| ≥ 2 trick for disproving.
- [ ] `D8.3` *Matroids* — `[EXAM]` the most-repeated E6 topic (full 14-pointer on both
      SS23 and SS24).
- [ ] `T8.1` *Matroids* — `[EXAM]` second rep; axioms verbatim, exchange direction right
      (*smaller set grows, element comes from the larger*), 3-element counterexample.
- [ ] `D7.1` / `T7.1` *Cutting* — `[EXAM]` knapsack DP: fill the table, state O(nW) vs
      O(nV) and which to run, backtrack the solution.
- [ ] `S8.1` *TU + matroid dual* — `[EXAM]` spare. `S7.1` *Cutting* · `S7.2` *Bin
      Packing* — spares if DP felt shaky.

**Timed**
- [ ] SS23 E4 DP problem (14 cr — DP is *not* dropped by the chair) + SS24 P5 matroids
  (prove one, disprove one).
- Reproduce the three numbered TU conditions — naming "Ghouila-Houri" earns nothing.
- Done when: you can decide TU for small matrices, state what TU + integral b buys you
  (P1e tested exactly the vertices-vs-feasible-points distinction), and fill a knapsack
  DP table.

### Day 9 (Sun 14 Sep) — Network flow *(P6, 17 cr)*
- [ ] [theory/09-network-flow.md](theory/09-network-flow.md) + [theory/09a-network-flow-lesson.md](theory/09a-network-flow-lesson.md)

**Warm-up ladder — sheet 8 + CE, untimed**
- [ ] `T8.2` *Maximum Flow* — **`[EXAM]` — upgraded from `[DRILL]`: P6 was a full
      17-credit Ford–Fulkerson run.** Residual network → augmenting paths → min cut.
      Do it twice if the residual network isn't clean on the first pass.
- [ ] `S8.3` *Max-flow/min-cut counterexamples* — `[SAME]` was SS25 E6 word-for-word.
- [ ] `D8.4` *Network modelling tricks* — `[CONCEPT]` the `theory/09` transformations
      (multiple sources/sinks, node capacities); read, don't grind.

**Timed**
- [ ] **Endterm 2026 P6 complete (17 min)**. Drill the exact deliverables it asked:
  flow value at s or t; *every* residual arc with its capacity (forward u−f AND backward
  f — the backward arcs are where credits die); Ford–Fulkerson augmentations documented
  as path + amount + changed arcs; the min cut as two node sets with its capacity; and
  the one-sentence integrality argument (integral capacities → augment by integral
  amounts → integral max flow exists).
- Done when: you can produce a correct residual network on the first pass, twice in a row.

### Day 10 (Mon 15 Sep) — TSP & approximation
- [ ] [theory/10-tsp-and-approximation.md](theory/10-tsp-and-approximation.md) + [theory/10a-tsp-lesson.md](theory/10a-tsp-lesson.md)

**Warm-up ladder — sheet 9 + CE, untimed**
- [ ] `T9.1` *Euler vs Hamilton* — `[DRILL]` the definitions that MC items live on
      (edge-once vs node-once; which is NP-complete).
- [ ] `D9.1` *TSP-Approximation* — `[EXAM]` MST-doubling and Christofides executed on a
      graph; matches SS24 P6 and SS23 E6.
- [ ] `T9.2` *Filling of ATMs* — `[EXAM]` the "which problem class is this?" question —
      literally SS24 P6b's format.
- [ ] `S9.1` *Two Travelers* · `S9.2` *Slitherlink* · `D9.2` *Scooters* — `[EXAM]`
      spares; pick one if the SEC/MTZ formulations still feel wobbly.

**Timed**
- [ ] SS24 P6 or SS23 E6 (whichever you haven't redone recently); then redo **2026 P1f**
  (the 1.5-approximation MC: cost 30 ⇒ OPT ≥ 30/1.5 = 20 and OPT ≤ 30).
- Christofides = 3/2 (matching on odd-degree MST vertices); MST-doubling = 2. Past papers
  mislabel MST-doubling as "Christofides" — execute what the wording describes.
- Done when: approximation-ratio arithmetic (given ratio + returned cost, bound OPT both
  ways) is automatic.

### Day 11 (Tue 16 Sep) — Convexity & unconstrained optimization
First half of P7's 22 credits: everything before the KKT machinery.
- [ ] [theory/11-nonlinear-and-convex-kkt.md](theory/11-nonlinear-and-convex-kkt.md) (convexity part) + [theory/11a-nonlinear-unconstrained-lesson.md](theory/11a-nonlinear-unconstrained-lesson.md) + [cram-nonlinear-convex.md](cram-nonlinear-convex.md)

**Warm-up ladder — sheets 10/11 + CE-10, untimed**
- [ ] `D10.2` *Unconstrained Optimization* — `[EXAM]` gradient → critical points →
      Hessian classification by minors, eigenvalue fallback. Two or three functions
      until the minor test is automatic. (SS24 P7a's exact task.)
- [ ] `T11.1` *Convex Functions – Examples* — `[DRILL]` warm-up for the definition work.
- [ ] `T11.2` *Convex functions* — `[EXAM]` **prove convexity from the definition** —
      the P7d and SS25 E7b task. Do this one properly; it's the day's core rep.
- [ ] `D10.1` *Topological Properties* — `[CONCEPT]` convex sets vs convex functions.
- [ ] `T10.1` *Gradient Descent vs Newton* — `[CONCEPT]` read only; MC fodder.

**Timed**
- [ ] **Endterm 2026 P7 a–d only (~11 min)**: state Slater; concavity of
  log(1+x₁)+α·log(1+x₂) via the Hessian; feasible set convex + Slater holds; and the
  from-the-definition proof that max{x₁,x₂} is convex. Memorize that last proof pattern —
  "directly from the definition" is a standing exam phrase.
- Done when: you can classify convex/concave via Hessian for 2×2 cases fast, and write
  the λ-definition proof for max{·,·} without notes.

### Day 12 (Wed 17 Sep) — KKT & Slater *(second half of P7)*
- [ ] [theory/11b-convexity-kkt-lesson.md](theory/11b-convexity-kkt-lesson.md) — Parts 3–9.

**Warm-up ladder — sheet 10 + CE-10, untimed**
- [ ] `T10.2` *Lagrange multipliers* — `[DRILL]` build the Lagrangian, equalities only.
      Start here.
- [ ] `D10.3` *KKT-conditions* — `[EXAM]` the full system with the tight/not-tight case
      split. The core rep of the day.
- [ ] `T10.3` *KKT* — `[EXAM]` second rep, only if `D10.3` felt shaky.
- [ ] (`T11.3` *Hedge* and `D11.1–3` stay `[SKIP]` — online optimization, never examined.)

**Timed**
- [ ] SS25 E7 *Minimal Circle Enclosure*, all parts — the model KKT question.
- [ ] **Endterm 2026 P7 e–g (~11 min)**: rewrite max as min, Lagrangian, full KKT
  list (stationarity, primal/dual feasibility, complementary slackness), show the budget
  constraint is active, then the three-case μ analysis over α — interior point vs the two
  boundary points, each with its α-range — and the "KKT + convex ⇒ global optimum"
  closing sentence.
- Done when: the case-analysis bookkeeping (which multipliers vanish in which case) comes
  out organized on the first attempt, not reconstructed from scratch.

### Day 13 (Thu 18 Sep) — Column generation + MC sweep
- [ ] [theory/07-column-generation.md](theory/07-column-generation.md) — RMP, pricing problem, when to stop. It was absent
  five papers running, which is precisely why it can't be skipped anymore. Target: write
  the RMP and pricing problem for the cutting-stock example without help.
- [ ] `D7.2` *Vacation* — `[SAME]` was SS21 A7 word-for-word (23 pts). The one full rep.
- [ ] MC sweep: the whole [mc-question-bank.md](mc-question-bank.md) against [mc-answers.md](mc-answers.md), timed at ~1 min/question.
  Redo **endterm 2026 P1 a–h** cold and score it with the official solution. Log every
  miss — each one names the theory section to reread tonight.
- Done when: MC bank ≥ 90% and every 2026 P1 item is both answered and *explained*.

### Day 14 (Fri 19 Sep) — Full dress rehearsal
- [ ] **Morning: a complete past endterm you haven't redone recently (SS24 or SS23),
  120 min, strict conditions** — calculator only, no notes, answers documented.
- [ ] Score it with its solution PDF; convert to /121 to estimate where you stand.
- [ ] Afternoon: patch the two weakest problems with their warm-up ladders.
- [ ] Evening: rebuild your one-page exam card from memory, then diff it against
  [exam-day-card-2page.pdf](exam-day-card-2page.pdf). What you couldn't reproduce is tomorrow-morning reading.

---

## After Day 14

If the retake is more than two weeks out, loop Days 4–12 with fresh past papers
(SS21/23/24/25 all sit in `exams/endterm/`) — one full timed paper every 3–4 days,
topic-day rotation in between, always weakest topic first.

## Standing rules (unchanged, still true)

- Timebox exam problems to their credit count in minutes; running over is data.
- Warm-ups untimed with solutions allowed; papers timed with none.
- **Write the approach even when the answer is obvious — undocumented answers score 0.**
  This is printed on page 1 of the exam and it is not a formality.
- No red/green ink, no pencil — practice in the pen you'll use.
