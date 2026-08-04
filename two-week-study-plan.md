# Operations Research — endterm plan (final 6 days)

**Exam: Friday 7 August 2026, 120 min, ~120 points, 7 exercises.**
Remaining study days: Sat 1 Aug → Thu 6 Aug.

Done so far: LP geometry (`theory/01`) and simplex mechanics (`theory/02`).
Everything else is cold. This plan is triaged accordingly — see [exam-outline.md](exam-outline.md)
for what each exercise slot actually looks like.

## How this plan is now organised

**One exercise slot per day, ordered by points-per-hour — not by lecture order.**

The old plan marched through topics 1→11 and paired each with an exercise sheet. With six
days left that spends your time in the wrong places. New rules:

- **Theory file → warm-up ladder → past papers.** Never straight from theory to a paper.
  The ladder builds the component skills; the paper tests them under exam conditions.
- **Theory file first, but only the Procedures + Formula box.** Definitions sections are
  reference, not reading.
- **Timebox every past-paper problem to its point count in minutes.** 22 points = 22 minutes,
  then stop and check. Running over is data about where you're weak.
- **Warm-ups are untimed, papers are timed.** Warm-ups are for understanding — look at the
  solution when stuck. Papers are for performance — no solution until the clock stops.
- **Write the approach out even when you can see the answer.** Undocumented answers score zero.

## Exercise notation

```
T1.1, T5.2   sheet, "Training Exercises"        exercises/<topic>/sheet-NN-exercises.pdf
S1.1, S5.3   sheet, "Self-study Exercises"      same file, later pages
D1.1, D5.2   central-exercise demo              central exercises/<topic>/ce-NN-demo.pdf
```

Sheet and CE numbers **do not match** the `theory/` chapter numbers. The folder names do —
navigate by folder, not by number.

## The finding that should change how you work

**The endterm recycles central-exercise demos verbatim.** Not "similar" — the same text:

| Demo / sheet | Appeared as | How close |
|---|---|---|
| **D5.2** School Planning | **SS25 E4** (21 pts) | word-for-word, parts a–e |
| **S8.3** Max-flow/min-cut counterexamples | **SS25 E6** (14 pts) | word-for-word, same graphs |
| **D7.2** Vacation (Georgina, column generation) | **SS21 A7** (23 pts) | word-for-word |

That's 58 points across three papers lifted straight from material you already have. So the
warm-up ladders below are not just preparation for the papers — **some of them are the papers.**
Every demo marked `[SAME]` gets done properly, not skimmed.

## How the warm-ups are rated

Each exercise below is tagged for how much it resembles a real exam question:

- `[SAME]` — has appeared on a paper essentially verbatim. Highest priority.
- `[EXAM]` — same format, same difficulty, different scenario. This is the target level.
- `[DRILL]` — below exam level. Builds one component skill in isolation. Fast.
- `[CONCEPT]` — teaches an idea the exam assumes but never asks in this form. Read, don't grind.

If you are short on time, do the `[SAME]` and `[EXAM]` items and skip the rest.

## The triage logic

| Slot | Topic             | Pts   | Cost from cold | Verdict                                         |
| ---- | ----------------- | ----- | -------------- | ----------------------------------------------- |
| E5   | Branch & Bound    | 13–16 | half day       | **do first** — pure mechanics                   |
| E3   | Duality + CS      | 6–19  | half day       | **do first** — 3 procedures, done               |
| E4   | IP modelling      | 21–35 | full day       | **do first** — biggest block on every paper     |
| E7   | Nonlinear + KKT   | 12–23 | 1.5 days       | **do** — second largest, but expensive          |
| E2   | Sensitivity       | 14–19 | half day       | **do** — fiddly, and simplex half is already in |
| E6   | Combinatorial     | 12–18 | half day       | **do the mechanics only**, skip the proofs      |
| E1   | Multiple choice   | 14–16 | free           | **last** — it free-rides on everything above    |
| E8   | Column generation | 0–23  | full day       | **dropped** — absent since SS21                 |

E3 + E4 + E5 alone is ~52 points of mechanical procedure. That's the floor this plan
protects. E7 takes you to ~72. Everything after is upside.

## Explicitly dropped

Not "if there's time" — **dropped**, so you don't feel the pull:

- **Column generation** (`theory/07`, D7.2, SS21 A7). Four papers without it. If it appears,
  write the RMP and the pricing-problem sentence from the Day 6 skim and move on.
- **Gomory cuts as a procedure** — T6.2, S6.2, D6.2, D6.3. Cuts appear only as MC items.
- **Hungarian method** — T8.3 Dredgers. **DGS auction** — S8.2, D8.2. Never examined.
- **Online optimization** — T11.3, all of CE-11 (D11.1–D11.3). Never examined.
- **Two-phase and Big-M simplex from scratch.** Never asked — the exam hands you a tableau.
- **The SS21 retake paper, entirely.** Too old to be representative; the file has been removed
  from the repo. The evidence base is now SS21 endterm, SS23, SS24, SS25 — all seven-exercise
  papers. Where the retake was the only source for a drill, a replacement is named in place
  (Day 5's sensitivity chain moves to `D3.2`, `S3.5` and the midterms).

## Corrections to carry into the exam

- **Christofides = 3/2-approximation** (matching on odd-degree MST vertices).
  **MST-doubling = 2-approximation.** Past papers mislabel MST-doubling as "Christofides" —
  if the wording matches theirs, execute MST-doubling. That is what gets graded.
- **The TU sufficient condition is never named in the lecture.** Reproduce the three numbered
  conditions; citing "Ghouila-Houri" earns nothing.
- **Midterm SS26 P2d sample solution is wrong.** The cone it gives for unbounded `c` is
  actually where the LP is bounded.
- **Dynamic programming is not dropped by the chair.** SS23 E4 was a full 14-pt DP question.

---

## Day 1 — Sat 1 Aug — **E4: IP modelling** (21–35 pts)

The largest block on every single paper, and it depends on nothing else. Start here.

- [x] `theory/05a` — the lesson: context, the answer template, and the patterns that are
      actually examined. Read this first.
- [x] `theory/05` — the full trick catalogue. Copy each pattern out by hand.
**Warm-up ladder — sheet 5 + CE-05, untimed, ~2h**
- [x] `S5.1` *Logic* — `[DRILL]` pure "translate this logical statement into inequalities".
      The Part 3 material of `05a` with nothing else attached. Start here.
- [x] `T5.1` *Modelling Tricks – OR* — `[DRILL]` the either-or / big-M disjunction in
      isolation. Exactly the pattern behind SS24 P4e.
- [x] `T5.2` *Caffeine* — `[EXAM]` short word problem, full model.
- [x] `S5.2` *Party Planning* — `[EXAM]` word problem with logical side conditions.
- [x] `D5.1` *Organic Farmer* — `[EXAM]` crop rotation: history conditions ("grown in each of the two preceding years"), the hardest index-range work in the course. `theory/05` §15.
- [x] **`D5.2` *School Planning*** — `[SAME]` **this is SS25 E4 word-for-word.** Do it properly, then check against the SS25 solutions to see exactly what the graders wanted.
- [ ] `S5.3` *Dutch Petroleum* — `[EXAM]` piecewise-linear pricing. Only if time; `theory/05` §20.
- [ ] `T5.3` *Exam Preparation* — `[EXAM]` extra, same level as the above.

**Then the papers, timeboxed**
- [x] SS25 E4 *School Planning* (21 pts / 21 min) — now a re-run of D5.2 under the clock
- [x] SS24 P4 *Tour d'Allemagne* (22 pts / 22 min)
- [x] SS23 E2 *Ice cream production* (22 pts / 22 min)
- [ ] Then the monster, for stamina: SS21 A3 *Biergärten* (35 pts).
- [ ] **End-of-day test.** From blank paper, write the encoding for each:
  1. assignment — "each `j` to exactly one `i`"
  2. capacity linked to an open decision — `Σⱼ x_{i,j} ≤ cᵢ yᵢ`
  3. "only if built" — `x_{i,j} ≤ yᵢ`
  4. mutual exclusion from a parameter — `yᵢ + yᵢ' ≤ 1` for all pairs violating a threshold
  5. implication "if A > k then B ≥ m" — helper `z ∈ {0,1}` + two big-M rows
  6. two objectives in one — weighted sum, and say why the sign flips
- [ ] Every helper variable gets a sentence in words. No exceptions — this is graded.

## Day 2 — Sun 2 Aug — **E3: Duality** + **E5: Branch & Bound** (~30 pts)

The highest points-per-hour day of the plan. Both are short procedures with fixed shapes.

**Morning — E3 (15 pts)**
- [x] `theory/04a` — the lesson: where the dual comes from, the SOB table, CS derived, the
      refutation procedure, worked SS25 E3. Read this first.
- [x] `theory/04` — the reference. Formula box only.
- [x] Reproduce the SOB table from memory before touching a paper.

*Warm-up ladder — sheet 4 + CE-04, ~1h15*
- [x] `D4.1` *Dual Problem* — `[DRILL]` mechanical dual derivation, nothing else. Do 2–3 until
      the SOB table is automatic.
- [x] `T4.2` *Duality* — `[DRILL]` same, with mixed constraint types and free variables.
- [x] `T4.1` *Duality & Complementary Slackness* — `[EXAM]` this is the exam's exact shape:
      derive dual → state CS → use it.
- [x] `S4.2` *Duality & Complementary Slackness* — `[EXAM]` second rep of the same shape.
- [x] `D4.2` *Primal-Dual* — `[EXAM]` extra rep if the CS certificate still feels shaky.
- [x] `T4.3` *Rock-Paper-Scissors* — `[CONCEPT]` zero-sum games. Read only; no endterm has
      asked for one. `D4.3` *Transportation Problem* — `[CONCEPT]`, same call.

*Then the papers, timeboxed*
- [x] SS25 E3 (15), SS24 P2 (16), SS23 E1 (19).
- [x] Memorise the 4-step optimality certificate: assume optimal → CS forces which duals are
      zero → solve the reduced dual system → show it's infeasible or violates a dual constraint.
- [x] Know the answer to *"is (D) unbounded, infeasible, or finite — without solving it?"*

**Afternoon — E5 (14 pts)**
- [x] `theory/06a` — the lesson: why the relaxation bounds, why branching loses no integer
      point, the three pruning rules, a full worked tree, FIFO/LIFO traced, worked SS25 E5.
      Read this first.
- [x] `theory/06` — the reference. Procedures + Formula box only.

*Warm-up ladder — sheet 6 + CE-06, ~1h*
- [x] `D6.1` *Branch-and-Bound* — `[EXAM]` a full worked tree. Do this one first and slowly;
      it is the template for everything else.
- [x] `T6.1` *Knapsack Branch-and-Bound* — `[EXAM]` B&B on a knapsack, so the relaxation is
      solved by the greedy ratio rule rather than graphically. Good second angle.
- [x] `S6.1` *Branch-and-Bound* — `[EXAM]` third rep. Stop here if the pruning rules are solid.
- [x] `T6.3` *Staff Scheduling* — `[DRILL]` an IP model, so it doubles as Day 1 revision.
- [x] Skip `T6.2`, `S6.2`, `D6.2`, `D6.3` — all Gomory cuts, dropped.

*Then the papers, timeboxed*
- [ ] SS25 E5 (the ink-blot reconstruction — this is the current format), SS24 P3,
      SS23 E3, SS21 A4.
- [ ] **End-of-day test.** Given any node: state the three reasons to prune, and the FIFO and
      LIFO traversal order of a 7-node tree, out loud.

## Day 3 — Mon 3 Aug — **E7a: Nonlinear, unconstrained** (12–23 pts)

- [x] `theory/11a` — the lesson: gradient and Hessian built from the one-variable case,
      definiteness by det/trace and by minors, the eigenvalue fallback, the convexity
      shortcut, worked SS24 P7a and SS23 E7. Read this first.
- [x] `theory/11` first half — the reference. Procedures 1–2 and the Formula box.

**Warm-up ladder — CE-10 + sheet 10, ~1h**
- [x] `D10.2` *Unconstrained Optimization* — `[SAME-TYPE]` gradient → critical points → Hessian
      classification. This is exactly SS24 P7a's task. Do two or three functions until the
      minor test is automatic.
- [x] `T10.1` *Gradient Descent vs. Newton's Method* — `[CONCEPT]` read only. Never asked as a
      computation, but it is MC fodder (SS25 1h was about gradient descent convergence).

**Then the papers, timeboxed**
- [x] SS24 P7a (`x² + y² + e^{2x} − (8x+6y) + 2xy`), SS21 A6.
- [x] Then the parameterised one: SS23 E7 — classify all critical points of
      `f_α(x,y) = −¼x⁴ − ¼y⁴ + αxy` **as a function of α**. Expect a case split, not one answer.
**Rest of today — the graph block** (moved up from Day 5)

Max-flow / min-cut is the cheapest 14 points on the paper and the demo *is* the exam question.
Do it while the day still has hours in it.

- [x] `theory/09a` — the lesson: flows and cuts from zero, why cut capacity is forward-only,
      max-flow/min-cut as the LP duality you did on Day 2, why the residual backward arc
      exists, Ford-Fulkerson, worked SS25 E6 counterexamples. Read this first.
- [x] `theory/09` — the reference. Procedures 1–3 and the Formula box.
- [x] **`S8.3`** *Max-flow and min-cut counterexamples* — `[SAME]` **this is SS25 E6
      word-for-word, same graphs.** Do it properly, then check against the SS25 solutions.
- [ ] `T8.2` *Maximum Flow* — `[DRILL]` only if Ford-Fulkerson itself is shaky.
- [ ] `D8.4` *Tips and tricks for network modeling* — `[CONCEPT]`, 10 min skim. The five
      transformations (node splitting, lower bounds, multiple sources) in `theory/09`.
- [ ] Paper: SS21 A5 (flow part).
- [ ] Note the recurring verb: **"give a counterexample"**. You are usually disproving, not proving. Practise producing a small graph that breaks a statement.

## Day 4 — Tue 4 Aug — **E2: Sensitivity** + **rest of E6** (~30 pts)

**Morning — E2 (16 pts).** Your simplex mechanics are already in from Day 2;
this is the layer on top.
- [ ] `theory/03a` — the lesson: tableau anatomy, which half of the tableau each change
      breaks, the sign convention, shadow prices as Day 2's dual solution, RHS and cost
      ranging, pricing a new column, worked SS25 E2. Read this first.
- [ ] `theory/03` — the reference. Procedures 2–6 and the Formula box.

*Warm-up ladder — sheet 3 + CE-03, ~1h15*
- [ ] `T3.3` *Sensitivity* — `[DRILL]` ranging in isolation, no story. Start here.
- [ ] `T3.2` *Lemonade Production* — `[EXAM]` **closest match on the sheets to SS25 E2** — a
      lemonade factory, an optimal tableau handed to you, questions read off it. Different
      numbers, same task.
- [ ] `D3.2` *Waldgeist Distillery* — `[EXAM]` the full sensitivity chain in a story wrapper.
      **With retake-2021 out of scope this is now your main source for the complete chain** —
      shadow price → validity range → range violated → new shadow price. Do it properly.
- [ ] `S3.5` *PopCo* — `[EXAM]` second rep of the chain. Promoted from optional.
- [ ] `D3.1` / `T3.1` / `S3.2` *Revised Simplex* — `[CONCEPT]` **you need the Row 0 formula,
      not the algorithm.** No endterm has ever asked for a revised-simplex iteration; the SS25
      solution merely *cites* `Row0 = c_B B⁻¹A − c`. Read for the formula, don't drill iterations.

*Then the papers, timeboxed*
- [ ] SS25 E2 (16) — the tableau with deliberately wrong Row 0. The current format.
- [ ] SS21 A1 *Backmischung* (16) — sensitivity from an optimal tableau, including the
      RHS-range question ("within what limits can the flour stock move without changing the
      basis?").
- [ ] Midterm SS25 P5 and midterm SS26 P4, both *Sensitivity Analysis* — the midterms carry
      more sensitivity than the endterms do, and they are the same task.
- [ ] Memorise the Row 0 formula and the reduced cost of a new column. These two carry the
      whole question.

**Afternoon — rest of E6 (14 pts).** Flow is done. Four topics still rotate through this slot
and you cannot learn all four properly. Learn the **mechanical core of each**, skip every proof.
Each block is *one* warm-up then straight to the paper question. No ladders — there isn't time.

- [ ] **Matroids** — 45 min. The three axioms, verbatim; all bases equicardinal.
      → `D8.3` *Matroids* `[EXAM]`, then `T8.1` *Matroids* `[EXAM]`. Papers: SS24 P5, SS23 E5.
- [ ] **Total unimodularity** — 30 min. The three sufficient conditions verbatim; TU + integral
      `b` ⇒ integral polyhedron ⇒ IP in P.
      → `D8.1` *Unimodularity* `[EXAM]`, then `S8.1` *TU and dual of a Matroid* `[EXAM]` —
      that one covers TU and matroids together. Paper: SS21 A5b.
- [ ] **Knapsack DP** — 30 min. Fill the table; know `O(nW)` vs `O(nV)` and run the smaller.
      → `D7.1` / `T7.1` *Cutting* `[EXAM]` — the DP table drill. `S7.2` *Bin Packing* `[DRILL]`
      is modelling, not DP; skip unless Day 1 felt weak. Paper: SS23 E4.
- [ ] **TSP** — 30 min. MST-doubling procedure; Christofides = 3/2 vs doubling = 2; both need
      the triangle inequality.
      → `D9.1` *TSP-Approximation* `[EXAM]` — execute the approximation. Then `T9.1`
      *Euler vs Hamilton* `[DRILL]` for the definitions, and `T9.2` *Filling of ATMs* `[EXAM]`,
      which asks "which problem class is this?" — the exact SS24 P6b question type.
      Papers: SS24 P6, SS23 E6.
- [ ] Skip `T8.3` *Dredgers* (Hungarian), `S8.2` / `D8.2` *DGS* — never examined.

**Evening — convexity only, no KKT (~1h).** This is the cheap half of tomorrow's topic, and
doing it tonight means Wednesday isn't first contact with anything.
- [ ] `theory/11b` **Parts 1–2 only** — convex sets vs convex functions, and the four routes to
      proving convexity. Stop before Part 3.
- [ ] `T11.1` *Convex Functions – Examples* — `[DRILL]` decide convexity for a list.
- [ ] `T11.2` *Convex functions* — `[EXAM]` **prove convexity from the definition.** This is
      SS25 E7b's task. Drill until the `f(λx+(1−λ)y) ≤ …` chain is muscle memory.
- [ ] `D10.1` *Topological Properties and Convex Sets* — `[CONCEPT]` skim. Know convex set vs
      convex function, and closed + bounded ⟹ compact.

## Day 5 — Wed 5 Aug — **E7b: Slater and KKT** (the last new topic)

Moved to the end deliberately: it is the hardest block on the paper. Convexity is already in
from last night, so today is only the KKT machinery.

- [ ] `theory/11b` **Parts 3–9** — standard form, the Lagrangian, the four KKT blocks, the
      complementary-slackness case split, Slater vs LICQ, worked SS25 E7.
- [ ] `theory/11` second half — the reference. Procedures 4–7 and the Formula box.

**Warm-up ladder — sheet 10 + CE-10, ~2h**
- [ ] `T10.2` *Lagrange multipliers* — `[DRILL]` build the Lagrangian, equalities only. Start here.
- [ ] `D10.3` *KKT-conditions* — `[EXAM]` full system with inequalities and the tight/not-tight
      case split. The core rep.
- [ ] `T10.3` *Non-linear optimization – KKT conditions* — `[EXAM]` second rep.
- [ ] Skip `T11.3` *Hedge Algorithm* and all of CE-11 — online optimization, dropped.

**Then the papers, timeboxed**
- [ ] SS25 E7 *Minimal Circle Enclosure*, all parts. The model question.
- [ ] SS24 P7b — KKT with a single linear constraint, Slater verified. Shorter, second rep.

**If KKT does not fully land, bank the parts that don't need it.** SS25 E7 parts (a), (b) and
(c) are *definition, norm proof, formulate* — pure convexity, no KKT, and they were roughly a
third of the marks. Write those cold and move on rather than stalling on the Lagrangian.

- [ ] **End-of-day test.** Without notes:
  - prove a given function is convex from the definition
  - write the Lagrangian, then the KKT system, then the tight / not-tight case split
  - state: **Slater ⇒ a KKT point is a global optimum**; **LICQ ⇒ candidates only**

## Day 6 — Thu 6 Aug — **E1: Multiple choice** + final rehearsal

MC free-rides on all five previous days, which is why it is last, not first.

- [ ] Drill SS24 P1 and SS25 E1. For **every** option, write one line on why it is true or false.
- [ ] Read the **True/false facts** section of all eleven `theory/` files (~120 statements).
- [ ] Read the **Formula box** of all eleven files. Nothing else.
- [ ] 20-minute column-generation skim (`theory/07` Formula box only) — enough to write the RMP
      and one sentence on the pricing problem if it appears. Then let it go.
- [ ] Recall out loud, no notes:
  1. Standard form; what slack / surplus / artificial variables do
  2. Simplex optimality and unboundedness criteria
  3. The SOB primal→dual table
  4. Complementary slackness, both directions
  5. B&B pruning rules — for max **and** for min
  6. TU + integral `b` ⇒ integral polyhedron ⇒ IP in P
  7. Knapsack DP: `O(nW)` vs `O(nV)`
  8. Matroid axioms; bases are equicardinal
  9. Max-flow = min-cut; cut capacity counts forward arcs only
  10. SEC (exponential, tight) vs MTZ (polynomial, weak)
  11. Christofides 3/2 vs MST-doubling 2
  12. Hessian classification + the eigenvalue fallback
  13. KKT's four blocks; Slater ⇒ global, LICQ ⇒ candidates
- [ ] No new topics. Pack calculator, ruler, analog dictionary, ID. Sleep.

---

## Appendix — every exercise, rated

`[SAME]` verbatim on a paper · `[EXAM]` same level/type · `[DRILL]` one component skill ·
`[CONCEPT]` read only · `[SKIP]` never examined

| Slot | Exercise | Rating | Matches |
|---|---|---|---|
| **E4** | `D5.2` School Planning | `[SAME]` | **SS25 E4, word-for-word** |
| E4 | `D5.1` Organic Farmer | `[EXAM]` | history/index-range conditions |
| E4 | `T5.2` Caffeine · `S5.2` Party Planning · `T5.3` Exam Preparation | `[EXAM]` | SS23 E2, SS24 P4 |
| E4 | `S5.3` Dutch Petroleum | `[EXAM]` | piecewise-linear pricing |
| E4 | `S5.1` Logic · `T5.1` Modelling Tricks – OR | `[DRILL]` | the SS24 P4e pattern |
| E4 | `T6.3` Staff Scheduling | `[DRILL]` | IP model, doubles as revision |
| **E3** | `T4.1` · `S4.2` Duality & Compl. Slackness | `[EXAM]` | SS25 E3, SS24 P2, SS23 E1 |
| E3 | `D4.2` Primal-Dual | `[EXAM]` | extra CS-certificate rep |
| E3 | `D4.1` Dual Problem · `T4.2` Duality | `[DRILL]` | SOB table only |
| E3 | `T4.3` Rock-Paper-Scissors · `D4.3` Transportation | `[CONCEPT]` | no endterm has asked |
| **E5** | `D6.1` · `S6.1` Branch-and-Bound | `[EXAM]` | SS25 E5, SS24 P3, SS23 E3 |
| E5 | `T6.1` Knapsack B&B | `[EXAM]` | greedy-ratio relaxation variant |
| E5 | `T6.2` · `S6.2` · `D6.2` · `D6.3` Cuts | `[SKIP]` | MC items only |
| **E2** | `T3.2` Lemonade Production | `[EXAM]` | closest sheet match to **SS25 E2** |
| E2 | `D3.2` Waldgeist Distillery · `S3.5` PopCo | `[EXAM]` | the full sensitivity chain |
| E2 | `T3.3` Sensitivity | `[DRILL]` | ranging in isolation |
| E2 | `D3.1` · `T3.1` · `S3.2` Revised Simplex | `[CONCEPT]` | formula yes, iterations no |
| **E6** | `S8.3` Max-flow/min-cut counterexamples | `[SAME]` | **SS25 E6, word-for-word** |
| E6 | `D8.3` · `T8.1` Matroids | `[EXAM]` | SS24 P5, SS23 E5 |
| E6 | `D8.1` Unimodularity · `S8.1` TU + matroid dual | `[EXAM]` | SS21 A5b |
| E6 | `D7.1` · `T7.1` Cutting | `[EXAM]` | SS23 E4 knapsack DP |
| E6 | `D9.1` TSP-Approximation | `[EXAM]` | SS24 P6, SS23 E6 |
| E6 | `T9.2` Filling of ATMs | `[EXAM]` | "which problem class?" = SS24 P6b |
| E6 | `T8.2` Maximum Flow · `T9.1` Euler vs Hamilton | `[DRILL]` | mechanics/definitions |
| E6 | `T7.2` Hiking trip · `S7.1` Cutting · `S7.2` Bin Packing · `S9.1` Two Travelers · `S9.2` Slitherlink · `D9.2` Scooters | `[EXAM]` | spares if a block feels weak |
| E6 | `D8.4` Network modelling tricks | `[CONCEPT]` | `theory/09` transformations |
| E6 | `T8.3` Dredgers · `S8.2` · `D8.2` DGS | `[SKIP]` | Hungarian / DGS |
| **E7** | `T11.2` Convex functions | `[EXAM]` | **SS25 E7a**, prove from definition |
| E7 | `D10.3` · `T10.3` KKT conditions | `[EXAM]` | SS25 E7d–f, SS24 P7b |
| E7 | `D10.2` Unconstrained Optimization | `[EXAM]` | SS24 P7a, SS23 E7, SS21 A6 |
| E7 | `T11.1` Convex Functions – Examples · `T10.2` Lagrange multipliers | `[DRILL]` | components |
| E7 | `D10.1` Topological Properties | `[CONCEPT]` | sets vs functions |
| E7 | `T11.3` Hedge · `D11.1–3` CE-11 | `[SKIP]` | online optimization |
| **E8** | `D7.2` Vacation | `[SAME]` | **SS21 A7, word-for-word** — but dropped |

---

## Exam day — Fri 7 Aug

**Allowed:** non-programmable calculator, analog dictionary, ruler. No cheat sheet.

Scan all exercises first, then work in this order regardless of their numbering:

```
E1  Multiple choice   ~15 pts / 12 min   ← best pts/min, and never leave one blank
E3  Duality           ~15 pts / 15 min   ← most procedural, banks points early
E5  Branch & Bound    ~14 pts / 14 min
E6  Combinatorial     ~14 pts / 14 min   ← flow/counterexamples, your strongest of these
E4  IP modelling      ~22 pts / 25 min   ← largest; do it while you're still sharp
E2  Simplex/sens      ~16 pts / 18 min
E7  Nonlinear         ~20 pts / 22 min   ← LAST. Write the convexity parts first, then KKT
                                ~20 min spare — spend it on E4 and E7
```

- Partial credit is real on E4 and E7. Write the setup even if you can't finish.
- On multiple choice: all-correct = 2 pts, one error = 1 pt, blank = 0. Always guess.
- Document the approach on everything. An undocumented correct answer scores zero.
