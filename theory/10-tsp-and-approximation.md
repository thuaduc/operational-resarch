## Definitions

**Degree** `deg(v)` — number of edges incident to node `v`.

**Eulerian path** — a path that contains **each edge** of the graph exactly once.
**Eulerian cycle (Euler tour)** — an Eulerian path with start point = end point.
**Eulerian graph** — a graph that contains an Eulerian cycle.

**Hamiltonian cycle** — a cycle that contains **each node** of the graph exactly once.
**Hamiltonian graph** — a graph containing a Hamiltonian cycle.

**TSP** — finding a *minimum-weight* Hamiltonian cycle in a weighted graph. Complete search is `O(n!)`.

- **Symmetric TSP**: `c_ij = c_ji`. Number of distinct tours `= (n−1)!/2`.

- **Asymmetric TSP**: outward and return leg may differ, `c_ij ≠ c_ji`. Number of tours `= (n−1)!`.

- **Metric TSP**: symmetric **and** the triangle inequality holds. Sheet 09 T9.1 states it as two conditions: *edge symmetry* + *triangle inequality*.

**Matching** — a set of edges with no node in common.
**Perfect matching** — every vertex is incident to exactly one selected edge.
Minimum-weight perfect matching on a general (non-bipartite) graph is solvable in polynomial time; the bipartite max-weight version is the assignment problem (Hungarian algorithm).

**Approximation ratio `ρ`** — a **worst-case guarantee**: `ALG(I) ≤ ρ · OPT(I)` for *every* instance `I`. It says nothing about any single instance.

**Spanning tree / MST** — subgraph that is a tree and touches all `n` nodes (`n−1` edges); MST = minimum total weight such tree.

---

## Procedures

### 1. Kruskal's algorithm (MST)

1. Sort all edges by weight ascending.
2. Start with empty forest `I = ∅`.
3. Take the next cheapest unprocessed edge `e`.
4. If `I ∪ {e}` has no cycle, add `e`; otherwise discard it.
5. Stop when `I` has `n − 1` edges.

### 2. Nearest-Neighbour heuristic

1. Start at any node `v₀`; mark it visited.
2. Move to the nearest unvisited node, add that edge to the tour, repeat until all nodes visited.
3. Add the closing edge back to `v₀`.

Runtime `O(n²)`. No constant-factor approximation guarantee.

### 3. Nearest-Insertion heuristic

1. Start from a small existing subtour.
2. Among all nodes not yet in the tour, pick the one closest to any node already in the tour.
3. Insert it at the position `(i, j)` minimising `c_ik + c_kj − c_ij`.
4. Repeat until all nodes are inserted.

### 4. Double-tree / MST-doubling algorithm — ratio 2

1. Compute the MST `T` of the graph.
2. Double every edge of `T` to get a multigraph where every node has even degree.
3. Find an Euler tour on this multigraph.
4. Walk the Euler tour; skip any already-visited node and go directly to the next unvisited one.
5. Close the tour back to the start.

Only valid under the triangle inequality (metric instances).

### 5. Christofides' algorithm — ratio 3/2 (metric only)

1. Compute the MST `T`.
2. Let `O` = set of odd-degree vertices in `T`.
3. Compute a minimum-weight perfect matching `M` on `O` using original edge costs.
4. Combine `T ∪ M` into a multigraph (all degrees now even) and find an Eulerian cycle.
5. Shortcut repeated vertices to get a Hamiltonian cycle.

### 6. NP-hardness of TSP: reduction HC ≤_p metric TSP

1. Given an unweighted graph `G = (V, E)` (Hamiltonian-Cycle instance).
2. Build the complete graph on `V` with weights: `u(i,j) = 1` if `(i,j) ∈ E`, else `u(i,j) = 2`.
3. This satisfies symmetry and triangle inequality, so it is a metric TSP instance.
4. Ask: "Does a tour of length `|V|` exist?"
5. Yes implies all edges have weight 1, so they all lie in `E` — that is a Hamiltonian cycle in `G`.
6. No implies every tour uses a weight-2 edge not in `E` — so `G` has no Hamiltonian cycle.
7. The reduction is polynomial, so TSP is NP-hard.

---

## Formula box

**TSP as IP — degree constraints (assignment relaxation):**

- `min Σ_{i∈N} Σ_{j∈N} c_ij · x_ij`
- `Σ_{i∈N\{j}} x_ij = 1` for each `j ∈ N` (each city entered once)
- `Σ_{j∈N\{i}} x_ij = 1` for each `i ∈ N` (each city left once)
- `x_ij ∈ {0,1}`

Degree constraints alone are **not enough** — they permit **subtours** (this is exactly the assignment problem).

**Subtour elimination, SEC (Dantzig-Fulkerson-Johnson 1954):**

- `Σ_{i∈U} Σ_{j∈U} x_ij ≤ |U| − 1` for each `U ⊂ N`, `2 ≤ |U| ≤ n − 1`

**Special cases:**

- **2-city subtour:** `x_ij + x_ji ≤ 1`
- **3-city subtour:** `x_ij + x_jk + x_ki ≤ 2`

Number of constraints: exponential (`~2^n`).

**Subtour elimination, MTZ (Miller-Tucker-Zemlin 1960) — course form:**

- `u_1 = 1` (city 1 gets label 1)
- `2 ≤ u_i ≤ n` for each `i ∈ N\{1}`
- `u_j ≥ u_i + 1 − (n−1)(1 − x_ij)` for each `i,j ∈ N\{1}`, `i ≠ j`

**Equivalent rearrangement** (the form the CE writes, with `|V|` for `n`):

- `u_i − u_j + 1 ≤ (n − 1)(1 − x_ij)`

The often-quoted textbook form `u_i − u_j + n·x_ij ≤ n − 1` is the same family of constraints with big-M `= n` instead of `n−1`; **use the lecture's `(n−1)` version in the exam.** Number of constraints: `O(n²)`.

**Multi-agent / multi-depot m-TSP (CE D9.2, agents `k ∈ {F, M}` with depots `D_1, D_2`):**

- **(a) Visit each scooter site once:**
  - `Σ_{i∈V, i≠j} (x^F_ij + x^M_ij) = 1` for all `j ∈ V \ {D1, D2}`

- **(b) Each agent leaves / returns to own depot once:**
  - `Σ_{i≠D1} x^F_{D1,i} = 1 = Σ_{i≠D2} x^M_{D2,i}`
  - `Σ_{i≠D1} x^F_{i,D1} = 1 = Σ_{i≠D2} x^M_{i,D2}`

- **(c) Flow conservation (round trip) per agent:**
  - `Σ_{i≠j} x^k_ij = Σ_{i≠j} x^k_ji` for all `j ∈ V`, all `k ∈ {F, M}`
  - **Per-agent SEC:**
    - `Σ_{i,j∈U_F, i≠j} x^F_ij ≤ |U_F| − 1` for all `U_F ⊆ V \ {D1}`
    - `Σ_{i,j∈U_M, i≠j} x^M_ij ≤ |U_M| − 1` for all `U_M ⊆ V \ {D2}`
  - (or per-agent MTZ labels `u^F`, `u^M` with `u^k_{depot} = 1`)

- **(d) Objective:**
  - `min Σ_{i,j∈V, i≠j} c_ij (x^F_ij + x^M_ij)`

**Bounds (CE D9.1c numbers):**

- `L(MST) ≤ L(OPT)` -- lower bound from the tree
- `L(tour from a 2-approx) ≤ 2 · L(OPT)` implies `L(OPT) ≥ ½ · L(tour)`
- `best available lower bound = max( L(MST), ½·max_t L(tour_t) )`

CE D9.1 instance: `L(MST) = 13.24`; two shortcutting directions of the same doubled MST give
`L(outside) = 17.6` and `L(inside) = 22.19`, hence lower bounds `17.6/2 = 8.8`, `22.19/2 = 11.09`, and `13.24` from the MST itself. Best lower bound `= 13.24`; so `13.24 ≤ L(OPT) ≤ 17.6` (17.6 is in fact optimal here).

**Counting tours:**

- **Asymmetric:** `(n−1)!`
- **Symmetric:** `(n−1)!/2`

---
