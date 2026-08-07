# Multiple-choice bank

Exam format: *"Mark all correct answers (there can be more than one)."*
All correct = **2 pts** · one deviation = **1 pt** · two or more, or blank = **0**.

**Answers are in [mc-answers.md](mc-answers.md)** — separate file, so this one is a real test.
Items marked **[SS24]** / **[SS25]** are from real papers.

---

# §1 · LP basics and simplex

**Q1.** Which are correct about converting to standard form?
- [x] a `≤` constraint gets a slack variable, and that slack column can serve as a basis column
- [ ] a `≥` constraint gets a surplus variable whose column can serve as a basis column
- [x] a free variable is replaced by `x = x⁺ − x⁻` with `x⁺, x⁻ ≥ 0`
- [ ] `min cᵀx` and `max cᵀx` have the same optimal solution set

**Q2.** A point `v` of `X = {x : Ax ≤ b, x ≥ 0}` is a vertex if and only if…
- [x] it has `n` linearly independent active constraints
- [x] it corresponds to a basic feasible solution
- [x] it is not a convex combination of two other points of `X`
- [ ] it maximises some linear objective over `X`

**Q3.** Which can happen for a linear program?
- [x] the feasible region is empty
- [x] the feasible region is non-empty and the objective is unbounded
- [ ] there are exactly two optimal solutions
- [x] there are infinitely many optimal solutions

**Q4. [SS25]** Which scenarios can cause the simplex algorithm to cycle?
- [ ] the simplex algorithm never cycles
- [ ] the feasible region is unbounded
- [ ] there are multiple distinct optimal solutions
- [x] the problem is degenerate

**Q5.** In a max simplex tableau (z-row holding `−c`), which readings are correct?
- [x] no negative z-row entry ⟹ the current basis is optimal
- [x] the entering column has no strictly positive entry ⟹ the LP is unbounded
- [x] a tie in the ratio test ⟹ the next basis is degenerate
- [ ] a basic variable equal to zero ⟹ the LP is infeasible

---

# §2–3 · Tableau reading and sensitivity

**Q6.** From an optimal tableau you can read off directly…
- [x] the values of the basic variables
- [x] the shadow prices
- [x] the optimal objective value
- [ ] the number of simplex iterations that were performed

**Q7.** About shadow prices `y` in a max problem:
- [x] `yᵀ = c_Bᵀ B⁻¹`
- [x] they appear in Row 0 under the slack variables
- [x] a constraint with positive slack has shadow price zero
- [ ] `yᵢ` remains valid for arbitrarily large changes in `bᵢ`

**Q8. [SS25]** You perturb the RHS of a constraint by `Δ` and the basis stays optimal. Which are always true?
- [ ] the shadow price vector `π` should be updated
- [ ] `π` can be used to find the change in the values of the basic variables
- [x] `π` can be used to find the change in the objective value
- [x] the solution can become degenerate for some `Δ`

**Q9.** Cost ranging. Which are correct?
- [ ] changing a **non-basic** variable's cost can change the optimal solution but not the objective value, while it stays in range
- [x] changing a **basic** variable's cost changes the objective value but not the plan, while it stays in range
- [x] a basic variable's cost range is computed from that variable's **row** of the tableau
- [ ] a non-basic variable's cost range is two-sided

**Q10.** In an optimal tableau:
- [x] a basic variable with RHS zero indicates degeneracy
- [x] a non-basic variable with Row-0 entry zero indicates multiple optimal solutions
- [ ] degeneracy and multiple optima are the same phenomenon
- [x] the endpoints of a RHS validity range are exactly where the basis becomes degenerate

**Q11.** A new product needs resources `a_new` and sells at `c_new`. It is worth introducing iff…
- [x] `c_new > yᵀa_new`
- [x] its Row-0 entry `yᵀa_new − c_new` is negative
- [ ] `a_new` uses less of every resource than some existing product
- [ ] the LP must be re-solved from scratch to decide

---

# §4 · Duality

**Q12. [SS25]** Which are correct?
- [x] every linear program has a dual program
- [x] if a primal constraint is not tight, its corresponding dual variable is zero
- [ ] if the primal is infeasible, the dual is unbounded
- [x] if the primal is unbounded, the dual is infeasible

**Q13.** About the dual of `max cᵀx s.t. Ax ≤ b, x ≥ 0`:
- [x] the dual has one variable per primal constraint
- [x] the dual has one constraint per primal variable
- [ ] the sign restrictions `x ≥ 0` each contribute a dual variable
- [x] the dual of the dual is the primal

**Q14.** In a **max** primal, which SOB classifications are right?
- [x] a `≥` constraint is bizarre, so its dual variable satisfies `y ≤ 0`
- [x] an `=` constraint is odd, so its dual variable is free
- [x] a free primal variable gives a dual **equality** constraint
- [ ] a primal variable `x ≤ 0` gives a dual constraint in the sensible direction

**Q15.** Weak and strong duality:
- [x] `cᵀx ≤ bᵀy` holds for every feasible `x` and every feasible `y`
- [x] at optimality `cᵀx* = bᵀy*`
- [x] if both problems are feasible, both have finite optima
- [ ] a duality gap can exist for a feasible bounded LP

**Q16.** To show a given feasible `x*` is **not** optimal using complementary slackness, you…
- [ ] solve the primal LP and compare objective values
- [ ] set `yᵢ = 0` for every slack primal constraint
- [ ] force dual constraint `j` to equality for every `x_j ≠ 0`
- [ ] check the recovered `y` against the dual's remaining constraints and sign restrictions

---

# §5 · IP modelling and complexity

**Q17.** About the LP relaxation of a maximisation IP:
- [ ] its optimal value is an upper bound on the IP optimum
- [ ] if its optimum is integral, it solves the IP
- [ ] rounding its optimum always gives a feasible integer solution
- [ ] with integer objective coefficients you may round the bound down to `⌊z_LP⌋`

**Q18.** Two correct formulations of the same IP. The **stronger** one is the one whose…
- [ ] LP relaxation has a smaller feasible region
- [ ] integer feasible set is smaller
- [ ] relaxation gives a tighter bound, so branch & bound prunes earlier
- [ ] constraint count is smaller

**Q19.** Big-M constraints:
- [ ] `M` must be a constant, not a decision variable
- [ ] choosing `M` too small can cut off feasible solutions
- [ ] choosing `M` very large weakens the LP relaxation
- [ ] `t = 1 ⟺ x > K` can be modelled exactly for a continuous `x`

**Q20.** Complexity:
- [ ] deciding whether an integer program has a feasible solution is NP-hard
- [ ] linear programming is in P
- [ ] the simplex algorithm runs in polynomial time in the worst case
- [ ] every NP-hard problem is in NP

**Q21.** Which are on Karp's list of NP-complete problems as presented in the lecture?
- [ ] the travelling salesperson problem
- [ ] knapsack
- [ ] set cover
- [ ] linear programming

---

# §6 · Branch and bound and cuts

**Q22.** In branch & bound on a **maximisation** problem, a node may be pruned when…
- [ ] its relaxation is infeasible
- [ ] its relaxation optimum is integral
- [ ] its relaxation value is `≤` the incumbent
- [ ] its relaxation value is `≥` the incumbent

**Q23.** Branching on a fractional `x_i = 2.25` into `x_i ≤ 2` and `x_i ≥ 3`:
- [ ] discards no integer feasible point
- [ ] removes the current fractional optimum from both children
- [ ] each child inherits all constraints added by its ancestors
- [ ] the two children have disjoint LP feasible regions

**Q24. [SS25]** About cutting planes:
- [ ] they remove integer solutions from the LP relaxation's feasible region
- [ ] an ideal outcome is the convex hull of all integer feasible solutions
- [ ] the method always terminates with exactly two Gomory cuts
- [ ] combining cuts with branch & bound can speed up solving integer programs

**Q25.** Node-selection strategies:
- [ ] FIFO is breadth-first and takes the oldest open node
- [ ] LIFO is depth-first and takes the newest open node
- [ ] LIFO tends to find a first incumbent sooner
- [ ] FIFO always explores fewer nodes than LIFO

---

# §7 · Total unimodularity

**Q26.** `A` is totally unimodular. Which follow?
- [ ] every entry of `A` lies in `{−1, 0, +1}`
- [ ] `Aᵀ` is totally unimodular
- [ ] `[A, I]` is totally unimodular
- [ ] every square submatrix of `A` is invertible

**Q27. [SS25]** `A` is TU and `b ∈ ℤᵐ`. Consider `max{cᵀx : Ax ≤ b}`. Which claims are correct?
- [ ] if `c` is integral, every optimal vertex solution `x*` has integral value `cᵀx*`
- [ ] adding the constraint `x ≥ 0` can destroy total unimodularity of `A`
- [ ] there exists a bipartite graph `G` such that `A` is the incidence matrix of `G`
- [ ] the determinant of every submatrix of `A` is in `{+1, −1, 0}`

**Q28.** Which matrices are guaranteed TU?
- [ ] the incidence matrix of a bipartite graph
- [ ] the incidence matrix of a directed graph
- [ ] the incidence matrix of any undirected graph
- [ ] a `{0,1}` matrix with the consecutive-ones property

---

# §8 · Matroids

**Q29. [SS25]** Let `E` be a finite set. For which collections `I` is `(E, I)` a matroid?
- [ ] `I = {S ⊆ E : |S| is even}`
- [ ] `I = {S : S ⊆ E}`
- [ ] `I = {S ⊆ E : S contains no cycle}`, `E` the edge set of a graph
- [ ] `I = {S ⊆ E : |S| is odd}`

**Q30.** The exchange axiom says: for `A, B ∈ I` with `|A| < |B|`…
- [ ] there is an `x ∈ B \ A` with `A ∪ {x} ∈ I`
- [ ] there is an `x ∈ A \ B` with `B ∪ {x} ∈ I`
- [ ] the smaller set is the one that grows
- [ ] the new element comes from the larger set

**Q31.** In a matroid:
- [ ] all bases have the same cardinality
- [ ] a basis is a maximal independent set
- [ ] the greedy algorithm in decreasing weight order returns a maximum-weight basis
- [ ] the intersection of two matroids on the same ground set is a matroid

---

# §9 · Knapsack and approximation

**Q32.** The knapsack DP with table `B[i,w]`:
- [ ] runs in `O(n·W)` when indexed by weight
- [ ] runs in polynomial time in the input length
- [ ] can be indexed by value instead, in `O(n·V)`
- [ ] recovers the chosen items by backtracking through the table

**Q33.** Approximation classes:
- [ ] `P ⊆ FPTAS ⊆ PTAS ⊆ APX`
- [ ] knapsack admits an FPTAS
- [ ] an FPTAS runs in time polynomial in `n` and in `1/ε`
- [ ] every NP-hard optimisation problem admits a constant-factor approximation

---

# §10 · Network flow

**Q34.** For an `s–t` cut `S = [X, V\X]`:
- [ ] its capacity counts arcs from `X` to `V\X` only
- [ ] arcs from `V\X` back to `X` also contribute to the capacity
- [ ] every flow value is at most every cut capacity
- [ ] the minimum cut is unique

**Q35.** Ford–Fulkerson and the residual network:
- [ ] a forward residual arc has capacity `u(e) − f(e)`
- [ ] a backward residual arc has capacity `f(e)`
- [ ] backward arcs let the algorithm undo an earlier routing decision
- [ ] without backward arcs, greedy path-finding still finds the maximum flow

**Q36.** Which are true?
- [ ] max-flow equals min-cut
- [ ] with integral capacities there is an integral maximum flow
- [ ] with all capacities odd, some maximum flow has `f(e)` odd on every arc
- [ ] the min cut can be read off as the nodes reachable from `s` in the final residual network

---

# §11 · TSP

**Q37.** Definitions:
- [ ] an Eulerian cycle visits every edge exactly once
- [ ] a Hamiltonian cycle visits every node exactly once
- [ ] deciding whether a graph is Eulerian is NP-complete
- [ ] TSP asks for a minimum-weight Hamiltonian cycle

**Q38.** Subtour elimination:
- [ ] the degree constraints alone already forbid subtours
- [ ] SEC has exponentially many constraints
- [ ] MTZ has `O(n²)` constraints
- [ ] MTZ gives a weaker LP relaxation than SEC

**Q39.** Approximation algorithms for metric TSP:
- [ ] MST-doubling is a 2-approximation
- [ ] Christofides is a 3/2-approximation
- [ ] Christofides adds a minimum-weight perfect matching on the odd-degree vertices of the MST
- [ ] the nearest-neighbour heuristic has a constant-factor guarantee

---

# §12–14 · Nonlinear

**Q40.** At a critical point with Hessian `H` (2×2, `det = ad − b²`, `tr = a + d`):
- [ ] `det > 0` and `tr > 0` ⟹ local minimum
- [ ] `det < 0` ⟹ saddle point
- [ ] `det = 0` ⟹ the point is a saddle
- [ ] `det > 0` and `tr < 0` ⟹ local maximum

**Q41.** Leading principal minors of a Hessian:
- [ ] all `D_k > 0` ⟹ positive definite
- [ ] all `D_k < 0` ⟹ negative definite
- [ ] `D₁ < 0, D₂ > 0, D₃ < 0, …` ⟹ negative definite
- [ ] some `D_k = 0` ⟹ the criterion does not apply and you should use eigenvalues

**Q42.** Convexity:
- [ ] `H_f ⪰ 0` on a convex domain ⟺ `f` is convex
- [ ] `H_f ≻ 0` everywhere ⟹ `f` is strictly convex
- [ ] `f` strictly convex ⟹ `H_f ≻ 0` everywhere
- [ ] a convex function on a convex feasible set has every local minimum global

**Q43.** Convex sets:
- [ ] the circle `{x : ‖x‖ = r}` is convex
- [ ] the closed disk `{x : ‖x‖ ≤ r}` is convex
- [ ] the intersection of two convex sets is convex
- [ ] the union of two convex sets is convex

**Q44.** Compactness:
- [ ] in `ℝⁿ`, compact ⟺ closed and bounded
- [ ] `[0, ∞)` is closed but not bounded
- [ ] a compact non-empty set plus a continuous `f` guarantees a global min and max
- [ ] "not open" is the same as "closed"

**Q45. [SS25]** Which are true?
- [ ] gradient descent always converges to the global minimum for strictly convex functions, independent of step size
- [ ] gradient methods with momentum always need fewer iterations than without
- [ ] for a constrained problem, a KKT point is always a local optimum
- [ ] for a constrained **convex** problem a KKT point is always a global minimiser if Slater's condition holds

**Q46.** In the KKT system for `min f` s.t. `g_i ≤ 0`, `h_j = 0`:
- [ ] the inequality multipliers satisfy `λ_i ≥ 0`
- [ ] the equality multipliers `μ_j` are free in sign
- [ ] complementary slackness says `λ_i · g_i(x*) = 0`
- [ ] an inactive constraint may have a strictly positive multiplier

**Q47.** Constraint qualifications:
- [ ] Slater requires a point that satisfies every inequality **strictly**
- [ ] Slater requires convexity of `f` and the `g_i`
- [ ] under LICQ, a KKT point is guaranteed to be a global optimum
- [ ] under LICQ you must compare objective values across the candidates

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
