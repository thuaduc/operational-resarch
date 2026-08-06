# Multiple-choice bank

Exam format: *"Mark all correct answers (there can be more than one)."*
All correct = **2 pts** · one deviation = **1 pt** · two or more, or blank = **0**.

Answers sit in a blockquote under each question — cover them, or read straight through.
Items marked **[SS24]** / **[SS25]** are from real papers.

---

# §1 · LP basics and simplex

**Q1.** Which are correct about converting to standard form?
- [ ] a `≤` constraint gets a slack variable, and that slack column can serve as a basis column
- [ ] a `≥` constraint gets a surplus variable whose column can serve as a basis column
- [ ] a free variable is replaced by `x = x⁺ − x⁻` with `x⁺, x⁻ ≥ 0`
- [ ] `min cᵀx` and `max cᵀx` have the same optimal solution set

> **1, 3.**
> 2 ✗ — the surplus column is `−1`, not `+1`, so it is *not* a basis column; that's why artificial variables exist.
> 4 ✗ — `min cᵀx = −max(−cᵀx)`; you negate the objective, not reuse it.

**Q2.** A point `v` of `X = {x : Ax ≤ b, x ≥ 0}` is a vertex if and only if…
- [ ] it has `n` linearly independent active constraints
- [ ] it corresponds to a basic feasible solution
- [ ] it is not a convex combination of two other points of `X`
- [ ] it maximises some linear objective over `X`

> **1, 2, 3.**
> 4 ✗ — true for *some* objective only if the LP is bounded in that direction, and non-vertex points can also be maximisers when there are multiple optima.

**Q3.** Which can happen for a linear program?
- [ ] the feasible region is empty
- [ ] the feasible region is non-empty and the objective is unbounded
- [ ] there are exactly two optimal solutions
- [ ] there are infinitely many optimal solutions

> **1, 2, 4.**
> 3 ✗ — if two distinct points are optimal, every convex combination of them is too, so you get infinitely many. Never exactly two.

**Q4. [SS25]** Which scenarios can cause the simplex algorithm to cycle?
- [ ] the simplex algorithm never cycles
- [ ] the feasible region is unbounded
- [ ] there are multiple distinct optimal solutions
- [ ] the problem is degenerate

> **4 only.** Cycling requires a degenerate pivot — a basis change with no objective improvement.

**Q5.** In a max simplex tableau (z-row holding `−c`), which readings are correct?
- [ ] no negative z-row entry ⟹ the current basis is optimal
- [ ] the entering column has no strictly positive entry ⟹ the LP is unbounded
- [ ] a tie in the ratio test ⟹ the next basis is degenerate
- [ ] a basic variable equal to zero ⟹ the LP is infeasible

> **1, 2, 3.**
> 4 ✗ — that's degeneracy, and a degenerate BFS is perfectly feasible.

---

# §2–3 · Tableau reading and sensitivity

**Q6.** From an optimal tableau you can read off directly…
- [ ] the values of the basic variables
- [ ] the shadow prices
- [ ] the optimal objective value
- [ ] the number of simplex iterations that were performed

> **1, 2, 3.** The tableau records the final state only, not the path taken.

**Q7.** About shadow prices `y` in a max problem:
- [ ] `yᵀ = c_Bᵀ B⁻¹`
- [ ] they appear in Row 0 under the slack variables
- [ ] a constraint with positive slack has shadow price zero
- [ ] `yᵢ` remains valid for arbitrarily large changes in `bᵢ`

> **1, 2, 3.**
> 4 ✗ — valid only while the basis stays optimal, i.e. inside the RHS range.

**Q8. [SS25]** You perturb the RHS of a constraint by `Δ` and the basis stays optimal. Which are always true?
- [ ] the shadow price vector `π` should be updated
- [ ] `π` can be used to find the change in the values of the basic variables
- [ ] `π` can be used to find the change in the objective value
- [ ] the solution can become degenerate for some `Δ`

> **3, 4.**
> 1 ✗ — if the basis is unchanged, so is `π`.
> 2 ✗ — the basic variables move by `B⁻¹Δb`, not by `π`. `π` gives only the *objective* change.

**Q9.** Cost ranging. Which are correct?
- [ ] changing a **non-basic** variable's cost can change the optimal solution but not the objective value, while it stays in range
- [ ] changing a **basic** variable's cost changes the objective value but not the plan, while it stays in range
- [ ] a basic variable's cost range is computed from that variable's **row** of the tableau
- [ ] a non-basic variable's cost range is two-sided

> **2, 3.**
> 1 ✗ — inside the range *neither* the plan nor `z` changes; you're making none of it.
> 4 ✗ — one-sided: `c_j ≤ yᵀa_j`.

**Q10.** In an optimal tableau:
- [ ] a basic variable with RHS zero indicates degeneracy
- [ ] a non-basic variable with Row-0 entry zero indicates multiple optimal solutions
- [ ] degeneracy and multiple optima are the same phenomenon
- [ ] the endpoints of a RHS validity range are exactly where the basis becomes degenerate

> **1, 2, 4.**
> 3 ✗ — degeneracy is one point described by several bases; multiple optima is several points with the same `z`.

**Q11.** A new product needs resources `a_new` and sells at `c_new`. It is worth introducing iff…
- [ ] `c_new > yᵀa_new`
- [ ] its Row-0 entry `yᵀa_new − c_new` is negative
- [ ] `a_new` uses less of every resource than some existing product
- [ ] the LP must be re-solved from scratch to decide

> **1, 2** — the same statement twice.
> 3 ✗ — irrelevant. 4 ✗ — that's the whole point of pricing a column.

---

# §4 · Duality

**Q12. [SS25]** Which are correct?
- [ ] every linear program has a dual program
- [ ] if a primal constraint is not tight, its corresponding dual variable is zero
- [ ] if the primal is infeasible, the dual is unbounded
- [ ] if the primal is unbounded, the dual is infeasible

> **1, 2, 4.**
> 3 ✗ — infeasible primal ⟹ dual is unbounded **or** infeasible. Both-infeasible is a real case.

**Q13.** About the dual of `max cᵀx s.t. Ax ≤ b, x ≥ 0`:
- [ ] the dual has one variable per primal constraint
- [ ] the dual has one constraint per primal variable
- [ ] the sign restrictions `x ≥ 0` each contribute a dual variable
- [ ] the dual of the dual is the primal

> **1, 2, 4.**
> 3 ✗ — sign restrictions are not counted as constraints.

**Q14.** In a **max** primal, which SOB classifications are right?
- [ ] a `≥` constraint is bizarre, so its dual variable satisfies `y ≤ 0`
- [ ] an `=` constraint is odd, so its dual variable is free
- [ ] a free primal variable gives a dual **equality** constraint
- [ ] a primal variable `x ≤ 0` gives a dual constraint in the sensible direction

> **1, 2, 3.**
> 4 ✗ — `x ≤ 0` is bizarre, so its dual constraint takes the *bizarre* direction.

**Q15.** Weak and strong duality:
- [ ] `cᵀx ≤ bᵀy` holds for every feasible `x` and every feasible `y`
- [ ] at optimality `cᵀx* = bᵀy*`
- [ ] if both problems are feasible, both have finite optima
- [ ] a duality gap can exist for a feasible bounded LP

> **1, 2, 3.**
> 4 ✗ — LPs have no duality gap; that's strong duality. Gaps are an *integer* programming phenomenon.

**Q16.** To show a given feasible `x*` is **not** optimal using complementary slackness, you…
- [ ] solve the primal LP and compare objective values
- [ ] set `yᵢ = 0` for every slack primal constraint
- [ ] force dual constraint `j` to equality for every `x_j ≠ 0`
- [ ] check the recovered `y` against the dual's remaining constraints and sign restrictions

> **2, 3, 4.**
> 1 ✗ — the entire point is that you never solve the LP.

---

# §5 · IP modelling and complexity

**Q17.** About the LP relaxation of a maximisation IP:
- [ ] its optimal value is an upper bound on the IP optimum
- [ ] if its optimum is integral, it solves the IP
- [ ] rounding its optimum always gives a feasible integer solution
- [ ] with integer objective coefficients you may round the bound down to `⌊z_LP⌋`

> **1, 2, 4.**
> 3 ✗ — rounding frequently lands outside the feasible region.

**Q18.** Two correct formulations of the same IP. The **stronger** one is the one whose…
- [ ] LP relaxation has a smaller feasible region
- [ ] integer feasible set is smaller
- [ ] relaxation gives a tighter bound, so branch & bound prunes earlier
- [ ] constraint count is smaller

> **1, 3.**
> 2 ✗ — the integer sets are identical; that's what "same IP" means.
> 4 ✗ — the disaggregated linking form has *more* constraints and is stronger.

**Q19.** Big-M constraints:
- [ ] `M` must be a constant, not a decision variable
- [ ] choosing `M` too small can cut off feasible solutions
- [ ] choosing `M` very large weakens the LP relaxation
- [ ] `t = 1 ⟺ x > K` can be modelled exactly for a continuous `x`

> **1, 2, 3.**
> 4 ✗ — `{x > K}` is not closed, so it isn't a polytope. Only for integer `x`.

**Q20.** Complexity:
- [ ] deciding whether an integer program has a feasible solution is NP-hard
- [ ] linear programming is in P
- [ ] the simplex algorithm runs in polynomial time in the worst case
- [ ] every NP-hard problem is in NP

> **1, 2.**
> 3 ✗ — Klee–Minty cubes force `2ⁿ` vertices. LP is in P via ellipsoid/interior-point, not simplex.
> 4 ✗ — NP-hard does not require membership in NP; NP-**complete** does.

**Q21.** Which are on Karp's list of NP-complete problems as presented in the lecture?
- [ ] the travelling salesperson problem
- [ ] knapsack
- [ ] set cover
- [ ] linear programming

> **1, 2, 3.**
> 4 ✗ — LP is in P.

---

# §6 · Branch and bound and cuts

**Q22.** In branch & bound on a **maximisation** problem, a node may be pruned when…
- [ ] its relaxation is infeasible
- [ ] its relaxation optimum is integral
- [ ] its relaxation value is `≤` the incumbent
- [ ] its relaxation value is `≥` the incumbent

> **1, 2, 3.**
> 4 ✗ — that's the *minimisation* rule. A node above the incumbent is exactly the one worth exploring.

**Q23.** Branching on a fractional `x_i = 2.25` into `x_i ≤ 2` and `x_i ≥ 3`:
- [ ] discards no integer feasible point
- [ ] removes the current fractional optimum from both children
- [ ] each child inherits all constraints added by its ancestors
- [ ] the two children have disjoint LP feasible regions

> **1, 2, 3, 4.** All four hold — no integer lies strictly between 2 and 3, and the two half-spaces don't overlap.

**Q24. [SS25]** About cutting planes:
- [ ] they remove integer solutions from the LP relaxation's feasible region
- [ ] an ideal outcome is the convex hull of all integer feasible solutions
- [ ] the method always terminates with exactly two Gomory cuts
- [ ] combining cuts with branch & bound can speed up solving integer programs

> **2, 4.**
> 1 ✗ — by definition a valid inequality never removes an integer point.
> 3 ✗ — no such guarantee.

**Q25.** Node-selection strategies:
- [ ] FIFO is breadth-first and takes the oldest open node
- [ ] LIFO is depth-first and takes the newest open node
- [ ] LIFO tends to find a first incumbent sooner
- [ ] FIFO always explores fewer nodes than LIFO

> **1, 2, 3.**
> 4 ✗ — depends entirely on the instance. On SS25's tree FIFO happened to be shorter; that's not a rule.

---

# §7 · Total unimodularity

**Q26.** `A` is totally unimodular. Which follow?
- [ ] every entry of `A` lies in `{−1, 0, +1}`
- [ ] `Aᵀ` is totally unimodular
- [ ] `[A, I]` is totally unimodular
- [ ] every square submatrix of `A` is invertible

> **1, 2, 3.**
> 4 ✗ — determinant `0` is allowed, and that means singular.

**Q27. [SS25]** `A` is TU and `b ∈ ℤᵐ`. Consider `max{cᵀx : Ax ≤ b}`. Which claims are correct?
- [ ] if `c` is integral, every optimal vertex solution `x*` has integral value `cᵀx*`
- [ ] adding the constraint `x ≥ 0` can destroy total unimodularity of `A`
- [ ] there exists a bipartite graph `G` such that `A` is the incidence matrix of `G`
- [ ] the determinant of every submatrix of `A` is in `{+1, −1, 0}`

> **1, 4.**
> 2 ✗ — appending an identity block preserves TU.
> 3 ✗ — bipartite incidence matrices are TU, but not every TU matrix is one.

**Q28.** Which matrices are guaranteed TU?
- [ ] the incidence matrix of a bipartite graph
- [ ] the incidence matrix of a directed graph
- [ ] the incidence matrix of any undirected graph
- [ ] a `{0,1}` matrix with the consecutive-ones property

> **1, 2, 4.**
> 3 ✗ — an odd cycle breaks it; the graph must be bipartite.

---

# §8 · Matroids

**Q29. [SS25]** Let `E` be a finite set. For which collections `ℐ` is `(E, ℐ)` a matroid?
- [ ] `ℐ = {S ⊆ E : |S| is even}`
- [ ] `ℐ = {S : S ⊆ E}`
- [ ] `ℐ = {S ⊆ E : S contains no cycle}`, `E` the edge set of a graph
- [ ] `ℐ = {S ⊆ E : |S| is odd}`

> **2, 3.**
> 1 ✗ — not hereditary: `{a,b} ∈ ℐ` but `{a} ∉ ℐ`.
> 4 ✗ — `∅ ∉ ℐ`, since `|∅| = 0` is even. Fails axiom 1.

**Q30.** The exchange axiom says: for `A, B ∈ ℐ` with `|A| < |B|`…
- [ ] there is an `x ∈ B \ A` with `A ∪ {x} ∈ ℐ`
- [ ] there is an `x ∈ A \ B` with `B ∪ {x} ∈ ℐ`
- [ ] the smaller set is the one that grows
- [ ] the new element comes from the larger set

> **1, 3, 4.**
> 2 ✗ — that's the direction reversed, the standard error.

**Q31.** In a matroid:
- [ ] all bases have the same cardinality
- [ ] a basis is a maximal independent set
- [ ] the greedy algorithm in decreasing weight order returns a maximum-weight basis
- [ ] the intersection of two matroids on the same ground set is a matroid

> **1, 2, 3.**
> 4 ✗ — it's an independence system, but generally not a matroid. That's exercise `T8.1`.

---

# §9 · Knapsack and approximation

**Q32.** The knapsack DP with table `B[i,w]`:
- [ ] runs in `O(n·W)` when indexed by weight
- [ ] runs in polynomial time in the input length
- [ ] can be indexed by value instead, in `O(n·V)`
- [ ] recovers the chosen items by backtracking through the table

> **1, 3, 4.**
> 2 ✗ — **pseudo**polynomial. `W` is a number; writing it takes `log W` bits.

**Q33.** Approximation classes:
- [ ] `P ⊆ FPTAS ⊆ PTAS ⊆ APX`
- [ ] knapsack admits an FPTAS
- [ ] an FPTAS runs in time polynomial in `n` and in `1/ε`
- [ ] every NP-hard optimisation problem admits a constant-factor approximation

> **1, 2, 3.**
> 4 ✗ — max clique, set cover and max independent set are not in APX at all.

---

# §10 · Network flow

**Q34.** For an `s–t` cut `S = [X, V\X]`:
- [ ] its capacity counts arcs from `X` to `V\X` only
- [ ] arcs from `V\X` back to `X` also contribute to the capacity
- [ ] every flow value is at most every cut capacity
- [ ] the minimum cut is unique

> **1, 3.**
> 2 ✗ — backward arcs contribute nothing; they cannot carry anything toward `t`.
> 4 ✗ — the *value* is unique, the cut achieving it need not be.

**Q35.** Ford–Fulkerson and the residual network:
- [ ] a forward residual arc has capacity `u(e) − f(e)`
- [ ] a backward residual arc has capacity `f(e)`
- [ ] backward arcs let the algorithm undo an earlier routing decision
- [ ] without backward arcs, greedy path-finding still finds the maximum flow

> **1, 2, 3.**
> 4 ✗ — greedy alone can get stuck strictly below the maximum.

**Q36.** Which are true?
- [ ] max-flow equals min-cut
- [ ] with integral capacities there is an integral maximum flow
- [ ] with all capacities odd, some maximum flow has `f(e)` odd on every arc
- [ ] the min cut can be read off as the nodes reachable from `s` in the final residual network

> **1, 2, 4.**
> 3 ✗ — this is SS25 E6a's counterexample: two odd flows merging give an even one.

---

# §11 · TSP

**Q37.** Definitions:
- [ ] an Eulerian cycle visits every edge exactly once
- [ ] a Hamiltonian cycle visits every node exactly once
- [ ] deciding whether a graph is Eulerian is NP-complete
- [ ] TSP asks for a minimum-weight Hamiltonian cycle

> **1, 2, 4.**
> 3 ✗ — Eulerian is easy (check degrees and connectivity). *Hamiltonian* is the hard one.

**Q38.** Subtour elimination:
- [ ] the degree constraints alone already forbid subtours
- [ ] SEC has exponentially many constraints
- [ ] MTZ has `O(n²)` constraints
- [ ] MTZ gives a weaker LP relaxation than SEC

> **2, 3, 4.**
> 1 ✗ — degree constraints alone are the assignment problem, which permits disjoint subtours.

**Q39.** Approximation algorithms for metric TSP:
- [ ] MST-doubling is a 2-approximation
- [ ] Christofides is a 3/2-approximation
- [ ] Christofides adds a minimum-weight perfect matching on the odd-degree vertices of the MST
- [ ] the nearest-neighbour heuristic has a constant-factor guarantee

> **1, 2, 3.**
> 4 ✗ — nearest neighbour has no guarantee at all.

---

# §12–14 · Nonlinear

**Q40.** At a critical point with Hessian `H` (2×2, `det = ad − b²`, `tr = a + d`):
- [ ] `det > 0` and `tr > 0` ⟹ local minimum
- [ ] `det < 0` ⟹ saddle point
- [ ] `det = 0` ⟹ the point is a saddle
- [ ] `det > 0` and `tr < 0` ⟹ local maximum

> **1, 2, 4.**
> 3 ✗ — `det = 0` means the test is **inconclusive**, not that it's a saddle. Go to eigenvalues.

**Q41.** Leading principal minors of a Hessian:
- [ ] all `D_k > 0` ⟹ positive definite
- [ ] all `D_k < 0` ⟹ negative definite
- [ ] `D₁ < 0, D₂ > 0, D₃ < 0, …` ⟹ negative definite
- [ ] some `D_k = 0` ⟹ the criterion does not apply and you should use eigenvalues

> **1, 3, 4.**
> 2 ✗ — the pattern **alternates**, starting negative. Check against `−I`.

**Q42.** Convexity:
- [ ] `H_f ⪰ 0` on a convex domain ⟺ `f` is convex
- [ ] `H_f ≻ 0` everywhere ⟹ `f` is strictly convex
- [ ] `f` strictly convex ⟹ `H_f ≻ 0` everywhere
- [ ] a convex function on a convex feasible set has every local minimum global

> **1, 2, 4.**
> 3 ✗ — `f(x) = x⁴` is strictly convex but `f''(0) = 0`.

**Q43.** Convex sets:
- [ ] the circle `{x : ‖x‖ = r}` is convex
- [ ] the closed disk `{x : ‖x‖ ≤ r}` is convex
- [ ] the intersection of two convex sets is convex
- [ ] the union of two convex sets is convex

> **2, 3.**
> 1 ✗ — the chord between two points on a circle passes through the interior, which isn't on the curve.
> 4 ✗ — two disjoint disks give an obvious counterexample.

**Q44.** Compactness:
- [ ] in `ℝⁿ`, compact ⟺ closed and bounded
- [ ] `[0, ∞)` is closed but not bounded
- [ ] a compact non-empty set plus a continuous `f` guarantees a global min and max
- [ ] "not open" is the same as "closed"

> **1, 2, 3.**
> 4 ✗ — `[0,1)` is neither; `ℝⁿ` is both.

**Q45. [SS25]** Which are true?
- [ ] gradient descent always converges to the global minimum for strictly convex functions, independent of step size
- [ ] gradient methods with momentum always need fewer iterations than without
- [ ] for a constrained problem, a KKT point is always a local optimum
- [ ] for a constrained **convex** problem a KKT point is always a global minimiser if Slater's condition holds

> **4 only.**
> 1 ✗ — too large a step diverges. 2 ✗ — "always" is false. 3 ✗ — without a CQ, KKT points need not be optima.

**Q46.** In the KKT system for `min f` s.t. `g_i ≤ 0`, `h_j = 0`:
- [ ] the inequality multipliers satisfy `λ_i ≥ 0`
- [ ] the equality multipliers `μ_j` are free in sign
- [ ] complementary slackness says `λ_i · g_i(x*) = 0`
- [ ] an inactive constraint may have a strictly positive multiplier

> **1, 2, 3.**
> 4 ✗ — inactive means `g_i < 0`, so complementary slackness forces `λ_i = 0`.

**Q47.** Constraint qualifications:
- [ ] Slater requires a point that satisfies every inequality **strictly**
- [ ] Slater requires convexity of `f` and the `g_i`
- [ ] under LICQ, a KKT point is guaranteed to be a global optimum
- [ ] under LICQ you must compare objective values across the candidates

> **1, 2, 4.**
> 3 ✗ — that's Slater's guarantee. LICQ yields candidates only.

---

# Self-check

Twenty minutes, no notes, before the exam. Score yourself with the real rule:
**all correct 2 · one deviation 1 · otherwise 0.**

| If you drop points on | Reread |
|---|---|
| Q1–Q5 | card §1 |
| Q6–Q11 | card §2, §3 |
| Q12–Q16 | card §4 |
| Q17–Q21 | card §5 |
| Q22–Q25 | card §6 |
| Q26–Q28 | card §7 |
| Q29–Q31 | card §8 |
| Q32–Q33 | card §9 |
| Q34–Q36 | card §10 |
| Q37–Q39 | card §11 |
| Q40–Q47 | card §12, §13, §14 |
