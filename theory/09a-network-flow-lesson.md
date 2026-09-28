# Network flow

Companion to [09-network-flow](09-network-flow.md).

Exam slot E6/P6 (12–18 pts), which rotates between flow, matroids, TU, knapsack DP and TSP. `S8.3` is SS25 E6 word for word.

---

# Part 0: What the exam asks

**SS25 E6** (14 pts), *Maximum flow and minimum cut counterexamples*:

> *"Consider the following statements. Use the given graphs to find a suitable counterexample for each one of them. Clearly describe why the given counterexample refutes the statement."*
>
> a) If all capacities are odd then there is a maximal $s$–$t$ flow $f$ such that $f(e)$ is odd for all $e \in E$.
> b) Adding a number $\lambda \in \mathbb{N}$ to all capacities $c(e)$ does not change the minimal cuts.

Both are false; you write capacities onto a blank diamond graph.

**SS21 A5:** run Ford–Fulkerson, state the max flow, give a minimum cut.

**2026 P6** (17 cr): starting from a given flow, list the residual network, run Ford–Fulkerson (one path needs a backward arc), give a cut equal to the flow value, and justify integrality.

Two skills: run the algorithm, and break a claim with a counterexample.

---

# Part 1: Flow networks

$$N = (V, E, u, s, t)$$

| Symbol | Meaning |
|---|---|
| $V$ | nodes |
| $E$ | directed arcs |
| $u$ | $u(e) \ge 0$, capacity of arc $e$ |
| $s$ | source |
| $t$ | sink |

A flow $f$ assigns $f(e)$ to each arc.

**Capacity:**

$$0 \le f(e) \le u(e) \qquad \text{for every arc } e$$

**Conservation** at every node except $s$ and $t$:

$$\sum (\text{flow in}) = \sum (\text{flow out}) \qquad \text{for every } v \in V \setminus \{s, t\}$$

The course's min-cost form writes conservation with a supply term: $\sum_i f(i,j) + b_j = \sum_i f(j,i)$, with $b_j > 0$ production and $b_j < 0$ consumption. For max flow, $b = 0$ except at $s$ and $t$.

**Value:**

$$\text{val}(f) = \text{total flow leaving } s = \sum_j f(s,j)$$

By conservation this equals the flow arriving at $t$. Max flow: maximise $\text{val}(f)$.

---

# Part 2: Cuts

A cut splits the nodes into two sets with $s$ on one side and $t$ on the other:

$$S = [X, V\setminus X] \qquad \text{with } s \in X \text{ and } t \in V\setminus X$$

Any split with $s \in X$ and $t \notin X$ is a cut.

## Cut capacity: forward arcs only

$$\text{cap}(S) = \sum u(i,j) \qquad \text{over arcs with } i \in X,\; j \in V\setminus X$$

Only arcs from the $s$-side to the $t$-side count, using their capacities. Arcs from $V\setminus X$ back into $X$ and arcs inside one side contribute nothing. Counting backward arcs is the most common error in this topic.

---

# Part 3: Max-flow min-cut

For any flow $f$ and any cut $S$, all flow reaching $t$ must cross from $X$ to $V\setminus X$, so

$$\text{val}(f) \le \text{cap}(S) \qquad \text{for every flow and every cut}$$

This is weak duality, the same shape as $c^Tx \le b^Ty$ in LP ([04a](04a-duality-lesson.md) Part 1).

**Theorem (max-flow = min-cut):**

$$\max \text{val}(f) = \min \text{cap}(S)$$

Consequence: a flow and a cut with the same value are both optimal. This is the standard proof of optimality.

$$\text{any flow} \le \text{max flow} = \text{min cut} \le \text{any cut}$$

---

# Part 4: The residual network

For a current flow $f$ and each original arc $(i,j)$:

$$
\begin{aligned}
&\text{forward arc } (i,j) \text{ with residual capacity } u(e) - f(e) && \text{spare room} \\
&\text{backward arc } (j,i) \text{ with residual capacity } f(e) && \text{flow you can undo}
\end{aligned}
$$

Drop arcs with residual capacity 0.

## Why backward arcs are needed

Five arcs, all capacity 1:
```
    s → a  (1)      a → t  (1)
    s → b  (1)      b → t  (1)
                    a → b  (1)
```

Push 1 along $s \to a \to b \to t$. Now $s\to a$, $a\to b$, $b\to t$ are full. Using forward arcs only, $s\to b$ leads to the full $b\to t$, and $a\to t$ is only reachable through the full $s\to a$. Stuck at value 1, but the maximum is 2.

The residual network contains the backward arc $b\to a$ (capacity 1, since $f(a,b) = 1$), giving the path

$$
\begin{aligned}
&s \to b && \text{forward, residual 1} \\
&b \to a && \text{backward, undoes 1 unit on } a\to b \\
&a \to t && \text{forward, residual 1}
\end{aligned}
$$

Augmenting adds 1 to $f(s,b)$, subtracts 1 from $f(a,b)$, adds 1 to $f(a,t)$. Result: $f(s,a) = f(a,t) = f(s,b) = f(b,t) = 1$, $f(a,b) = 0$, value 2.

Backward arcs let the algorithm undo earlier routing choices; without them it can stop below the maximum. They matter most when you start from a given flow, as in 2026 P6.

## Updating after an augmentation

For each arc on the path, pushing $\kappa$:

$$
\begin{aligned}
&\text{residual in the path's direction} && -\kappa \\
&\text{residual in the opposite direction} && +\kappa
\end{aligned}
$$

If the path used a backward arc $(j,i)$, the flow on the original arc $(i,j)$ decreases by $\kappa$.

---

# Part 5: Ford–Fulkerson

1. Start with $f(e) = 0$ (or the given flow).
2. Build the residual network.
3. Find any $s \to t$ path in it with all residuals $> 0$. If none exists, stop: $f$ is maximum.
4. $\kappa$ = smallest residual capacity on the path (bottleneck).
5. Augment: $f \mathrel{+}= \kappa$ on forward arcs, $f \mathrel{-}= \kappa$ on backward arcs. $\text{val} \mathrel{+}= \kappa$.
6. Go to 2.

Any path with positive residuals is allowed and gives the same final value. The choice only affects the number of iterations. **Edmonds–Karp** always takes a shortest path (BFS), which makes it polynomial.

| Algorithm | Running time | |
|---|---|---|
| Ford–Fulkerson | $O(\lvert E\rvert \cdot U)$ | pseudopolynomial |
| Edmonds–Karp | $O(V \cdot E^2)$ | polynomial |

## Integrality

$$
\begin{aligned}
&u \text{ integral, start } f = 0 \\
&\implies \text{all residual capacities } u - f \text{ and } f \text{ stay integral} \\
&\implies \text{every bottleneck } \kappa \text{ is an integer} \ge 1 \\
&\implies \text{each augmentation raises } \text{val}(f) \text{ by} \ge 1 \\
&\implies \text{FF terminates } (\text{val}(f) \le \text{cap}(S) \text{ bounds it}) \\
&\phantom{\implies} \text{and returns a max flow with integral } f(e) \text{ on every arc.}
\end{aligned}
$$

This is also the termination proof; with irrational capacities FF need not terminate. 2026 P6e asked for this argument, so write the chain, not just the conclusion. (Alternative: the max-flow LP has a totally unimodular constraint matrix.)

## Reading off the minimum cut

1. Take the final residual network (no augmenting path left).
2. $X$ = every node reachable from $s$ in it.
3. $S = [X, V\setminus X]$ is a minimum cut.
4. Check $\text{cap}(S) = \text{val}(f)$.

Why: $t$ is unreachable, so this is a cut. Every arc leaving $X$ is saturated (otherwise its forward residual would extend $X$), and every arc entering $X$ carries zero flow (otherwise its backward residual would extend $X$). So $\text{val}(f) = \text{cap}(S) - 0$.

Sanity check on the final flow: $S\to T$ arcs full, $T\to S$ arcs empty.

---

# Part 6: Worked example, SS25 E6 / S8.3

The given graph: $s \to a \to c$, $s \to b \to c$, $c \to t$.

## (a) "All capacities odd $\implies$ some maximum flow has every $f(e)$ odd"

Capacities 1 on $s\to a$, $a\to c$, $s\to b$, $b\to c$, and 3 on $c\to t$:

```
              a
        1  ↗     ↘  1
   s                 c  --3-->  t
        1  ↘     ↗  1
              b
```

All capacities are odd. The maximum flow sends one unit through $a$ and one through $b$; both merge at $c$, so

$$f(c,t) = 2 \qquad \text{even}$$

Every maximum flow must send both units through $c\to t$, so the claim is false. Mechanism: two odd flows merging give an even flow.

## (b) "Adding $\lambda$ to every capacity does not change the minimal cuts"

Same graph and capacities:

$$
\begin{aligned}
&\text{cut around } s: && X = \{s\} && \text{arcs } s\to a,\ s\to b && \text{cap} = 1 + 1 = 2 && \text{minimum} \\
&\text{cut around } t: && X = \{s,a,b,c\} && \text{arc } c\to t && \text{cap} = 3
\end{aligned}
$$

Add $\lambda = 2$ to every capacity:

$$
\begin{aligned}
&\text{cut around } s: && \text{two arcs, each 3} && \text{cap} = 6 \\
&\text{cut around } t: && \text{one arc, 5} && \text{cap} = 5 && \text{now the minimum}
\end{aligned}
$$

The minimum cut moved, so the claim is false. Mechanism: adding $\lambda$ raises a cut by $\lambda$ times its number of arcs, so cuts with fewer arcs gain less.

## Counterexample method

1. Keep it small: 4–5 nodes.
2. Find the mechanism the claim ignores:
    - merging flows $\to$ parities combine
    - counting arcs $\to$ per-arc changes scale with cut size
    - ties $\to$ break the tie the other way
3. Compute the numbers.
4. Write the sentence stating which part of the claim fails.

---

# Part 7: Modelling transformations

([09](09-network-flow.md) Procedures 4–7, `D8.4`)

| Problem | Fix |
|---|---|
| several sources / sinks | super-source $s$ with arcs to each $s_i$, capacity $b(s_i)$; likewise a super-sink |
| node capacity | split $v$ into $v_{\text{in}}$ and $v_{\text{out}}$ joined by an arc with that capacity; incoming arcs go to $v_{\text{in}}$, outgoing leave $v_{\text{out}}$ |
| negative costs | reverse the arc, negate the cost, adjust $b$ at both ends |
| undirected edge | two opposite directed arcs |
| lower bounds on arcs | shift flow so the lower bound is zero and adjust supplies |

$$
\begin{aligned}
&\text{assignment} \subset \text{transportation} \subset \text{min-cost flow} = \text{a linear program} \\
&\text{shortest path, max flow} \to \text{also special cases of min-cost flow}
\end{aligned}
$$

All have totally unimodular constraint matrices, so their LP relaxations are integral. See [08-total-unimodularity-and-matroids](08-total-unimodularity-and-matroids.md).

---

# Part 8: Traps and practice

## Where points are lost

1. Counting backward arcs in a cut's capacity.
2. Forgetting backward arcs in the residual network. (Cut: ignore them. Residual: essential.)
3. Not checking $\text{cap}(S) = \text{val}(f)$.
4. Reading the min cut from the original graph instead of the final residual network.
5. Giving a counterexample without the explanatory sentence.
6. Using a large counterexample.

## Recall list

- conservation: in = out at every node except $s$ and $t$
- $0 \le f(e) \le u(e)$
- cut capacity: capacities of $S\to T$ arcs only
- $\text{val}(f) \le \text{cap}(S)$ always; max flow = min cut
- residual: forward $u - f$, backward $f$
- min cut $X$ = nodes reachable from $s$ in the final residual network

## Exercises (untimed)

1. `theory/09` Procedures 1–3: Ford–Fulkerson, residual network, min cut.
2. `S8.3` *Max-flow and min-cut counterexamples* `[SAME]`: SS25 E6 with the same graphs. Check against the SS25 solutions.
3. `T8.2` *Maximum Flow* `[DRILL]`: if running Ford–Fulkerson is still slow.
4. `D8.4` *Tips and tricks for network modeling* `[CONCEPT]`: 10-minute skim.

Sheet 8: `exercises/09-integer-programming-network-flow/sheet-08-exercises.pdf`; S8.x is in the second half.

## Papers

- 2026 P6: residual network from a given flow, backward-arc augmentation, cut, integrality.
- SS21 A5 (flow part): run the algorithm, state the max flow, give a minimum cut.
