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
- **Days 1–3 are recorded as days; everything remaining is seven independent blocks**
  (B1–B7), cut to what the four in-scope papers say is most likely. Each block stands
  alone, so you can reorder or drop one without unpicking the rest.
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

Days 1–3 are done. Everything below is restructured into standalone blocks.

---
# Remaining work — cut to what pays

Three days left. This is **not** everything that could appear — it is what the four in-scope
papers say is most likely, in points-per-hour order.

| Block | Topic | Slot | Pts | Time | Why |
|---|---|---|---|---|---|
| **B1** | Sensitivity — papers only | E2 | 16 | 45 min | lessons + warm-ups already done |
| **B2** | Matroids | E6 | 14 | 45 min | full question on **SS23 and SS24** |
| **B3** | E6 insurance — TU · DP · TSP | E6 | 14 | 45 min | facts only, no exercises |
| **B4** | Convexity | E7 | ~7 of 20 | 1 h | cheap, and scores even if B5 fails |
| **B5** | Slater and KKT | E7 | ~13 of 20 | 2.5 h | SS25 put all 20 nonlinear pts here |
| **B6** | Multiple choice | E1 | 14–16 | 1.5 h | free-rides on all of the above |
| **B7** | Final rehearsal | — | — | 2 h | no new topics |

```
Tue 4 Aug   B1  B2  B3  B4
Wed 5 Aug   B5
Thu 6 Aug   B6  B7
```

**Ordering rule:** B4 before B5, B5 before B7. Everything else is free.

## What I cut, and the risk

- **Network flow tail** (`T8.2`, `D8.4`, SS21 A5). `S8.3` *is* SS25 E6 word-for-word and you've done it. The rest is polish.
- **Full TU / knapsack-DP / TSP blocks** — exercises and papers dropped, headline facts kept in B3. **Only one E6 topic appears per paper**, and matroids is the one that has appeared twice.
- **Everything already marked `[SKIP]`** — Gomory cuts, Hungarian, DGS, online optimization, column generation.

**The risk, stated plainly:** if E6 turns out to be a full TU, DP or TSP question, B3 gets you partial credit, not full. That's the trade — matroids at depth beats four topics at a skim.

---

## B1 — Sensitivity, papers only · E2 · 16 pts · 45 min

- [x] **SS25 E2** (16) — done already? If not, do it first; it's the current format.
- [ ] **SS21 A1** *Backmischung* (16) — sensitivity from an optimal tableau, with RHS ranging.
- [ ] **Midterm SS25 P5** / **SS26 P4** — the midterms carry more sensitivity than the endterms.

**Done when** you can read shadow prices off Row 0, range the RHS from a slack column, and price a new column, without notes.

---

## B2 — Matroids · E6 · 14 pts · 45 min

The most repeated E6 topic: a full 14-pointer on **both SS23 and SS24**, plus an SS25 MC item.

- [x] `theory/08a` — the lesson.
- [x] `D8.3` *Matroids*
- [x] Papers: **SS24 P5** (prove one, disprove one), **SS23 E5** (basis, equicardinality,
      symmetric difference).

**Done when** you can write the three axioms verbatim with the exchange direction right
(*smaller set grows, element comes from the larger*), and build a three-element counterexample.

---

## B3 — E6 insurance · 45 min · facts only

No exercises, no papers. Fifteen minutes each so you can **write something worth partial credit**
if the combinatorial slot isn't matroids.

**Total unimodularity** — `theory/08b` Part One, skim:
```
TU ⟺ every square submatrix has det ∈ {−1,0,+1}
TU + b integral ⟹ integral polyhedron ⟹ IP solvable in poly time
incidence matrix of a BIPARTITE or DIRECTED graph is TU     ← usually the whole answer
to disprove: find a ZERO-FREE 2×2 with |det| ≥ 2
```

**Knapsack DP** — `theory/08b` Part Two, skim + fill one small table:
```
B[i,w] = max{ B[i−1,w] , vᵢ + B[i−1, w−wᵢ] }
index by weight O(nW) or value O(nV) — run the SMALLER, and say why
backtrack: B[i,w] = B[i−1,w] ⟹ item i not taken
```

**TSP** — `theory/10a`, skim:
```
Euler = every EDGE once (easy)     Hamilton = every NODE once (NP-complete)
degree constraints alone = assignment problem, allows SUBTOURS
SEC: exponential, tight            MTZ: polynomial, weak
MST-doubling = 2                   Christofides = 3/2 (odd-degree matching)
both need the triangle inequality; papers mislabel doubling as Christofides
```

**Done when** you can write those three boxes from memory.

---

## B4 — Convexity · E7 · 1 h

The cheap half of the nonlinear block, and **it scores even if B5 never lands** — SS25 E7 parts
(a), (b), (c) are definition, norm proof and formulate, with no KKT at all.

- [ ] `theory/11b` **Parts 1–2 only**. Stop before Part 3.
- [ ] `T11.2` *Convex functions* — **prove convexity from the definition.** SS25 E7b's task.
- [ ] `T11.1` *Convex Functions – Examples* if the definition still feels slow.

**Done when** you can prove a norm is convex from the definition in three lines, and say why
`|λ| = λ` needs `λ ∈ [0,1]`.

---

## B5 — Slater and KKT · E7 · 2.5 h

The hardest block, alone on its own day. **SS25 put its entire 20-point nonlinear question here.**

- [ ] `theory/11b` **Parts 3–9**.
- [ ] `T10.2` *Lagrange multipliers* — build the Lagrangian, equalities only. Start here.
- [ ] `D10.3` *KKT-conditions* — full system with the tight/not-tight case split. The core rep.
- [ ] `T10.3` — second rep, only if D10.3 felt shaky.
- [ ] Paper: **SS25 E7** *Minimal Circle Enclosure*, all parts. The model question.
- [ ] Paper: **SS24 P7b** — shorter, one linear constraint.

**Done when**, without notes: the Lagrangian, the four blocks, the case split, and
**Slater ⇒ global optimum** vs **LICQ ⇒ candidates only, compare them**.

---

## B6 — Multiple choice · E1 · 14–16 pts · 1.5 h

- [ ] **[mc-question-bank.md](mc-question-bank.md)** — 47 questions, 188 statements, every
      topic. Answers are in **[mc-answers.md](mc-answers.md)**, so this is a real test —
      sit it closed-book first, then mark it.
- [ ] **SS24 P1** and **SS25 E1**. For *every* option, write one line on why it's true or false.
- [ ] Read the **True/false facts** section of the `theory/` files you've actually covered.

**Never leave one blank** — all-correct 2 pts, one error 1 pt, blank 0. Guessing dominates.

---

## B7 — Final rehearsal · 2 h

- [ ] **[exam-day-card.md](exam-day-card.md)** — every decision rule in the course, one page
      per exam slot. Read it end to end. This is the only thing you read on Friday morning.
- [ ] **Formula box** of the covered `theory/` files, if time.
- [ ] Recall out loud, no notes:
  1. Standard form; what slack variables do
  2. Simplex optimality and unboundedness criteria
  3. The SOB primal→dual table
  4. Complementary slackness, both directions
  5. B&B pruning rules — max **and** min
  6. TU + integral `b` ⇒ integral polyhedron ⇒ IP in P
  7. Knapsack DP: `O(nW)` vs `O(nV)`
  8. Matroid axioms; bases equicardinal
  9. Max-flow = min-cut; cut capacity counts forward arcs only
  10. SEC (exponential, tight) vs MTZ (polynomial, weak)
  11. Christofides 3/2 vs MST-doubling 2
  12. Hessian classification + the eigenvalue fallback
  13. KKT's four blocks; Slater ⇒ global, LICQ ⇒ candidates
- [ ] **No new topics.** Pack calculator, ruler, analog dictionary, ID. Sleep.

---



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
