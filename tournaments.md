# Tournaments

## Setting

This is the last setting we’re going to look at.

A tournament $$T$$ can be represented formally by a set of alternatives $$A$$, and a pairwise dominance relation $$\succ$$ between any two alternatives. That is, $$T = (A, \succ)$$.

Here, $$a \succ b$$ means that alternative $$a$$ dominates alternative $$b$$.

And for every pair of alternatives, exactly one dominates the other. So, the “tournament graph” $$(A, \succ)$$ is has $$\binom n 2$$ directed edges (it’s a complete graph). This is the most common way to represent a tournament.

Examples of use-cases include sports (where some teams beat others), elections (where we can form a majority graph, given a ranking of candidates by all voters), webpage ranking, biological interactions (for exampe, the pecking order in a flock of chickens).

## Terminology

**Outdegree** of $$x \in A$$ is the number of alternatives dominated by $$x$$ (it’s called outdegree because it’s the number of outgoing edges from $$x$$ in the tournament graph).

Outdegree is a reasonable measure of how strong an alternative is in some situations.

Note that in any tournament graph, the outdegree + indegree = $$n-1$$, where $$n$$ is the number of alternatives.

**Condorcet Winner**: An alternative that dominates all other alternatives (i.e., outdegree = $$n-1$$)

**Condorcet Loser**: An alternatiev that is dominated by all other alternatives (i.e., outdegree = 0)

## Tournament Solutions

A tournament solution is a method of choosing the “winners” (in fact, this mechanism is what defines who “wins”) of any tournament. Formally, it is a mapping from any tournament instance to a non-empty subset of alternatives.

A reasonable requirement of any tournament solution is to be invariant under isomorphisms. That is, relabelling the alternatives (but preserving the structure of the tournament graph) should yield the same winners. Basically, the algorithm should not use labels of the alternatives (e.g. names of sports teams) to determine who wins. An example is that the algorithm should not tie-break by alphabetical ordering of the alternatives.

Formally, if $$h: A \to A'$$ is an isomorphism between 2 tournaments, then the subset of that a tournament solution $$S$$ chooses from $$T'$$ is the image (with respect to $$h$$) of the subset that $$S$$ chooses from $$T$$.

As a direct consequence of this, in a cyclic tournament of size 3, every tournament solution must select ALL alternatives.

2 other common-sense axioms that we want our tournament solutions to satisfy are:

1. **Condorcet Consistency**: If there is a Condorcet winnner $$x$$, then $$x$$ is uniquely chosen.
2. **Monotonicity**: If $$x$$ is chosen, then it should remain chosen when it is strengthened against another alternative $$y$$ and everything else remains the same. (i.e., improving a candidate’s strength cannot convert it from a winner to a loser.)

### Examples

Examples of possible tournament solutions:

1. **Copeland set (CO)**: Alternatives with the highest outdegree.
2. **Top cycle (TC)**: Alternatives that can reach every other alternative via a directed path (of any length)
3. **Uncovered set (UC)**: Alternatives that can reach every other alternative via a directed path of length $$\leq 2$$
4. **Banks set (BA)**: Alternatives that appear as the maximal (i.e., strongest) element of some transitive subtournamnet that cannot be extended. (A transitive tournament is one in which the alternatives can be ordered as $$a_1, \cdots, a_k$$ so that $$a_i$$ dominates $$a_j$$ for all $$i < j$$.)

### Top Cycle

An equivalent definition of Top cycle is that it is the unique smallest nonempty set $$B$$ of alternatives such that all alternatives in $$B$$ dominate all alternatives outside $$B$$.

First, we need to show that such a set $$B$$ is a well-defined / valid tournament solution. Clearly the set of all the alternatives satisfies the property that “all alternatives in $$B$$ dominate all alternatives outside $$B$$” trivially — so we can always return some valid solution.

Next, we that the smallest such set $$B$$ must be unique. Suppose not. Say, there were 2 smallest sets $$C$$ and $$D$$ such that both satisfy “all alternatives inside the set dominate all alternatives outside it”. This is clearly not possible: it is impossible to find $$x \in C$$ and $$y \in D$$ such that $$x \notin D$$ and $$y \notin C$$. If we did, then by definition, we would need an arrow from $$x \to y$$ as well as from $$y \to x$$ (but both cannot dominate each other). (And one can’t be a strict subset of the other because of our requirement for “smallest” such set.)

So, we’ve shown that there is such a unique smallest nonempty set $$B$$.

Finally, we need to show that this definition of $$B$$ is equivalent to our original definition.

Observe that any $$p \notin B$$ cannot reach any $$q \in B$$ (because everything in $$B$$ dominates everything outside $$B$$, so all the edges from $$B$$ are outwards), hence $$p$$ is not in TC.

Further, any $$q \in B$$ can reach any $$p \notin B$$ directly.

So, all we have to show is that any $$q \in B$$ can also reach any other $$r \in B$$ (because for $$q$$ to be in TC, it needs to be able to reach _all_ other alternatives). This is where we can make use of the “smallest” criteria in the definition of $$B$$. Suppose not. Then, $$q$$ cannot reach $$r$$. Find all the alternatives in $$B$$ that can reach $$r$$ (including $$r$$ itself) — and we claim that this is a smaller set such that every alternative inside it dominates every alternative outside it (it’s smaller because it does not include $$q$$ and it is a subset of $$B$$). It’s a valid such set because if any other alternative outside this set could dominate anything inside it, then it would be able to reach $$r$$ via a path (of any length), and would’ve already been inside this set in the first place.

Hence, we’ve proved that the two definitions are equivalent.

### Uncovered Set

Covering relation: An alternative $$x$$ covers another alternative $$y$$ iff:

* $$x$$ dominates $$y$$, and
* For any $$z$$, if $$y$$ dominates $$z$$, then $$x$$ also dominates $$z$$.

It’s a very strong indicator that $$x$$ is better than $$y$$ → literally “i beat you, and i also beat everyone you beat.”

We claim that an equivlent definition of UC is the set of all uncovered alternatives (which is where the name comes from)

Suppose $$x$$ can reach any $$y$$ in at most 2 steps. Then, either $$x$$ dominates $$y$$ (path length = 1) or $$x$$ dominates some $$z$$ that dominates $$y$$ (path length = 2). In either case, $$y$$ cannot cover $$x$$. Similarly, we can show the other direction. So, $$x$$ can reach $$y$$ in 2 steps $$\iff$$ $$y$$ does not cover $$x$$.

## Containment Relations

<figure><img src=".gitbook/assets/Screenshot_2024 11 09_at_7.17.57_PM.png" alt=""><figcaption></figcaption></figure>

We prove some of them:

1. UC $$\subseteq$$ TC: trivial.
2. CO $$\subseteq$$ UC: Let $$x \in$$ CO. Then, $$x$$ has the highest outdegree among all alternatives. Suppose for contradiction that $$y$$ covers $$x$$. Then, $$y$$ must dominate $$x$$ as well as everyone that $$x$$ dominates. Clealry, $$y$$ must have outdegree strictly more than $$x$$. Contradiction.
3. BA $$\subseteq$$ UC: Let $$x \in$$ BA. Then, there is a transitive tournament $$T'$$ with $$x$$ as the maximal element (head) that cannot be extended. Suppose for contradiction that $$y$$ covers $$x$$. Then, $$y$$ can extend $$T'$$ since it beats $$x$$ and beats everyone that $$x$$ beats. Contradiction.

### Others

There are some other tournament solutoins (which goes to show that people try to come up with novel and fancy methods of solving this problem):

1. **Slater set (SL)**: Alternatives that are maximal elements in some transitive tournament that can be obtained by inverting as few edges as possible.
2. **Bipartisan set (BP)**: Alternatives that are chosen with nonzero probability in the (unique!) Nash equilibrium of the zero-sum game formed by the tournment matrix.
3. **Markov set (MC)**: Alternatives that stay most often in the “winner stays” competition corresponding to the tournament (i.e., the limiting distribution of the markov chain modeled by the tournament where the randomness lies in selecting the next candidate to play against the current winner, not in who beats whom.)
