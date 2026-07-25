## Definitions

| Term                       | Definition                                                                                                      |
| -------------------------- | --------------------------------------------------------------------------------------------------------------- |
| **LP**                     | Optimise a linear objective over finitely many linear (in)equalities                                            |
| **Canonical form**         | `max c^T x` s.t. `Ax ≤ b, x ≥ 0`                                                                                |
| **Standard form**          | `max c^T x` s.t. `Ax = b, x ≥ 0, b ≥ 0` — simplex input                                                         |
| **Slack / surplus**        | `a^Tx ≤ b` ⟺ `a^Tx + s = b, s ≥ 0`; `a^Tx ≥ b` ⟺ `a^Tx − s = b, s ≥ 0`                                          |
| **Free variable split**    | `x ∈ R` ⟺ `x = x⁺ − x⁻`, `x⁺, x⁻ ≥ 0`                                                                           |
| **Hyperplane / halfspace** | `H = {x \| a^Tx = b}`; `H⁻ = {x \| a^Tx ≤ b}`. `a` = outward normal                                             |
| **Polyhedron**             | `P = {x \| Ax ≤ b}` — finite intersection of halfspaces. **Polytope** = bounded polyhedron                      |
| **Convex set**             | `x,y ∈ X, α ∈ [0,1]` ⟹ `(1−α)x + αy ∈ X`                                                                        |
| **Convex combination**     | `Σ αᵢ vᵢ` with `αᵢ ≥ 0`, `Σ αᵢ = 1`                                                                             |
| **Active constraints**     | `I(x) = {i \| aᵢ^Tx = bᵢ}`, `J(x) = {j \| xⱼ = 0}`                                                              |
| **Vertex = extreme point** | not a convex combination of two other points; equivalently `n` lin. indep. active constraints                   |
| **BFS**                    | pick basis `B` (`m` lin. indep. columns), set `x_N = 0`, `x_B = B⁻¹b`; feasible iff `x_B ≥ 0`. **Vertex ⟺ BFS** |
| **Adjacent vertices**      | share `n−1` active constraints; one simplex pivot apart                                                         |
| **Recession direction**    | `d ≠ 0` with `x + td ∈ X` ∀`t ≥ 0`. `rec(X) = {d \| Ad ≤ 0, d ≥ 0}`. Bounded ⟺ `rec(X) = {0}`                   |
| **Tangent cone**           | `T_X(x) = cl({d \| x + td ∈ X, t > 0})` — feasible directions at `x`                                            |
| **Normal cone**            | `N_X(x) = {v \| ⟨v,d⟩ ≤ 0 ∀d ∈ T_X(x)}` = cone of active constraint normals                                     |
| **LP assumptions**         | proportionality, additivity, divisibility (variables real), certainty                                           |

## Procedures

**A. Model an LP**
1. Name index sets.
2. Define variables with units. Add auxiliary/split variables before writing constraints if thresholds appear.
3. Write the objective (revenue minus all variable costs).
4. Write one constraint per sentence of the problem text, then bounds/non-negativity.
5. Explain each constraint by number.

**B. Graphical solution**
1. Draw the coordinate system (`x_1, x_2 ≥ 0` quadrant).
2. Rewrite each constraint as an equation solved for `x_2` and draw the line.
3. Mark the feasible side of each line (test the origin), shade the intersection.
4. Rewrite the objective as `x_2 = z/c_2 − (c_1/c_2)x_1` — parallel lines with slope `−c_1/c_2`.
5. Shift the objective line in direction `+c` (max) / `−c` (min) until it last touches `X`.
6. Read the vertex, solve the two active equations exactly, report `x*` and `z*`.

**C. Convert to standard form `max c^T x, Ax = b ≥ 0, x ≥ 0`**
1. `min c^T x → max (−c)^T x`.
2. `≤` row: `+ slack`; `≥` row: `− surplus`; both `≥ 0`.
3. Equality as inequalities: `a^Tx = b ⟺ a^Tx ≤ b ∧ a^Tx ≥ b`.
4. Negative RHS: multiply row by `−1`.
5. Free variable: `x = x⁺ − x⁻`, `x⁺,x⁻ ≥ 0`. Lower bound `x ≥ l`: substitute `x' = x − l`.
6. Upper bound `x ≤ u` becomes an ordinary row.

**D. Find all `c` with multiple optima**
1. List the vertices and edges/rays of `X`.
2. For each edge direction `d`: objective is constant on it iff `c^T d = 0`. Take `c` = outward normal of the facet containing that edge.
3. Discard normals whose facet is not attained (LP unbounded in that direction).
4. Report each `c` with `z = c^T v` for a vertex `v` on that edge.

**E. Decide the four outcomes**
1. Is `X = ∅`? → **infeasible**.
2. Is there `d ∈ rec(X)` with `c^T d > 0`? → **unbounded**.
3. Is `c` orthogonal to an optimal edge? → **infinitely many optima**, write `{(1−α)v_1 + αv_2 | α ∈ [0,1]}`.
4. Else `c ∈ int N_X(v)` → **unique optimum** at `v`.

| Outcome | Geometric picture | Algebraic test | Reported answer |
|---|---|---|---|
| Unique optimum | objective line touches `X` in exactly one corner | `c ∈ int N_X(v)` | `x* = v`, `z* = c^Tv` |
| Infinitely many optima | objective line lies flat **on** an edge/facet | `c ⊥ (v_2−v_1)`, `c = λa_i`, `λ>0` | `{(1−α)v_1+αv_2 : α∈[0,1]}`, single `z*` |
| Unbounded | region open in a direction `d` with `c^Td > 0` | `∃ d ∈ rec(X): c^Td > 0` | "unbounded", `z → ∞` |
| Infeasible | no shaded area | `X = ∅` | "infeasible", no `z` |

## Formula box

**LP Forms**

- **Canonical:** `max c^T x` s.t. `Ax ≤ b`, `x ≥ 0`
- **Standard:** `max c^T x` s.t. `Ax = b`, `x ≥ 0`, `b ≥ 0`
- `min c^T x = − max (−c)^T x`
- `a^Tx ≤ b` ⟺ `a^Tx + s = b, s ≥ 0`
- `a^Tx ≥ b` ⟺ `a^Tx − s = b, s ≥ 0`
- `x` free ⟺ `x = x⁺ − x⁻`, `x⁺, x⁻ ≥ 0`

**Geometry**

- `X = {x | Ax ≤ b, x ≥ 0}` — convex polyhedron; polytope = bounded polyhedron
- vertex `v` ⟺ `n` lin. indep. active constraints at `v` ⟺ BFS (`x_B = B⁻¹b ≥ 0`, `x_N = 0`)
- `rec(X) = {d | Ad ≤ 0, d ≥ 0}` — `X` bounded ⟺ `rec(X) = {0}`

**Cones**

- `T_X(x) = cl({d | x + td ∈ X, t > 0})`
- `N_X(x) = {v | ⟨v,d⟩ ≤ 0 ∀d ∈ T_X(x)} = { Σ_{i∈I(x)} λ_i a_i − Σ_{j∈J(x)} μ_j e_j | λ,μ ≥ 0 }`

**Optimality Conditions**

- **Optimality (max):** `x*` optimal ⟺ `∇f(x*) = c ∈ N_X(x*)`
- **Unbounded (max):** `X ≠ ∅` and `∃ d ∈ rec(X)` with `c^T d > 0`
- **Multi-optima:** `c^T(v_2 − v_1) = 0` for adjacent optimal `v_1, v_2`
- **Optimal face:** `x(α) = (1−α)v_1 + αv_2`, `α ∈ [0,1]`, `c^T x(α) = z*` ∀α

**Modelling Idioms**

- **Ratio** "≥ 60% of all units are A": `x_A ≥ 0.6(x_A+x_B)` ⟺ `0.4x_A − 0.6x_B ≥ 0` ⟺ `−2x_A + 3x_B ≤ 0`
- **Max:** `max{f_1(x), f_2(x)} ≤ c` ⟺ `f_1(x) ≤ c` AND `f_2(x) ≤ c` (one row per i)
- **Abs:** `|f(x)| ≤ c` ⟺ `f(x) ≤ c` AND `−f(x) ≤ c` (drop |·| if `f ≥ 0` known)
- **Min-max:** `max_i f_i(x) → min` ⟺ `min z` s.t. `f_i(x) ≤ z ∀i`
- **Piecewise cost** (2h/unit up to 30, then 3h): `x_A = x_{A,L} + x_{A,H}`, `x_{A,L} ≤ 30`, `x_{A,L}, x_{A,H} ≥ 0` — machine row becomes `2x_{A,L} + 3x_{A,H} + x_B ≤ 100`
- **Inventory balance** (multi-period): `L_m = L_{m−1} + P_m − F_m`, `L_0 = L_T = 0`
- **Up/down change:** `P_m = P_{m−1} + P⁺_m − P⁻_m`, `P⁺, P⁻ ≥ 0`
