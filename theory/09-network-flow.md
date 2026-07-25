## Definitions

**Network (MCNFP form, CE D8.4).**

```
N = (V, E, c, u, b)
c : E → ℝ      transportation cost per unit of flow
u : E → ℝ⁺     capacity of an arc
b : V → ℝ      demand;  b(v) > 0 = production/supply,  b(v) < 0 = consumption
f : E → ℝ      the flow to be determined
```

**Flow conservation** (course's exact form — supply is added on the *inflow* side):

```
Σ_i f(i,j) + b_j = Σ_i f(j,i)      for all j ∈ V
```

**Capacity constraints.**

```
0 ≤ f(e) ≤ u(e)      for all e ∈ E
```

**Max-flow network (lecture form).** `N = (V, E, u, s, t)`: capacities only, plus a distinguished **source** `s` and **sink** `t`. No `b`; conservation holds at every node except `s, t`.

**Value of a flow.** `val(f) = Σ_{j∈V} f(s,j)` — total flow leaving the source (equivalently, entering `t`).

**Residual network.** Given flow `f`, replace each arc `e = (i,j)`:

```
forward arc  (i,j)  with residual capacity  u(e) − f(e)
backward arc (j,i)  with residual capacity  f(e)
```

Arcs of residual capacity 0 are dropped.

**Augmenting path.** A path `s → … → t` in the residual network all of whose arcs have strictly positive residual capacity. Its **bottleneck** `κ` is the minimum residual capacity along it.

**s-t cut.** A partition `S = [X, V\X]` with `s ∈ X ⊂ V` and `t ∈ V\X`. Removing the arcs from `X` to `V\X` destroys every `s–t` path.

**Capacity and flow value of a cut** (slide 59):

```
cap(S) = Σ_{(i,j): i∈X, j∈V\X}  u_ij                                    ← forward arcs ONLY
val(S) = Σ_{(i,j): i∈X, j∈V\X} f(i,j)  −  Σ_{(j,i): i∈X, j∈V\X} f(j,i)  ← forward minus backward
```

**Special cases of MCNFP** (slide 4/45/67, ordered specific → general): assignment problem ⊂ transportation problem; shortest path; maximum flow — all three reduce to the **minimum cost flow problem**, which is itself a linear program.

---

## Procedures

### 1. Ford-Fulkerson

1. Initialize `f(e) = 0` on all arcs.
2. Find any `s-t` path in the residual network; if none exists, stop -- flow is maximal.
3. Set `κ` = minimum residual capacity along that path.
4. Augment: `f(e) += κ` on forward arcs, `f(e) -= κ` on backward arcs; `val += κ`.
5. Rebuild the residual network and go to 2.

**Edmonds-Karp** is the same but always picks a shortest augmenting path via BFS.

### 2. Build the residual network

1. For each arc `(i,j)` with `f < u`: add forward arc `(i,j)` with capacity `u - f`.
2. For each arc `(i,j)` with `f > 0`: add backward arc `(j,i)` with capacity `f`.

### 3. Read the minimum cut

1. In the final residual network, let `X` = all nodes reachable from `s`.
2. `S = [X, V\X]` is a minimum cut; `cap(S) = Σ u(i,j)` for `i ∈ X, j ∉ X`.
3. Verify `cap(S)` equals the max-flow value.

### 4. Max-Flow to MCNFP

1. Add node `t'` and arc `(t, t')` with `c = -1`; all original arcs get `c = 0`.
2. Set `b(s) = -b(t') = |E| · max u(e)`, and `u(t,t') = -b(t')`.
3. Add bypass arc `(s, t')` with `c = 0`, `u = -b(t')`.
4. The optimal MCNFP flow on the original arcs is the max `s-t` flow; `-(objective)` = its value.

### 5. Multi-source/multi-sink to single s, t

1. Add super-source `s` with arcs `(s, s_i)` of capacity `b(s_i)` and cost 0.
2. Add super-sink `t` with arcs `(t_j, t)` of capacity `|b(t_j)|` and cost 0.
3. Set `b(s) = Σ b(s_i)`, `b(t) = Σ b(t_j)`, zero out original supply/demand values.

### 6. Removing negative costs

For each arc `e = (v₁, v₂)` with `c(e) < 0` and finite `u(e)`:

```
e' = (v₂, v₁),  c'(e') = -c(e),  u'(e') = u(e)
b'(v₁) = b(v₁) - u(e),  b'(v₂) = b(v₂) + u(e)
recover: f(e) = u(e) - f'(e')
```

### 7. Node capacities to arc capacities

1. Split node `v` into `v_in` and `v_out`.
2. Redirect all incoming arcs to `v_in`, all outgoing arcs from `v_out`.
3. Add arc `(v_in, v_out)` with cost 0 and capacity `ū`.

---

## Formula box

**Flow constraints**

- **Conservation:** `Σ_i f(i,j) + b_j = Σ_i f(j,i)` `∀ j ∈ V`
- **Capacity:** `0 ≤ f(e) ≤ u(e)` `∀ e ∈ E`
- **Flow value:** `val(f) = Σ_j f(s,j)`

**Residual network and augmentation**

- **Residual:** fwd: `u(e) − f(e)`, bwd: `f(e)`
- **Augment:** `κ` = min residual capacity on the s--t residual path

**Cuts**

- **Cut capacity:** `cap(S) = Σ_{i∈X, j∉X} u_ij` (forward arcs only!)
- **Cut flow value:** `val(S) = Σ_{i∈X, j∉X} f(i,j) − Σ_{i∈X, j∉X} f(j,i)`

**Duality**

- **Weak duality:** `val(f) ≤ cap(S)` for every flow `f`, every cut `S`
- **MFMC:** `max val(f) = min cap(S)`

**Min cut**

- **Min cut:** `X = { v : reachable from s in the FINAL residual network }`

**Complexity**

- **FF:** `O(|E| · U)` pseudopolynomial
- **Edmonds-Karp:** `O(V · E²)` polynomial (BFS = shortest augmenting path)

---
