# Network flow — from scratch

Teaching companion to [09-network-flow](09-network-flow.md). That file is the reference;
**this one assumes you know nothing**.

Part of exam slot **E6** (12–18 pts), which rotates between flow, matroids, TU, knapsack DP and
TSP. Flow is the cheapest of the five to learn and **`S8.3` is SS25 E6 word-for-word**, so this
is the highest-value hour in the whole combinatorial block.

---

# Part 0 — What the exam actually asks

**SS25 E6** (14 pts) — *Maximum flow and minimum cut counterexamples*:

> *"Consider the following statements. Use the given graphs to find a suitable counterexample
> for each one of them. Clearly describe why the given counterexample refutes the statement."*
>
> a) If all capacities are odd then there is a maximal `s–t` flow `f` such that `f(e)` is odd
>    for all `e ∈ E`.
> b) Adding a number `λ ∈ ℕ` to all capacities `c(e)` does not change the minimal cuts.

Both are **false**, and you're given a blank diamond graph to write capacities onto.

**SS21 A5** — the more traditional version: run Ford-Fulkerson, state the max flow, identify a
minimum cut.

So two skills: **execute the algorithm**, and **break a plausible-sounding claim**.

---

# Part 1 — What a flow network is

A directed graph where each arc has a **capacity** — the most that can pass through it. Think
water pipes, or traffic.

```
N = (V, E, u, s, t)

V   nodes
E   directed arcs
u   u(e) ≥ 0 — capacity of arc e
s   the SOURCE — where everything starts
t   the SINK — where everything must end up
```

A **flow** `f` assigns a number `f(e)` to each arc: how much is actually flowing through it,
as opposed to how much *could*.

## The two rules a flow must obey

**Rule 1 — capacity.** You can't push more through a pipe than it holds, and you can't push
negative amounts:
```
0  ≤  f(e)  ≤  u(e)        for every arc e
```

**Rule 2 — conservation.** At every node *except* `s` and `t`, what comes in must go out.
Nothing is created or stored:
```
Σ (flow in)  =  Σ (flow out)        for every v ∈ V \ {s, t}
```

`s` is exempt (it produces), `t` is exempt (it absorbs). That's all a flow is.

> In the min-cost formulation the course writes conservation with a supply term on the inflow
> side: `Σᵢ f(i,j) + bⱼ = Σᵢ f(j,i)`. Here `bⱼ > 0` is production, `bⱼ < 0` is consumption. For
> max-flow problems `b = 0` everywhere except `s` and `t`, which is the version above.

## Value of a flow

```
val(f) = total flow leaving s = Σⱼ f(s,j)
```

By conservation this equals the total arriving at `t` — nothing leaks in between. **The
max-flow problem is: make `val(f)` as large as possible.**

---

# Part 2 — Cuts

A **cut** is a way of splitting the nodes into two teams, with `s` on one side and `t` on the
other:

```
S = [X, V\X]      with  s ∈ X   and   t ∈ V\X
```

Delete the arcs going from `X` to `V\X` and there is no longer any route from `s` to `t`. A cut
is a *bottleneck you could impose*.

## Cut capacity — forward arcs only

```
cap(S)  =  Σ  u(i,j)      over arcs with  i ∈ X,  j ∈ V\X
```

**Only arcs pointing from the `s`-side to the `t`-side count.** Arcs pointing backwards — from
`V\X` back to `X` — contribute **nothing**.

Why: capacity measures how much *could* cross toward `t`. An arc pointing the wrong way can't
carry anything toward `t` at all, so it imposes no limit. Counting it would inflate the cut.

**This is the single most common error in the topic.** Write "forward only" next to your working.

---

# Part 3 — Weak duality, and why this is Day 2 again

Here's the elegant part. Take **any** flow `f` and **any** cut `S`. Everything reaching `t` has
to cross from `X` to `V\X` at some point, and the crossing capacity is `cap(S)`. So:

```
val(f)  ≤  cap(S)          for EVERY flow and EVERY cut
```

**That is weak duality** — the same statement, and the same shape of argument, as
`cᵀx ≤ bᵀy` in LP duality ([04a](04a-duality-lesson.md) Part 1). Every cut is a *certificate*
bounding every flow, exactly as every dual-feasible point bounds every primal-feasible one.

And the theorem:

> **Max-flow = min-cut.**
> ```
> max val(f)  =  min cap(S)
> ```

That's **strong duality** for this problem. The best flow exactly matches the tightest
bottleneck — no gap.

The practical consequence you'll use constantly:

> **If you find a flow and a cut with the same value, both are optimal — and you've proved it.**

No further work needed. That's your verification step every time.

---

# Part 4 — The residual network

This is the one genuinely new construction, and the one conceptual hurdle. Take it slowly.

Given a current flow `f`, build a new graph showing **what moves are still available**. For each
original arc `(i,j)`:

```
FORWARD arc  (i,j)  with capacity  u(e) − f(e)     ← spare room left
BACKWARD arc (j,i)  with capacity  f(e)            ← flow you could UNDO
```

Arcs with residual capacity 0 are dropped.

## Why the backward arc — the bit that matters

The forward arc is obvious: unused capacity. The backward arc looks strange. It exists so you can
**take back a routing decision you made earlier**. Without it, greedy path-finding gets stuck.

**Concrete example.** Five arcs, every capacity 1:
```
    s → a  (1)      a → t  (1)
    s → b  (1)      b → t  (1)
                    a → b  (1)
```

Suppose your first augmenting path is `s → a → b → t`, pushing 1 unit. Now `s→a`, `a→b`, `b→t`
are all full. Look for another `s→t` path using only unused arcs: `s→b` is free, but `b→t` is
full. `a→t` is free, but `s→a` is full. **Dead end at value 1.**

But the true maximum is **2** — send one unit `s→a→t` and another `s→b→t`.

The residual network finds it. After the first augmentation it contains the backward arc `b→a`
(capacity 1, because `f(a,b) = 1`). So this path exists:
```
s → b        (forward, spare capacity 1)
b → a        (BACKWARD — undoing 1 unit of the a→b flow)
a → t        (forward, spare capacity 1)
```
Augmenting along it adds 1 to `f(s,b)`, **subtracts** 1 from `f(a,b)`, adds 1 to `f(a,t)`. The
result is `f(s,a) = f(a,t) = f(s,b) = f(b,t) = 1` and `f(a,b) = 0` — value **2**. ✓

> **The backward arc is the algorithm's undo button.** It's what makes Ford-Fulkerson correct
> rather than merely greedy.

---

# Part 5 — Ford-Fulkerson

```
1. Start with f(e) = 0 on every arc.
2. Build the residual network.
3. Find ANY path s → … → t in it. If none exists, STOP — the flow is maximum.
4. κ = the smallest residual capacity along that path (the "bottleneck").
5. Augment: f += κ on forward arcs, f −= κ on backward arcs.  val += κ.
6. Go to 2.
```

**Edmonds-Karp** is the same algorithm with one rule added: always take a *shortest* augmenting
path (BFS). That's what makes it polynomial.

```
Ford-Fulkerson   O(|E| · U)     pseudopolynomial — depends on capacity size
Edmonds-Karp     O(V · E²)      polynomial
```

## Reading off the minimum cut when you finish

The algorithm hands you the min cut for free:

```
1. Take the FINAL residual network (after no augmenting path remains).
2. X = every node reachable from s in that residual network.
3. S = [X, V\X] is a minimum cut.
4. Check: cap(S) should equal val(f). If it doesn't, you've made an error.
```

Step 4 is a free correctness check. Use it every time.

Why it works: if no augmenting path exists, `t` is unreachable, so `t ∉ X` — it's a genuine cut.
And every arc leaving `X` must be *saturated* (else it would still be in the residual network and
extend `X`), so the cut's capacity equals the flow crossing it.

---

# Part 6 — Worked example: SS25 E6 / S8.3

You get a blank graph shaped `s → a → c`, `s → b → c`, `c → t`, and must **write capacities on
it** to break each claim.

## (a) "All capacities odd ⟹ some maximum flow has every `f(e)` odd"

**Counterexample.** Put capacity 1 on `s→a`, `a→c`, `s→b`, `b→c`, and 3 on `c→t`:

```
              a
        1  ↗     ↘  1
   s                 c  --3-->  t
        1  ↘     ↗  1
              b
```

All five capacities — 1, 1, 1, 1, 3 — are odd. ✓

Now the maximum flow: one unit through `a`, one through `b`, both merging at `c`, then both down
`c→t`. So `val(f) = 2`, and:
```
f(c,t) = 2      ← EVEN
```
The arc `c→t` carries 2. And that's forced — no maximum flow can do otherwise, since all flow
must funnel through `c→t`. **So the claim is false.** ∎

The mechanism: merging two odd flows produces an even one. Look for a bottleneck arc that
*sums* several others.

## (b) "Adding `λ` to every capacity doesn't change the minimal cuts"

**Same graph works.** With the capacities above there are two natural cuts:

```
cut around s:   X = {s}            arcs s→a, s→b        cap = 1 + 1 = 2   ← minimum
cut around t:   X = {s,a,b,c}      arc  c→t             cap = 3
```

The minimum cut is the one isolating `s`, with capacity 2.

Now add `λ = 2` to **every** capacity:

```
cut around s:   two arcs, each 1+2 = 3        cap = 6
cut around t:   one arc,       3+2 = 5        cap = 5   ← now the minimum
```

**The minimum cut has moved to the other side of the graph.** The set of minimal cuts changed,
so the claim is false. ∎

The mechanism, worth stating in your answer: **adding `λ` penalises cuts in proportion to how
many arcs they contain.** A 2-arc cut gains `2λ`; a 1-arc cut gains only `λ`. So cuts with fewer
arcs get relatively cheaper, and a narrow-but-expensive cut can overtake a wide-but-cheap one.

## How to attack any "give a counterexample" question

```
1. Keep it SMALL — 4 or 5 nodes. You need one broken case, not a general theory.
2. Look for the mechanism the claim ignores:
      merging flows      → parities combine
      counting arcs      → per-arc effects scale with cut width
      ties               → a tie broken the other way
3. Compute the actual numbers. Don't hand-wave.
4. WRITE THE SENTENCE saying which part of the claim fails. The exam says
   "clearly describe why" — the numbers alone don't score.
```

---

# Part 7 — Modelling transformations

Occasionally you're asked to convert a messier network into the standard single-source,
single-sink, arc-capacity form. Five standard moves ([09](09-network-flow.md) Procedures 4–7,
plus `D8.4`):

| Problem | Fix |
|---|---|
| **Several sources / sinks** | add a super-source `s` with arcs to each `sᵢ`, capacity `b(sᵢ)`; likewise a super-sink |
| **Node has a capacity** | split `v` into `v_in` and `v_out`, join them by an arc with that capacity; incoming arcs → `v_in`, outgoing → `v_out` |
| **Negative costs** | reverse the arc, negate the cost, adjust `b` at both endpoints |
| **Undirected edge** | replace with two opposite directed arcs |
| **Lower bounds on arcs** | shift flow to make the lower bound zero and adjust supplies |

Node-splitting is the one worth knowing cold — it's the standard trick and it's easy to state.

## Where flow sits in the bigger picture

```
assignment  ⊂  transportation  ⊂  min-cost flow  =  a linear program
shortest path, max flow        →  also special cases of min-cost flow
```

All of these are LPs whose constraint matrices are **totally unimodular**, which is why they
solve in polynomial time and why their LP relaxations come out integral.
→ [08-total-unimodularity-and-matroids](08-total-unimodularity-and-matroids.md)

---

# Part 8 — Traps and drills

## Where points are lost

1. **Counting backward arcs in a cut's capacity.** Forward only. The most common error.
2. **Forgetting backward arcs in the *residual* network.** Opposite direction, same word —
   residual backward arcs are essential, cut backward arcs are ignored. Keep them straight.
3. **Not verifying `cap(S) = val(f)`.** It's a free check and it catches arithmetic slips.
4. **Building the min cut from the original graph** instead of the *final residual* network.
5. **Giving a counterexample without the sentence.** "Clearly describe why" is in the prompt.
6. **Over-large counterexamples.** Four or five nodes is plenty; big graphs invite arithmetic
   mistakes for no extra marks.

## Say these without looking

- conservation: in = out at every node but `s` and `t`
- `0 ≤ f(e) ≤ u(e)`
- cut capacity counts **forward arcs only**
- `val(f) ≤ cap(S)` always; **max-flow = min-cut** at optimum
- residual: forward `u − f`, backward `f`
- min cut `X` = nodes reachable from `s` in the **final** residual network

## Warm-up ladder (untimed)

1. `theory/09` Procedures 1–3 — Ford-Fulkerson, residual network, reading the min cut.
2. **`S8.3`** *Max-flow and min-cut counterexamples* — `[SAME]` **this is SS25 E6 word-for-word,
   same graphs.** Do it properly, then check against the SS25 solutions.
3. `T8.2` *Maximum Flow* — `[DRILL]` only if executing Ford-Fulkerson is still shaky.
4. `D8.4` *Tips and tricks for network modeling* — `[CONCEPT]`, 10 min skim.

Sheet 8 is `exercises/09-integer-programming-network-flow/sheet-08-exercises.pdf`; the
self-study section (S8.x) is in the second half of the same file.

## Then the paper

- **SS21 A5** (flow part) — the traditional format: run the algorithm, state max flow, give a
  minimum cut.
