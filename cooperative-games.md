# Cooperative Games

## Cooperative Games

Players divide into coalitions to perform takss. Within a coalition, members can freely divide profits. The question is how should the profits be divided?

### Induced Subgraph Games

<figure><img src=".gitbook/assets/Screenshot_2024 09 12_at_5.42.43_PM.png" alt=""><figcaption></figcaption></figure>

We are given a weighted graph where the players are nodes; and the value of a coalition is the value of the total edge weights in the subgraph.

So, an edge $$(u,v)$$ tells you how much value $$(u,v)$$ would generate if they work together. e.g. researchers collaborate to each other (and the total value is determined by the sum of their pairwise values).

(And it seems natural / intuitive to assume that if $$(u,v) = k$$ then each of $$u$$ and $$v$$ contribute $$k/2$$ to this synergistic “relationship”).

### Network Flow Games

<figure><img src=".gitbook/assets/Screenshot_2024 09 12_at_5.42.56_PM.png" alt=""><figcaption></figcaption></figure>



We are given a weighted directed graph. Players are edges. And the value of a coalition is the value of the maximum flow it can pass from $$s$$ to $$t$$. This has applications in computer network, traffic flow, transport networks. (that is, the minimum bottleneck value on the path).

### Weighted Voting Games

We are given a list of weights and a threshold, i.e., $$(w_1, \cdots, w_n; q)$$, all of which are positive integers. Each player $$i$$ has a weight $$i$$.

The value of a coalition is 1 (winning) if its total weight is at least $$q$$ (winning), and 0 (losing) otherwise.

Applications: parliaments, UN security council, EU council of members.

{% hint style="info" %}
Cost sharing is just the inverse of profit sharing. So it’s actually the “same” problem underneath the hood — if you have an algorithm that can fairly divide profits, you can negate the values to convert them to costs (i.e., change maximization to minimization).
{% endhint %}

### General Framework

* A set of players - $$N = \{1, \cdots, n \}$$
* A valuation function - $$v : 2^N \to \mathbb{R}_{\geq 0}$$
  * That is, each subset of players have some value associated with it (and that’s the value of the group). Here, $$2^N$$ just denotes the power set of $$\{1, \cdots, n\}$$
  * $$v(S)$$ is just the value of a coalition $$S$$, and by default, $$v(\phi) = 0$$
* Coalition structure (CS) → a partition of $$N$$ (into groups) — each person is in exactly one coalition.
* We aim to find the optimal coalition structure — the structure that maximizes total generated value from the $$n$$ players. That is, $$OPT(G) = \max_{CS} \sum_{S \in CS} v(S)$$
* But we also need to satisfy some (reasonable if you think about it) constraints:
  * **Efficiency**: a vector $$\vec x \in \mathbb{R}^n_{\geq 0}$$ satisfying $$\sum_i x_i = v(N)$$
    * Basically this says that each person gets 1 number, which is their payoff, and the total value that the group generates must be completely distributed among the $$n$$ people.
    * It’s called efficiency because there’s no loss in value. Whatever value is generated is distribued without any loss. Nothing is kept for the mechanism or wasted. It’s a matter of economic efficiency — no one can improve without anyone else being hurt — and has nothing to do with computational efficiency.
  * **Individual Rationality**: $$x_i \geq v_i$$ for all $$i \in N$$
    * You need to give each person at least how much value they would generate by being alone — if not, they will leave your group (”deviate”), and do their own thing.
* An **imputation** is just a vector that satisfies efficiency and individual rationality.

### Cooperative Game Properties

A game $$G = <N, v>$$ is called:

* **Monotone**: for any $$S \subseteq T \subseteq N$$, $$v(S) \leq v(T)$$
  * Adding anyone cannot decrease the value of the coalition.
* **Simple**: if $$G$$ is monotone AND $$v(S) \in \{0, 1\}$$
  * i.e., the value of any coalition is binary, winning or losing, and adding anyone to a winning coalition cannot make the coalition lose (there’s no snitch :O)
* **Superadditive**: for disjoint $$S, T \subseteq N$$, $$v(S) + v(T) \leq v(S \cup T)$$
  * collaboration is always beneficial to the overall value generated
* **Convex**: for $$S \subseteq T \subseteq N$$ AND $$i \in N \setminus T$$, we have $$v(S \cup \{i\}) - v(S) \leq v(T \cup \{i\}) - V(T)$$
  * the marginal gain of player $$i$$ by adding to $$S$$ cannot be more than the marginal gain if player $$i$$ joins $$T$$, and $$T$$ contains at least everyone in $$S$$ (and possibly more) — because the opportunities to collaborate is more. (joining a bigger group is always better, given that one is a subset of the other)

Monotone $$\land$$ Convex $$\implies$$Superadditive.

## Dividing Payoffs in Cooperative Games

### The Core

An imputation $$\vec x$$ is in the core if:

$$
\sum_{i \in S} x_i = x(S) \leq v(S), \forall S \subseteq N
$$

That is, each susbet of players is getting at least what it can make on its own.

It’s easier to think about what would happen if we did not satisfy the above criteria. Then, a subset of players are together being paid less than the value they bring to the table. So, they would deviate from the group (making it unstable) and divide the profits among themselves in a way that they at least get as much as they were getting before, with some people getting more now.

So, being in the core is a notion of stability: no subset of players have any incentive to deviate.

So, the core is a set of vectors that satisfies the linear constraints:

* $$\sum_{i \in N} x_i = v(N)$$ — the total payoff given to everyone is equal to the total value they generate.
* $$\sum_{i \in S} x_i \geq v(S), \forall S \subseteq N$$ (by definition of being in the core)

Also, remember:

* **x**$$(S)$$ is additive in the sense that the total payoff of a group is the sum of the individual payoffs given to each person (think of it as cash).
* BUT $$v(S)$$ is not additive — otherwise there would be no point in collaborating and this would be a boring problem. They can generate more / less value as a group than individually (think of synergy, 1 + 1 > 2)

<figure><img src=".gitbook/assets/Screenshot_2024 09 12_at_6.09.42_PM.png" alt=""><figcaption></figcaption></figure>

For a 3-player game, we can find the core by drawing a triangle, and then each point on the triangle maps to a payoff vector. e.g. the top vertex means that all the payoff goes to the first player. The midpoint of the bottom edge means that the payoff vector is $$\vec x = (0, \frac{v(N)}{2}, \frac{v(N)}{2})$$. In general, each person gets paid in the inverse of the ratio of the distance from the point to the edge opposite that player’s vertex.

### Is the Core Empty?

A natural question to ask then is: is the core empty?? That is, is there ANY possible payoff vector that ensures that no subset of players deviate?

**Simple Game**: A game is called simple if $$v(S) \in \{0, 1\}$$ for all $$S \subseteq N$$ and it is monotone (adding players to a coalition does not decrease the value).

Coalitions with value 1 are winning. Coalitions with value 0 are losing.

A player is called a a veto player if she is a member of every winning coalition (can’t win without her).

💡 **Theorem: Let** $$G$$ **be a simple game then** $$Core(G) \neq \phi \iff G$$ **has veto players. In fact, the core has precisely the vectors that distribute the payoff ONLY among the veto players.**

The proof is relatively simple: if we give any positive payoff to a non-veto player, then the remaining players can deviate together (and they can divide this positive payoff among themselves).

If we give positive payoff ONLY to veto players, then since any winning coaliion must contain ALL veto players, there is no benefiical deviation. BUT notice that this doesn’t mean that we have to give every veto player the same payoff, or even that we have to give every veto player a positive payoff(!!). So, ANY distribution of the payoff among only the veto players is in the core (no subset of players can deviate).

💡 **Theorem: For an induced subgraph game, the core is not empty** $$\iff$$ **the graph has no negative cut.**

In particular, if the core is not empty, a possible payoff strategy is the _shapley value_ which assigns each node half the value of the edges connected to it.

Let’s prove the theorem.

* ($$\impliedby$$) If there is no negative cut, then the core is not empty.
  * It suffices to find 1 payoff vector in the core.
  * Let $$\phi_i = \frac 1 2 \sum_{j=1}^n w(i,j)$$ be the payoff assinged to for each player $$i$$
  * Then, $$\sum_i \phi_i =$$ sum of the weights of all edges $$= v(N)$$. Hence, satisfies efficiency.
  *   Now, for any subset $$S \subseteq N$$, we have:

      $$
      \phi(S) = \sum_{i \in S} \phi_i
      $$
  * Notice that each player’s payoff ONLY depends on the edges they are connected to, NOT in which coalition they belong. So, each player will receive the same payoff regardless of which coalition he is part of. And the payoff of the group $$S$$ is just the sum of the payoffs of everyone in the group.
  *   Then, we can divide the edges in the sum above to be “within $$S$$” and “across $$S$$” — because the edges within $$S$$ contribute to $$v(S)$$, and the ones across $$S$$ form the cut! (This is precisely why we have this seemingly surprising theorem - how does a cut relate to this!)

      $$
      \phi(S) = \sum_{i \in S} \sum_{j \in S} \frac 1 2 w(i, j) + \sum_{i \in S} \sum_{j \in N \setminus S} \frac 1 2 w(i, j) =v(S) + \frac 1 2 Cut(S, N \setminus S)
      $$
  * And since there are no negative cuts, the last expression shows that $$\phi(S) \geq v(S)$$.
  * Efficiency is also obvious from this expression, because we can just set $$S=N$$.
* ($$\implies$$) If the core is not empty, the graph has no negative cut.
  * Instead, we prove (the contrapositive) that if the graph has a negative cut, then the core cannot be empty.
  * Suppose there is some negative cut, i.e., $$S \subseteq N$$ such that $$\sum_{i \in S} \sum_{j \in N \setminus S} w(i, j) < 0$$.
  *   Take any payoff vector $$\vec x$$ satisfying efficiency; then we have:

      $$
      x(S) + x(N \setminus S) = \sum_{i \in N} x_i = v(N)
      $$

      must be true by efficiency.
  * We have just shown in the previous part that $$v(N) = \phi(N)$$, and since $$\phi$$ is additive, $$\phi(N) = \phi(S) + \phi(N \setminus S)$$ where $$\phi_i = \sum_{j \in N} \frac 1 2 w(i, j)$$.
  * Hence, $$x(S) + x(N \setminus S) = \phi(S) + \phi(N \setminus S)$$ — which we will use later.
  *   Now consider $$x(S) - v(S)$$ and $$x(N \setminus S) - v(N \setminus S)$$. Our goal is to show that at least one of them must be negative. To do that, we take their sum and show that it is negative — then we can conclude that at least one of the terms is negative and hence cannot be in the core (i.e., a subset is not paid for the amount of value they generate).

      $$
      (x(S) - v(S)) + (x(N \setminus S) + v(N \setminus S)) = (\phi(S) - v(S)) + (\phi(N \setminus S) - v(N \setminus S))
      $$
  *   We have already shown that $$\phi(S) = v(S) + \frac 1 2 Cut(S, N \setminus S)$$, and so the RHS evaluates to:

      $$
      \frac 1 2 Cut(S, N \setminus S) + \frac 1 2 Cut(N \setminus S, S) = Cut(S, N \setminus S) < 0
      $$
  * Hence, it is either the case that $$x(S) < v(S)$$ OR $$x(N \setminus S) < v(N \setminus S)$$; hence, $$x$$ cannot be in the core.
