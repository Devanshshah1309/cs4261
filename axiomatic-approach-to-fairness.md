# Axiomatic Approach to Fairness

We’re trying to define what is “fair” by coming up with some reasonable and desire properties we would like this definition to have.

Fairness considerations include the discussion on Shapley value, Nash bargaining solution, and the fair allocation of goods.

That is, we’re trying to answer the question: given a cooperative game $$v : 2^N \to R_{+}$$, how should the revenue $$v(N)$$ be divided **fairly**?

## Properties of a Fair Approach

These are properties that we would like our method to have. Note that these aren’t some random mathemtical formulae. They reflect our goals — we’re trying to encode our beliefs of what an ideal-world algorithm should look like, in the form of mathematical equations.

1. Efficiency → $$\sum_{i \in N} \phi_i = v(N)$$. That is, there is no loss in the value generated and what is paid out to the people. Everything is distributed. Again, this is a notion of economic efficiency — we can’t make someone better off without making someone else worse off.
2.  Symmetry → Symmetric players are paid equally. Two players $$i$$ and $$j$$ are said to be symmetric iff when added to any set of players (excluding themselves), they contribute the same marginal value. That is, $$\forall S \subseteq N \setminus \, \{i, j\}, \ v(S \cup \{i\}) = v(S \cup \{j\})$$.

    This property is also called _equal treatment of equals_.
3. Dummy players aren’t paid. A player is said to be “dummy” if he does not contribute any value to any set of players. That is, $$\forall S \subseteq N \setminus \{i\}, \ v(S \cup \{i\}) = v(S)$$
4. Our “payoff division” function $$\phi$$, must be linear. That is,
   1. $$\phi_i(G_1) + \phi(G_2) = \phi(G_1 + G_2)$$. Here, $$G_1$$ and $$G_2$$ are two games, and “adding” the two games simply means that we’re considering the combined game now. And then, the payoff that each player gets in $$G_1$$ and $$G_2$$ should add up to the same if we combined both the games into one.
   2. $$\phi_i(a \cdot G_1) = a \cdot \phi_i(G_1)$$. Scaling a game by a factor of $$a$$ should just scale each player’s payoff by $$a$$. e.g. if everyone is producing 2x more value, they should all be paid 2x more — scaling shouldn’t change the ratio in which people are being paid.

In the above discussion, we were just stating our wishes for the ideal payment function. But there is no reason to believe that such an ideal function exists in the first place (and it’s not at all obvious why there must be a function that satisfies all these criteria).

However, it turns out that such a payment function $$\phi$$ does exist. And it’s called the…

## The Shapley Value

Given each payer $$i$$ and a set $$S \subseteq N$$, the **marginal contribution** of $$i$$ to $$S$$ is given by:

$$
m_i(S) = v(S \cup \{i\}) - v(S)
$$

That is, how much does $$i$$ contribute by joining $$S$$ ?

It seems natural that each player should be paid their marginal contribution to the group, right? It quite literally is the value they’re bringing to the table.

But the issue is: which group do we consider? By joining different groups, you might contribute different values — so what should you be paid?

One possible idea is to pay the average value you would contribute (where the average is taken across all possible sets $$S \subseteq N \setminus \{i\}$$). If we did this, it would satisfy ALL the desired properties except efficiency, i.e., the total value we pay everyone is exactly equal to the total value generated as a group. (So, we have the right idea, but the “scaling factor” of $$2^{n-1}$$ is not correct because the total value of a set $$S$$ is NOT equal to the $$\sum_{i \in S} m_i(S)$$, but instead we’ve to consider the order in which elements are added in $$S$$, and then look at the marginal contributions of each element only considering the elements before it).

So, the Shapley value is _almost_ that, except that it takes the average over all permutations of preceding people who are in the group before $$i$$. The reason we need to do this is that the probability of joining a set of players should depend on the size of the set — think of it this way: there are 6 ways for the group $$\{1, 2, 3\}$$ to be “formed” (the 6 permutations indicating who came first), but only 1 way for the group $$\{1\}$$ to be formed. So, your probability of joining the group with 3 members is more than that of joining the group with only 1 member. (We’re assuming here that the groups are formed in a way that people form a queue, with random positions, and then each person joins the group formed by everyone before them.)

Formally, given a permutation $$\sigma \in \Pi(N)$$ of players, let the predecessors of $$i$$ in $$\sigma$$ be:

$$
P_i(\sigma) = \{j \in N | \sigma(j) < \sigma(i) \}
$$

Basically, the set of players who are before $$i$$ in the permutation $$\sigma$$ (visualize it as people standing in a line — then everyone to the right of $$i$$ is considered to be “predecessors of $$i$$”).

We write $$m_i(\sigma) = m_i(P_i(\sigma))$$ → how much does $$i$$ contribute (marginally) if $$i$$ is added to the set of her predecessors in $$\sigma$$.

Suppose that we choose an ordering of the players uniformly at random. Then, the shapley value of player $$i$$ is:

$$
Sh_i = E[m_i(\sigma)] = \frac{1}{n!} \sum_{\sigma \in \Pi(N)} m_i(\sigma)
$$

💡 **The Shapley Value satisfies all of the 4 properties above: efficiency, symmetry, dummy, and linearity.**

In fact, (and this is a much stronger claim), the Shapley value is THE ONLY value satisfying efficiency, linearity, dummy and symmetry! That is, these 4 properties are a characterization of the Shapley value (so, if you want all of these 4 properties, you literally have no choice but to use the Shapley value)

e.g. Given a weighted voting game where $$w_1 = 1, w_2 = 1, w_3 = 3, w_4 = 4$$ and the threshold is $$q=5$$. Compute the shapley values of all players.

In a WVG (weighted voting game), there is only 1 pivotal player in any permutation — the player that “pushes” the coalition from losing to winning. All players before that player were part of a losing coalition and they didn’t do anything about it, and all players after that player were already part of a winning coalition so they didn’t have to do anything.

Here, observe that player 4 is only pivotal if he is second or third — and this happens with probability 0.5, so his shapley value is $$\frac 1 2$$ (i.e, in half the permutations, he is pivotal and in those cases, his marginal contribution is 1, and in the rest, he contributes nothing).

Player 1 is pivotal in only 4 out of 4! permutations (when he is preceded by players {2,3} or {4}) and so, his shapley value is $$\frac 1 6$$.

By symmetry, player 2 also gets $$\frac 1 6$$.

By efficiency, player 3 gets $$1 - (\frac 1 6 + \frac 1 6 + \frac 1 2)=\frac 1 6$$.

Note that $$Sh_1 = Sh_2 = Sh_3$$ even though $$w_3 = 3 > w_1 = w_2$$. So, having a higher weight does not necessarily result in higher payoff.

### Shapley Value in Induced Subgraph Games

💡 **Theorem: In an induced subgraph game,** $$\phi = \frac 1 2 \sum_{j \in N} w(i, j)$$ **— that is each player gets half of the “synergy” value in each relationship (marginally) with each of the other players.**

This theorem makes it much easier to calculate the shapley value since we don’t have to look at the $$n!$$ permutations — we can just look at the graph!

Let’s prove the theorem.

For each player $$i$$, her marginal contribution to a set $$S \subseteq N \setminus \{i\}$$ equals $$\sum_{j \in S} w(i, j)$$. This is true by definition of induced subgraph games (and the definition of marginal contribution).

For every player $$j \neq i$$, let $$I(j \in P_i(\sigma))$$ be an indicator random variable that equals 1 if $$j$$ appears before $$i$$ in $$\sigma$$, and 0 otherwise.

Then, the shapley vlaue is:

$$
SH_i =E_\sigma[v(P_i(\sigma) \cup \{i\}) - v(P_i(\sigma))] = E_\sigma \left[\sum_{j \in P_i(\sigma)} w(i, j) \right]
$$

And we can write it as

$$
= E_\sigma\left[\sum_{j \in N \setminus \{i\}} I(j \in P_i(\sigma)) \cdot w(i, j)\right]
$$

Notice this is kind of a double sum — for each permutation, we’re looking at the nodes and checking whether they are a predecessor of $$i$$ or not (and then deciding whether to contribute $$w(i,j)$$ or not).

```python
for sigma in permutations
	for j in N \ {i}
		...
```

```python
for j in N \ {i}
	for sigma in permutations
		...
```

What if… we change the order of the “loop”, and think of it as: for each node, look at all the permutations and check whether the node is a predecessor of $$i$$ or not.

The reason we prefer this is because it’s much easier to find the probability of any node being a predecessor of $$i$$ or not, in a random permutation — it’s just $$\frac 1 2$$.. Because in any random permtuation, $$i$$ is equally likely to be before $$j$$ or vice versa, and so the probability that a node $$j$$ is a predecessor of $$i$$ is 0.5.

Okay, let’s get back to the math now.

We have (by linearity of expectation):

$$
= \sum_{j \in N \setminus \{i\}} E_\sigma[I(j \in P_i(\sigma)) \cdot w(i, j)]
$$

and since $$w(i,j)$$ is a constant that does not depend on $$\sigma$$, we can pull it out (again by linearity):

$$
= \sum_{j \in N \setminus \{i\}} w(i, j) \cdot E_\sigma[I(j \in P_i(\sigma))]
$$

And, as we talked about earlier, by symmetry, it’s equally likely for $$i$$ to be before $$j$$ or $$j$$ to be before $$i$$. So, in half the permutations $$\sigma$$, $$I(j \in P_i(\sigma))$$ will be 1, and in the remaining half, it will be 0 — yielding an average value of $$\frac 1 2$$ (an easier way to think of this is that the expectation of any random variable is just its probability of it being 1).

So, we get:

$$
= \sum_{j \in N \setminus \{i\}} w(i, j) \cdot \frac 1 2 = \frac 1 2 \sum_{j \in N \setminus \{i\}} w(i, j)
$$

which is exactly what we claimed.

💡 Note that the “core” and “shapley value” measure different things. So, a payoff vector in the core might not be the shapley value, and the shapley value might not even be in the core. An example is to consider the following game:

* $$v(1) = v(2) = v(3)= 0$$
* $$v(1,2)=0, v(1,3)=1, v(2,3)=1$$
* $$v(1,2,3)=1$$

Clearly, since player 3 is a veto player (you cannot win without her), the core contains only the vector (0, 0, 1).

BUT we can find the Shapley value to be $$(\frac 1 6, \frac 1 6, \frac 2 3)$$ — intuitively, we give some payoff to players 1 and 2 because even though player 3 is a veto player, she cannot win by herself. She needs at least one of the players 1 or 2 to support her to win.

Core measures whether any set of players can benefit by deviating. Shapley value measures whether everyone is being paid their average marginal contribution.
