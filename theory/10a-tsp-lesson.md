# TSP and approximation — from scratch

Teaching companion to [10-tsp-and-approximation](10-tsp-and-approximation.md).

An **E6** block with a ~30-minute budget. Mechanics and definitions only.

---

# Part 0 — What the exam asks

TSP has never been a "compute the optimal tour" question. It's been:

```
SS23 E6 (12 pts)  "Fabienne believes she has found a modification of the
                   Nearest-Neighbor Heuristic that solves TSP optimally…"
                  → DISPROVE it

SS24 P6  (18 pts) "To which NP-hard problem learned in the lecture does this
                   correspond?"
                  → classify (answer: vertex cover)

T9.2              "To which of the problem classes (Assignment, Knapsack, Bin
                   Packing, Set Covering, Traveling Salesperson) can this be
                   assigned? Give reasons."
```

So: **definitions, the model, the approximation ratios, and the ability to break a heuristic.**

---

# Part 1 — Euler vs Hamilton

Two words that sound similar and mean opposite things. Get them straight — they're the cheapest
marks in the topic.

```
EULERIAN path    uses every EDGE exactly once
EULERIAN cycle   an Eulerian path that returns to its start

HAMILTONIAN cycle  visits every NODE exactly once
```

```
Euler  →  EDGES        (easy — solvable in polynomial time)
Hamilton → NODES       (hard — NP-complete)
```

**TSP = find a minimum-weight Hamiltonian cycle.**

```
brute force:        O(n!)
symmetric TSP:      (n−1)!/2 distinct tours       c_ij = c_ji
asymmetric TSP:     (n−1)!   distinct tours       c_ij ≠ c_ji
```

**Metric TSP** additionally satisfies the **triangle inequality** `c_ik ≤ c_ij + c_jk` — going
direct is never worse than detouring. Both approximation guarantees below need it.

---

# Part 2 — TSP as an integer program

## Step 1 — the degree constraints

```
min  Σᵢ Σⱼ c_ij x_ij

s.t. Σ_{i ≠ j} x_ij = 1     ∀j        each city ENTERED once
     Σ_{j ≠ i} x_ij = 1     ∀i        each city LEFT once
     x_ij ∈ {0,1}
```

## Step 2 — why that isn't enough

Those constraints are **exactly the assignment problem**, and they permit **subtours**: with six
cities you could get two disjoint triangles. Every city is entered once and left once, yet it
isn't a tour.

So you need subtour elimination. **Two ways, and the exam wants the trade-off.**

## SEC — Dantzig–Fulkerson–Johnson

```
Σ_{i∈U} Σ_{j∈U} x_ij  ≤  |U| − 1      for every U ⊂ N with 2 ≤ |U| ≤ n−1
```
*"Any group of `k` cities can contain at most `k−1` tour edges among themselves"* — so it can't
close into its own cycle.

```
2-city:  x_ij + x_ji ≤ 1
3-city:  x_ij + x_jk + x_ki ≤ 2
```

**Exponentially many constraints (~2ⁿ), but a tight relaxation.**

## MTZ — Miller–Tucker–Zemlin

Give city `i` a position label `u_i` and force the labels to increase along the tour:

```
u₁ = 1
2 ≤ u_i ≤ n                              ∀ i ≠ 1
u_j ≥ u_i + 1 − (n−1)(1 − x_ij)          ∀ i,j ≠ 1, i ≠ j
```

Equivalently, the form the central exercise writes:
```
u_i − u_j + 1 ≤ (n−1)(1 − x_ij)
```

**Only `O(n²)` constraints, but a weak relaxation.**

> **Use the lecture's `(n−1)` version.** The textbook form `u_i − u_j + n·x_ij ≤ n−1` is the same
> family with big-M `= n`; the course uses `n−1`.

## The trade-off, in one line

```
SEC:  exponential many constraints, TIGHT relaxation
MTZ:  polynomial many constraints, WEAK relaxation
```

That sentence is the answer whenever the comparison is asked.

---

# Part 3 — Heuristics and their guarantees

## Nearest neighbour

```
1. Start anywhere; mark visited.
2. Go to the nearest unvisited node.
3. Repeat until all visited, then close the tour.
```
`O(n²)`, and **no constant-factor guarantee at all.** It can be arbitrarily bad. That's what
SS23 E6 exploits.

## MST-doubling — ratio 2

```
1. Compute the MST T.
2. DOUBLE every edge of T  →  every node now has even degree.
3. Find an Euler tour on the doubled multigraph.
4. Walk it, SKIPPING already-visited nodes (shortcutting).
5. Close back to the start.
```

## Christofides — ratio 3/2

```
1. Compute the MST T.
2. Let O = the ODD-degree vertices of T.
3. Compute a MINIMUM-WEIGHT PERFECT MATCHING M on O.
4. T ∪ M is a multigraph with all degrees even → find an Euler tour.
5. Shortcut repeated vertices.
```

The only difference is step 2–3: **doubling everything (ratio 2) versus matching just the
odd-degree vertices (ratio 3/2).** Both require the triangle inequality, and both finish by
shortcutting an Euler tour.

## The correction you must carry in

> **Christofides = 3/2**, with the odd-degree matching.
> **MST-doubling = 2**, no matching.
>
> **Past papers and `ce-09-demo` D9.1 mislabel MST-doubling as "Christofides' 2-approximation".**
> If a question is phrased their way, **execute MST-doubling** — that is what gets graded. Adding
> a one-line note that Christofides proper is the 3/2 algorithm costs nothing and shows you know.

---

# Part 4 — Breaking a heuristic (SS23 E6)

> Fabienne claims a modified Nearest-Neighbour heuristic solves TSP **optimally**, given: a
> directed graph with all `c_uv ≥ 1`, a start node `a`, and an edge `(v,a)` of cost exactly 1
> from every other node back to `a`.

The claim is false, and the method is the same as the max-flow counterexamples:

```
1. Build a SMALL instance satisfying every stated assumption. Check them explicitly.
2. Run the heuristic and record the tour it produces, with its cost.
3. Exhibit a BETTER tour.
4. Write the sentence: the heuristic's tour costs more, so it is not optimal.
```

The lever is always the same: **greedy commits to a cheap early edge and pays for it later.** Make
the first hop attractive and the consequence expensive.

Keep it to four or five nodes. And **check the assumptions hold in your instance** — a
counterexample that violates the premises proves nothing.

---

# Part 5 — Classifying a problem

SS24 P6b and T9.2 both ask *"which known problem class is this?"* — free marks if you can pattern-match.

| Signature in the wording | Class |
|---|---|
| pair up `n` things with `n` things, one-to-one | **Assignment** |
| one budget, pick items to maximise value | **Knapsack** |
| minimise the *number of containers* used | **Bin packing** |
| cover every element at least once, minimise cost | **Set covering** |
| visit every node once and return | **TSP** |
| pick nodes so every *edge* is touched | **Vertex cover** ← SS24 P6 |

SS24 P6's model was `min Σᵢ xᵢ s.t. xᵢ + xⱼ ≥ 1 ∀(i,j) ∈ E` — every street needs an ATM at one of
its two ends. That's vertex cover. Follow-up: on a complete graph with `n` nodes you need
`n − 1`.

## NP-hardness of TSP, if asked

Reduce Hamiltonian Cycle to metric TSP:
```
1. Given an unweighted graph G = (V,E), build the complete graph on V.
2. Weight edges:  1 if (i,j) ∈ E,  else 2.
3. Symmetric and satisfies the triangle inequality → a metric TSP instance.
4. Ask "is there a tour of length |V|?" — yes iff G has a Hamiltonian cycle.
```

---

# Traps and drills

## Where points are lost

1. **Swapping Euler and Hamilton.** Euler = edges, Hamilton = nodes.
2. **Degree constraints alone.** They permit subtours — say so, then add SEC or MTZ.
3. **The Christofides / MST-doubling mislabel.** Execute what the wording describes.
4. **Forgetting the triangle inequality** when quoting either ratio.
5. **Claiming nearest neighbour has a guarantee.** It has none.
6. **Using the textbook MTZ big-M `n`** instead of the lecture's `n−1`.

## Say these without looking

```
Euler = every EDGE once (easy)     Hamilton = every NODE once (NP-complete)
degree constraints alone = the assignment problem, allows subtours
SEC: Σ_{i,j∈U} x_ij ≤ |U|−1        exponential, tight
MTZ: u_i − u_j + 1 ≤ (n−1)(1−x_ij)  polynomial, weak
MST-doubling = 2      Christofides = 3/2 (odd-degree matching)
both need the triangle inequality
nearest neighbour = no guarantee
```

## Warm-up and papers

```
D9.1  TSP-Approximation        [EXAM]  execute the approximation — start here
T9.1  Euler vs Hamilton        [DRILL] the definitions
T9.2  Filling of ATMs          [EXAM]  "which problem class?" = SS24 P6b's type

papers: SS24 P6 (18), SS23 E6 (12)
```

Sheet 9 is `exercises/10-integer-programming-tsp/sheet-09-exercises.pdf`; CE-09 is
`central exercises/10-integer-programming-tsp/ce-09-demo.pdf`.
