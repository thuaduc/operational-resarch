# Multiple-choice bank — answers

Questions are in [mc-question-bank.md](mc-question-bank.md).
Score yourself with the real rule: **all correct 2 · one deviation 1 · otherwise 0.**

---

# §1 · LP basics and simplex

**Q1** — **1, 3.**
2 ✗ — the surplus column is `−1`, not `+1`, so it is *not* a basis column; that's why artificial variables exist.
4 ✗ — `min cᵀx = −max(−cᵀx)`; you negate the objective, not reuse it.

**Q2** — **1, 2, 3.**
4 ✗ — true for *some* objective only if the LP is bounded in that direction, and non-vertex points can also be maximisers when there are multiple optima.

**Q3** — **1, 2, 4.**
3 ✗ — if two distinct points are optimal, every convex combination of them is too, so you get infinitely many. Never exactly two.

**Q4** — **4 only.** Cycling requires a degenerate pivot — a basis change with no objective improvement.

**Q5** — **1, 2, 3.**
4 ✗ — that's degeneracy, and a degenerate BFS is perfectly feasible.

# §2–3 · Tableau reading and sensitivity

**Q6** — **1, 2, 3.** The tableau records the final state only, not the path taken.

**Q7** — **1, 2, 3.**
4 ✗ — valid only while the basis stays optimal, i.e. inside the RHS range.

**Q8** — **3, 4.**
1 ✗ — if the basis is unchanged, so is `π`.
2 ✗ — the basic variables move by `B⁻¹Δb`, not by `π`. `π` gives only the *objective* change.

**Q9** — **2, 3.**
1 ✗ — inside the range *neither* the plan nor `z` changes; you're making none of it.
4 ✗ — one-sided: `c_j ≤ yᵀa_j`.

**Q10** — **1, 2, 4.**
3 ✗ — degeneracy is one point described by several bases; multiple optima is several points with the same `z`.

**Q11** — **1, 2** — the same statement twice.
3 ✗ — irrelevant. 4 ✗ — that's the whole point of pricing a column.

# §4 · Duality

**Q12** — **1, 2, 4.**
3 ✗ — infeasible primal ⟹ dual is unbounded **or** infeasible. Both-infeasible is a real case.

**Q13** — **1, 2, 4.**
3 ✗ — sign restrictions are not counted as constraints.

**Q14** — **1, 2, 3.**
4 ✗ — `x ≤ 0` is bizarre, so its dual constraint takes the *bizarre* direction.

**Q15** — **1, 2, 3.**
4 ✗ — LPs have no duality gap; that's strong duality. Gaps are an *integer* programming phenomenon.

**Q16** — **2, 3, 4.**
1 ✗ — the entire point is that you never solve the LP.

# §5 · IP modelling and complexity

**Q17** — **1, 2, 4.**
3 ✗ — rounding frequently lands outside the feasible region.

**Q18** — **1, 3.**
2 ✗ — the integer sets are identical; that's what "same IP" means.
4 ✗ — the disaggregated linking form has *more* constraints and is stronger.

**Q19** — **1, 2, 3.**
4 ✗ — `{x > K}` is not closed, so it isn't a polytope. Only for integer `x`.

**Q20** — **1, 2.**
3 ✗ — Klee–Minty cubes force `2ⁿ` vertices. LP is in P via ellipsoid/interior-point, not simplex.
4 ✗ — NP-hard does not require membership in NP; NP-**complete** does.

**Q21** — **1, 2, 3.**
4 ✗ — LP is in P.

# §6 · Branch and bound and cuts

**Q22** — **1, 2, 3.**
4 ✗ — that's the *minimisation* rule. A node above the incumbent is exactly the one worth exploring.

**Q23** — **1, 2, 3, 4.** All four hold — no integer lies strictly between 2 and 3, and the two half-spaces don't overlap.

**Q24** — **2, 4.**
1 ✗ — by definition a valid inequality never removes an integer point.
3 ✗ — no such guarantee.

**Q25** — **1, 2, 3.**
4 ✗ — depends entirely on the instance. On SS25's tree FIFO happened to be shorter; that's not a rule.

# §7 · Total unimodularity

**Q26** — **1, 2, 3.**
4 ✗ — determinant `0` is allowed, and that means singular.

**Q27** — **1, 4.**
2 ✗ — appending an identity block preserves TU.
3 ✗ — bipartite incidence matrices are TU, but not every TU matrix is one.

**Q28** — **1, 2, 4.**
3 ✗ — an odd cycle breaks it; the graph must be bipartite.

# §8 · Matroids

**Q29** — **2, 3.**
1 ✗ — not hereditary: `{a,b} ∈ I` but `{a} ∉ I`.
4 ✗ — `∅ ∉ I`, since `|∅| = 0` is even. Fails axiom 1.

**Q30** — **1, 3, 4.**
2 ✗ — that's the direction reversed, the standard error.

**Q31** — **1, 2, 3.**
4 ✗ — it's an independence system, but generally not a matroid. That's exercise `T8.1`.

# §9 · Knapsack and approximation

**Q32** — **1, 3, 4.**
2 ✗ — **pseudo**polynomial. `W` is a number; writing it takes `log W` bits.

**Q33** — **1, 2, 3.**
4 ✗ — max clique, set cover and max independent set are not in APX at all.

# §10 · Network flow

**Q34** — **1, 3.**
2 ✗ — backward arcs contribute nothing; they cannot carry anything toward `t`.
4 ✗ — the *value* is unique, the cut achieving it need not be.

**Q35** — **1, 2, 3.**
4 ✗ — greedy alone can get stuck strictly below the maximum.

**Q36** — **1, 2, 4.**
3 ✗ — this is SS25 E6a's counterexample: two odd flows merging give an even one.

# §11 · TSP

**Q37** — **1, 2, 4.**
3 ✗ — Eulerian is easy (check degrees and connectivity). *Hamiltonian* is the hard one.

**Q38** — **2, 3, 4.**
1 ✗ — degree constraints alone are the assignment problem, which permits disjoint subtours.

**Q39** — **1, 2, 3.**
4 ✗ — nearest neighbour has no guarantee at all.

# §12–14 · Nonlinear

**Q40** — **1, 2, 4.**
3 ✗ — `det = 0` means the test is **inconclusive**, not that it's a saddle. Go to eigenvalues.

**Q41** — **1, 3, 4.**
2 ✗ — the pattern **alternates**, starting negative. Check against `−I`.

**Q42** — **1, 2, 4.**
3 ✗ — `f(x) = x⁴` is strictly convex but `f''(0) = 0`.

**Q43** — **2, 3.**
1 ✗ — the chord between two points on a circle passes through the interior, which isn't on the curve.
4 ✗ — two disjoint disks give an obvious counterexample.

**Q44** — **1, 2, 3.**
4 ✗ — `[0,1)` is neither; `ℝⁿ` is both.

**Q45** — **4 only.**
1 ✗ — too large a step diverges. 2 ✗ — "always" is false. 3 ✗ — without a CQ, KKT points need not be optima.

**Q46** — **1, 2, 3.**
4 ✗ — inactive means `g_i < 0`, so complementary slackness forces `λ_i = 0`.

**Q47** — **1, 2, 4.**
3 ✗ — that's Slater's guarantee. LICQ yields candidates only.
