## Definitions

**Master problem (pattern formulation).** Instead of "which piece goes on which board", enumerate all *valid patterns* (columns). For cutting stock with pattern set `Γ`, `a_ij` = number of pieces of length `ℓ_i` in pattern `j`, `x_j` = how often pattern `j` is used:

```
min  Σ_{j∈Γ} c_j x_j
s.t. Σ_{j∈Γ} a_ij x_j ≥ b_i    ∀i = 1..m
     x_j ∈ ℕ₀
```

Advantage: **strong LP relaxation** (the naive bin-packing/assignment model `Σ_i ℓ_i y_ij ≤ L x_j` has a terrible bound). Disadvantage: `|Γ| = n` is astronomically large.

**Restricted master problem (RMP).** The master LP with only a subset of columns kept in memory (an initial feasible basis plus whatever CG has generated). Everything else is treated as non-basic and never written down.

**Dual prices / shadow prices / utility vector.** `π^T = c_B^T B⁻¹ ∈ ℝ^m`. One price per *row* (per demand type). Also written `u` or `y` in the exercises. Key point: `π` has only `m` entries — cheap, no matter how large `n` is.

**Column / pattern.** A vector `a ∈ ℕ₀^m` (cutting stock) or `a ∈ {0,1}^m` (set partitioning) that satisfies the problem's *validity* rules. `c_a` is its cost.

**Pricing problem.** An optimisation problem *over the space of valid columns*, whose objective is the negated reduced cost. It replaces "scan all `n` columns" by "solve one small IP".

**Branch-and-price.** Branch-and-bound where each node's LP relaxation is solved by column generation.

## Procedures

### One CG iteration

1. **Solve the RMP** with the current column subset; read off basis `B` and `c_B`.
2. **Compute duals:** `π^T = c_B^T B⁻¹`.
3. **Solve the pricing problem:** `max_a { π^T a − c_a : a valid }`.
4. **Check optimum:** if `≤ 0` → stop, current solution is optimal; if `> 0`, `a*` enters the basis.
5. **Min-ratio test:** compute `b' = B⁻¹b` and `N' = B⁻¹a*`; the leaving variable is `argmin_k { b'_k / N'_k : N'_k > 0 }`. Update `B`, `c_B`, repeat.

### Formulating a pricing problem

1. **Define what a column represents** and name the decision variables.
2. **Write the validity constraints** that make a column admissible in the master.
3. **Set the objective** to `max Σ_i π_i y_i − c_y`. If multiple resource types exist, write one pricing IP per type and take the best.

### When is CG worth it

CG pays off when `#columns ≫ #rows`. Count `n = |Γ|` combinatorially and compare with `m`. Few conflicts → exponentially many valid columns → use CG. Many conflicts → `n ≈ m` → write the full LP.

## Formula box

**Duals and reduced cost**

- **Duals (shadow prices):** `π^T = c_B^T B⁻¹` (m entries only)
- **Reduced cost (min master):** `c̄_j = c_j − π^T a_j`
- **Improving column:** `c̄_j < 0`

**Pricing problem (course convention, = −c̄)**

- **Objective:** `z* = max_a { π^T a − c_a : a is a valid column }`

**Termination test**

- `z* ≤ 0` ⇒ RMP optimum = full-master optimum
- `z* > 0` ⇒ `a*` enters the basis

**Leaving variable**

- **Min-ratio:** over `b' = B⁻¹b` vs `N' = B⁻¹a*`
- **Formula:** `argmin_k { b'_k / N'_k : N'_k > 0 }`
