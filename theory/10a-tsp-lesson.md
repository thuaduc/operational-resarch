# TSP and approximation

Companion to [10-tsp-and-approximation](10-tsp-and-approximation.md). Exam block E6/P6, about 30 minutes.

---

# Part 0: What the exam asks

TSP questions never ask for an optimal tour. Past questions:

```
SS23 E6 (12 pts)  "Fabienne believes she has found a modification of the
                   Nearest-Neighbor Heuristic that solves TSP optimally…"
                  → disprove it

SS24 P6  (18 pts) "To which NP-hard problem learned in the lecture does this
                   correspond?"
                  → classify (answer: vertex cover)

T9.2              "To which of the problem classes (Assignment, Knapsack, Bin
                   Packing, Set Covering, Traveling Salesperson) can this be
                   assigned? Give reasons."

2026 P1f          a 1.5-approximation returns 30 → bound OPT
```

What you need: the definitions, the IP model, the approximation ratios, and how to break a heuristic.

---

# Part 1: Euler vs Hamilton

| Term | Definition |
|---|---|
| Eulerian path | uses every EDGE exactly once |
| Eulerian cycle | an Eulerian path that returns to its start |
| Hamiltonian cycle | visits every NODE exactly once |

$$
\begin{aligned}
&\text{Euler} \to \text{edges} && (\text{polynomial}) \\
&\text{Hamilton} \to \text{nodes} && (\text{NP-complete})
\end{aligned}
$$

**TSP:** find a minimum-weight Hamiltonian cycle.

$$
\begin{aligned}
&\text{brute force:} && O(n!) \\
&\text{symmetric TSP:} && (n-1)!/2 \text{ distinct tours} && c_{ij} = c_{ji} \\
&\text{asymmetric TSP:} && (n-1)! \text{ distinct tours} && c_{ij} \ne c_{ji}
\end{aligned}
$$

**Metric TSP:** symmetric and the triangle inequality $c_{ik} \le c_{ij} + c_{jk}$ holds. Both approximation guarantees below require it.

---

# Part 2: TSP as an integer program

## Degree constraints

$$
\begin{aligned}
\min\ & \sum_i \sum_j c_{ij} x_{ij} \\
\text{s.t. } & \sum_{i \ne j} x_{ij} = 1 \quad \forall j && \text{each city entered once} \\
& \sum_{j \ne i} x_{ij} = 1 \quad \forall i && \text{each city left once} \\
& x_{ij} \in \{0,1\}
\end{aligned}
$$

## Why that is not enough

These are exactly the assignment constraints, and they allow **subtours**: with six cities, two disjoint triangles satisfy them. You need subtour elimination, and there are two ways to do it.

## SEC (Dantzig–Fulkerson–Johnson)

$$\sum_{i\in U} \sum_{j\in U} x_{ij} \le \lvert U\rvert - 1 \qquad \text{for every } U \subset N \text{ with } 2 \le \lvert U\rvert \le n-1$$

A group of $k$ cities may contain at most $k-1$ tour edges among themselves, so it cannot close into its own cycle.

$$
\begin{aligned}
&\text{2-city:} && x_{ij} + x_{ji} \le 1 \\
&\text{3-city:} && x_{ij} + x_{jk} + x_{ki} \le 2
\end{aligned}
$$

## MTZ (Miller–Tucker–Zemlin)

Give each city a position label $u_i$ and force the labels to increase along the tour:

$$
\begin{aligned}
&u_1 = 1 \\
&2 \le u_i \le n && \forall\, i \ne 1 \\
&u_j \ge u_i + 1 - (n-1)(1 - x_{ij}) && \forall\, i,j \ne 1,\ i \ne j
\end{aligned}
$$

Equivalent form used in the central exercise:

$$u_i - u_j + 1 \le (n-1)(1 - x_{ij})$$

If $x_{ij} = 1$, this reads $u_j \ge u_i + 1$. A subtour avoiding city 1 would need labels increasing all the way around a cycle, which is impossible.

Use the lecture's big-M of $(n-1)$. The textbook form $u_i - u_j + n\cdot x_{ij} \le n-1$ uses $M = n$.

## Trade-off

| Formulation | Constraints | LP relaxation |
|---|---|---|
| SEC | exponentially many | tight |
| MTZ | $O(n^2)$ | weak |

---

# Part 3: Heuristics and their guarantees

## Nearest neighbour

1. Start anywhere; mark visited.
2. Go to the nearest unvisited node.
3. Repeat until all visited, then close the tour.

Runs in $O(n^2)$ and has **no constant-factor guarantee**. SS23 E6 exploits this.

## MST-doubling (ratio 2)

1. Compute the MST $T$.
2. Double every edge of $T$, so every node has even degree.
3. Find an Euler tour on the doubled multigraph.
4. Walk it, skipping already-visited nodes (shortcutting).
5. Close back to the start.

Why 2: $c(\text{MST}) \le \text{OPT}$ (delete one edge of the optimal tour to get a spanning tree), the Euler tour costs $2\cdot c(\text{MST})$, and shortcutting does not increase cost under the triangle inequality.

## Christofides (ratio $3/2$)

1. Compute the MST $T$.
2. $O$ = the odd-degree vertices of $T$.
3. Compute a minimum-weight perfect matching $M$ on $O$.
4. $T \cup M$ has all degrees even; find an Euler tour.
5. Shortcut repeated vertices.


The only difference from MST-doubling is steps 2–3: match the odd-degree vertices instead of doubling every edge. Both need the triangle inequality and both end by shortcutting an Euler tour.

## The mislabel

Christofides is the $3/2$ algorithm with the odd-degree matching. MST-doubling is the 2-approximation without a matching.

Past papers and `ce-09-demo` D9.1 call MST-doubling "Christofides' 2-approximation". If a question is worded that way, execute MST-doubling, since that is what is graded. You can add one line noting that Christofides proper is the $3/2$ algorithm.

## Bounding OPT from an approximation

An $r$-approximation returning cost $C$ gives:

$$C / r \le \text{OPT} \le C$$

The lower bound comes from the guarantee, the upper bound from the returned tour being feasible. 2026 P1f: a 1.5-approximation returns 30, so $20 \le \text{OPT} \le 30$.

With an MST available, the best lower bound is $\max(c(\text{MST}), C/r)$. CE D9.1: $c(\text{MST}) = 13.24$, tours 17.6 and 22.19, so $13.24 \le \text{OPT} \le 17.6$.

---

# Part 4: Breaking a heuristic (SS23 E6)

> Fabienne claims a modified nearest-neighbour heuristic solves TSP optimally, given: a directed graph with all $c_{uv} \ge 1$, a start node $a$, and an edge $(v,a)$ of cost exactly 1 from every other node back to `a`.

Method:

1. Build a small instance that satisfies every stated assumption. Check each one.
2. Run the heuristic and record its tour and cost.
3. Give a better tour.
4. Conclude: the heuristic's tour costs more, so it is not optimal.

Greedy commits to a cheap first edge and pays later, so make the first hop cheap and the node it leads to expensive to leave. Four or five nodes are enough.

---

# Part 5: Classifying a problem

SS24 P6b and T9.2 both ask which known problem class a story belongs to.

| Signature in the wording | Class |
|---|---|
| pair $n$ things with $n$ things, one-to-one | **Assignment** |
| one budget, pick items to maximise value | **Knapsack** |
| minimise the number of containers used | **Bin packing** |
| cover every element at least once, minimise cost | **Set covering** |
| visit every node once and return | **TSP** |
| pick nodes so every edge is touched | **Vertex cover** (SS24 P6) |

SS24 P6's model was $\min \sum_i x_i \text{ s.t. } x_i + x_j \ge 1\ \forall (i,j) \in E$: every street needs an ATM at one of its ends, which is vertex cover. Follow-up: a complete graph on $n$ nodes needs $n - 1$.

## NP-hardness of TSP

Reduce Hamiltonian Cycle to metric TSP:
1. Given an unweighted graph $G = (V,E)$, build the complete graph on $V$.
2. Weight edges 1 if $(i,j) \in E$, else 2.
3. Symmetric and satisfies the triangle inequality, so it is metric TSP.
4. A tour of length $\lvert V\rvert$ exists iff $G$ has a Hamiltonian cycle.

---

# Traps

1. Swapping Euler and Hamilton. Euler = edges, Hamilton = nodes.
2. Stopping at degree constraints. They allow subtours; add SEC or MTZ.
3. The Christofides / MST-doubling mislabel. Execute what the wording describes.
4. Quoting a ratio without the triangle inequality.
5. Claiming nearest neighbour has a guarantee.
6. Using the textbook MTZ big-M $n$ instead of $n-1$.
7. Forgetting the free upper bound $\text{OPT} \le C$.

# Recall list

- Euler = every EDGE once (easy); Hamilton = every NODE once (NP-complete)
- degree constraints alone = assignment problem, allows subtours
- SEC: $\sum_{i,j\in U} x_{ij} \le \lvert U\rvert - 1$ (exponential, tight)
- MTZ: $u_i - u_j + 1 \le (n-1)(1-x_{ij})$ (polynomial, weak)
- $c(\text{MST}) \le \text{OPT}$
- MST-doubling $= 2$; Christofides $= 3/2$ (odd-degree matching)
- both need the triangle inequality
- nearest neighbour = no guarantee
- $r$-approx returns $C \implies C/r \le \text{OPT} \le C$

# Exercises

```
D9.1  TSP-Approximation        [EXAM]  run the approximation; start here
T9.1  Euler vs Hamilton        [DRILL] definitions
T9.2  Filling of ATMs          [EXAM]  "which problem class?" (SS24 P6b type)

papers: SS24 P6 (18), SS23 E6 (12), 2026 P1f
```

Sheet 9: `exercises/10-integer-programming-tsp/sheet-09-exercises.pdf`. CE-09: `central exercises/10-integer-programming-tsp/ce-09-demo.pdf`.
