# Fair Allocation of Indivisible Goods

Suppose we have $$N$$ players, and $$G = \{g_1, \cdots, g_m \}$$ indivisible goods (i.e., each $$g_i$$ cannot be further divided into any pieces).

Each player $$i$$ has a value $$v_i(g)$$ for good $$g$$.

Unless noted otherwise, assume that valuations are **additive** (even in the case of additive valuations, this problem is complex enough to warrant an interesting discussion, unlike in the case of auctions). This means that $$v_i(G') = \sum_{g \in G'} v_i(g)$$ for all $$G' \subseteq G$$.

The advantage of additive valuation is ease of elicitation: now we only need $$m$$ (instead of $$2^m$$) values to describe an agent’s value for any susbet of goods. The disadvantage is that it does not allow for complements (two things that go really well together have more value when they are gotten together, e.g. pen and diary) / susbtitutes (two things that are basically the “same” and you only want one of them e.g. pen and pencil) — basically. it imposes a constraint on the valuation function that might not hold in reality.

An **allocation** is a partition of goods, $$A = (A_1, \cdots, A_n)$$ where bundle $$A_i$$ is allocated to player $$i$$.

Note: Each player has their own valuations for the goods, so it makes no sense to compare the value of $$v_1(A_1)$$ and $$v_2(A_2)$$ because it’s possible that player 1 values everything 10x more than player 2, but that doesn’t say anything. Always make sure that you’re comparing things that use the same valuation function, i.e., $$v_1(A_1)$$ and $$v_1(A_2)$$ is comparable because it tells us whether player 1 would prefer $$A_1$$ over $$A_2$$ or not.

## Fairness Notions

We’re trying to quantify what it means for an allocation to be “fair” — i.e., we’re trying to describe desirable properties that our algorithm should achieve.

* **Proportionality**: $$v_i(A_i) \geq \frac{1}{n}v_i(G)$$ for all $$i \in N$$. That is, everyone should think they’re getting at least their proportional share. e.g. if the total value (according to me) is 10, and there are 2 players, i would like to get a value of at least 5.
* **Envy-freeness**: $$v_i(A_i) \geq v_i(A_j)$$ for all $$i, j \in N$$. That is, no envy should envy any other agent. No one should think that they would rather have gotten another bundle. Each person thinks they’re already getting the best (non-strictly) possible deal among everyone else.

For $$N=2$$, both proportionality and envy-freeness are equivalent. Because getting at least half the share is the same as saying that you’re getting at least as much as the other person.

For $$N \geq 3$$, envy-freeness is a strictly stronger condition than proportionality. That is, envy-freeness impies proportionality but proportionality does not imply envy-freeness. A short proof of this is as follows:

*   If $$v_i(A_i) \geq v_i(A_j)$$ for all $$i, j \in N$$, then we can sum the equations holding $$i$$ fixed, and varying $$j$$ to get:

    $$
    n \cdot v_i (A_i ) \geq v_i(A_1) + \cdots + v_i(A_N) = v_i(G)
    $$

    and so, $$v_i(A_i) \geq \frac 1 n v_i(G)$$, which means it satisfies proportionality.
* Proportionality $$\;\not\!\!\!\implies$$Envy-freeness. Example: assume $$m=n$$. Player 1 has value 1 for every good, while other players have value 0 for all goods. $$A_1 = \{g_1\}, A_2 = \{g_2, \cdots, g_m\}$$ and $$A_3 , \dots, A_n$$ are empty. Then, this satisfies proportionality but player 1 would rather get $$A_2$$ instead of $$A_1$$

Intuitively, envy-freeness not only cares about what you got, but what everyone else got too. Proportionality only cares about what you got (relative to the group as a whole).

Envy-freeness or even proportionality cannot always be satisfied. A very simple example is when we only have 1 good, and 2 players (each having positive value for this good). Clearly, no allocation will be envy-free or proportional.

So, we have to relax / weaken our constraints and try some other notions of fairness.

* Maximizing Utilitarian Welfare (i.e., the sum of the player’s utilities) — we simply give each good to the player who has the highest value for it (breaking ties arbitrarily) This works because each player’s valuations aree additive. But this does not feel ‘fair” e.g. if player 1 has a value of 9 for all goods, and player 2 has a value of 10 for all goods, this mechanism will allocate all goods to player 2, and player 1 will be left with nothing… yeah, doesn’t feel fair.
* **Envy-freeness up to one good (EF1)**: It’s a relaxation of envy-freeness so that each player may envy another player but only up to a single item, i.e., player $$i$$ may envy player $$j$$, but the envy can be eliminated by removing a good from $$j$$’s bundle. Formally, for any $$i,j \in N$$, if $$A_j \neq \phi$$, then there exists a $$g \in A_j$$ such that $$v_i(A_i) \geq v_i(A_j \setminus \{g\})$$. Notice that this $$g$$ can be different for different players (even for a fixed $$j$$)!

## Round Robin Algorithm

The good news is that we can satisfy EF1 by the round-robin algorithm. Let the players take tunrs choosing their favorite good from the remaining goods, in the order $$1, 2, \cdots, n, 1, 2, \cdots, n , 1, 2, \cdots$$ until the goods run out.

This works for any number of agents, and for any additive valuation functions.

The proof is actually really simple:

* If $$i$$ is ahead of $$j$$ in the round robin ordering, then in every round, $$i$$ does not envy $$j$$ (because $$i$$ would pick the good that she values more, because $$i$$ had the first choice) — envy-freeness.
* If $$i$$ is behind $$j$$ in the ordering, then consider the first round to start with $$i$$’s first pick (i.e., for now, ignore $$j$$’s first pick). Then, we can pretend as if $$i$$ is ahead of $$j$$ in all these rounds (if we’re ignoring $$j$$’s first pick, then we act as if $$i$$ picked frist). So, $$i$$ does not envy $$j$$ up to $$j$$’s first good. It’s possible that $$i$$ envies $$j$$ but that’s only because of $$j$$’s first good.&#x20;

<figure><img src=".gitbook/assets/image 1.png" alt=""><figcaption></figcaption></figure>

As a bonus, the resuting allocation is balanced: that is, the difference in the _number_ of goods is at most 1, between any pair of agents.

## Envy-Cycle Elimination Algorithm

Note: this mechanism works for general monotonic (not necessarily additive!) valuations: $$v_i(S) \leq v_i(T)$$ for any $$S \subseteq T \subseteq G$$.

We can still get an EF1 allocation (even for a general monotonic valuation function) using this envy-cycle elimination algorithm:

1. Allocate one good at a time in an arbitrary order.
2. Maintain an envy graph with the players as its vertices, and a directed edge $$i \to j$$ if $$i$$ envies $$j$$ with respect to the current partial allocation.
3. At each step, the enxt good is allocated to a player with no incoming edge. Any cycle that arises is eliminated by giving $$j$$’s bundle to $$i$$ for every $$i \to j$$ in the cycle.

The key intuition here is this:

* If no one envies player $$i$$, then giving 1 good to player $$i$$ can only make players envy $$i$$ up to at most 1 good in the worst case.
* When we have a cycle, it’s an indication that we can make everyone happier by rotating the bundles along the cycle — since it makes every player strictly better off. So, the total utility strictly increases. (Hence, we can’t have infinite cycles, because the total utility of allocating finitely many goods must be finite.)

We prove the correctness of this algorithm (and the fact that it terminates) with the following 3 claims:

1. Claim 1: At each step, the procedure of eliminating cycles must end.
   * Each time we eliminate a cycle, we do so by cycling the bundles around so that the new allocation has strictly higher utility. So, the utilitaria welfare strictly increases, because everyone on the cycle gets strictly higher utility.
   * So, it cannot happen infinitely since the allocation has finite goods and must generate finite utility.
2. Claim 2: When the procedure of eliminating cycles ends, there is an unenvied player (i.e., a “source” in the envy-graph)
   * Proof by contradiction: Suppose not. Then, player a is envied by b, b is envied by c, and so on.. But we only have finitely many players. So, we end up with a cycle in the envy graph, which contradicts the fact that we removed all the cycles.
3. Claim 3: At each step, the partial allocation is EF1.
   * We only allocate a good to an unenvied player, so any envy towards that player is by at most one good (i.e., the newly allocated good)

Claims 1 and 2 are enough to prove the termination of the algorithm, while claim 3 is required to prove the correctness (since it states that at every step, the allocation is EF1, and so, even at the end, the allocation is EF1).

Notice that we don’t use the “additive”-ness at all! But we do use monotonicity (i.e., when we add a good to a player’s bundle, her valuation cannot decrease — she cannot start to envy others as a result of this allocation!)

## Maximum Nash Welfare (MNW)

MNW requires additivity of the valuation functions.

The Nash Welfare of an allocation is the product of the players’ utilities: $$\prod_{i=1}^n v_i(A_i)$$.

{% hint style="warning" %}
**Theorem: An allocation that maximizes the Nash Welfare, called a Maximum Nash Welfare (MNW) allocation, is EF1.**
{% endhint %}

It’s a bit surprising that nash welfare (and not egalitarian welfare) leads to EF1 allocations.

Corner case: If MNW = 0, maximize the number of pllayers with positive utility first, and then maximize the Nash welfare among these players. This is the way to guarantee that the allocation satisifes EF1.

Proof sketch:

* Suppose for contradiction that playuer $$i$$ envies player $$j$$ even after removing any good from $$j$$’s bundle.
* Consider a good $$g$$ in $$j$$’s bundle that minimizes the ratio $$v_j(g)/v_i(g)$$ — that is, pick the good $$g$$ such that $$j$$ does not value it much, but $$i$$ does.
* Then, moving $$g$$ to $$i$$’s bundle can be shown to increase the Nash welfare.

Bonus: The resulting allocation is always **Pareto optimal**: we cannot make some player better off without making another player worse off. This is almost by definition because if we could, then we would do so and the Nash welfare (product of everyone’s utilities) will strictly increase (and the original allocation wouldn’t be “maximum” Nash welfare).

***

Quick Summary:

* All of Round-Robin, Envy-ycle Elimination, and MNW guarantee EF1
* Round-Robin ensures that the allocation is balanced.
* Envy-cycle does not require that the valuations are additive, only monotonic.
* MNW guarantees Pareto-optimality too.

So, each of them have their distinct advantage and hence, use-case.

## EFX

Recall that EF1 means “if a particular good $$g$$ is removed from your bundle, I will no longer envy you”.

Envy-freeness up to any good (EFX) states: For any $$i, j \in N$$ and ANY $$g \in A_j$$, we have $$v_i(A_i) \geq v_i(A_j \setminus \{g\})$$. That is, “if ANY good is removed from your bundle, I will no longer envy you.”

Clearly, EFX is a stronger constraint than EF1.

In general, we have: Envy-freeness $$\implies$$ EFX $$\implies$$ EF1.

**The output of round robin, envy-cycle elimination, and MNW may NOT satisfy EFX.**

Wait.. does there always exist an EFX allocation in the first place??

For $$n = 2$$ and $$n = 3$$, yes, an EFX allocation always exists. The proof for $$n=2$$ is simple, while proof for $$n=3$$ is much more complicated.

Proof for $$n=2$$: Cut-and-choose algorithm

* The first player divides the goods into 2 bundles that are equal as possible in her view.
* Then the second player chooses the bundle tha the prefers.
* The first player will be EFX, and the second player will be envy-free.
  * The proof that the first player will be EFX relies on the fact that she divided the goods into 2 “as equal as possible” bundles.
  * Let $$d=v_1(A_2)-v_1(A_1) > 0$$.
  * Every object $$g$$ in $$A_2$$ must have $$v_1(g) \geq d$$ because otherwise player 1 would’ve moved $$g$$to the other bundle to make the bundles “more equal”.
  * So, removing any object $$g$$ from $$A_2$$ will result in $$v_1(A_2 \setminus \{g\}) \leq v_1(A_2) - d = v_1(A_1)$$, and hence player 1 is EFX.

**For n** $$\geq 4$$**, this question is open! (there is neither an existence proof nor an impossibility result yet).**

## Maximin Share (MMS)

Until now, we had been relaxing the constraints of envy-freeness, and we saw that it’s always possible to obtain an EF1 allocation (and we saw multilpe different ways to achieve this, with additional properties of each algorithm).

Now, we hope to relax the proportionality condition (and one such relaxation yields the maximin share value).

A player’s maximin share (MMS) can be computed by performing the following thought experiment: The player divides the goods into $$n$$ bundles so as to maximize the value of the minimum-value bundle, and the value of the minimum bundle in such a partition is called the player’s MMS.

So, MMS means that each player thinks they’re getting _at least_ the worst bundle in a “fair” allocation. Getting less than MMS would make the player doubt whether the allocation was fair (because in her head, she’s getting even less than the worst possible outcome assuming the goods are split so that everyone gets nearly equal value).

Note that maximin share is a relaxation of proportionality, i.e., $$MMS_i \leq \frac 1 n v_i(G)$$. The proof by contradiction is quite simple: suppose not, then each bundle in this MMS allocation has value strictly greater than $$\frac 1 n v_i(G)$$, and so the sum of the valuations (using additivity of player $$i$$’s valuations) of each bundle gives: (total value) $$> n \cdot MMS_i > v_i(G)$$, which is not possible, since player i’s total value of $$G$$ should be the same, no matter the allocation. Basically, the lowest bundle’s value cannot be strictly greater than the arithmetic mean (which would be obtained when player i can divide all goods exactly equally).

An allocation that gives every player his / her maximin share always exists when there are 2 people, but NOT when there are at least 3 players (known impossibility proof). This shows how hard it is to satisfy MMS (let alone proportionality).

**However, for any number of players, we can always give every player more than** $$\frac 3 4$$ **of his / her maximin share.**

The proof that an allocation that gives every player at least his / her maximin share always exists when there are 2 players uses the cut-and-choose algorithm again:

* Alice divides the goods into 2 parts that are of as equal value as possible in her view.
* Either part yields value at least Alice’s MMS to her (by definition)
* Bob chooses a part that he prefers
* Bob is envy-free (and therefore, proportional, and therefore he gets his MMS). Recall that for 2 players, envy-freeness $$\equiv$$ proportionality and also, in general, proportionality $$\implies$$MMS.

(There’s also such a thing as minimax share — read up about it if you’re interested!)

## Query Complexity

It’s a way of comapring how complex different algorithms are. Sort of like the idea of time complexity.

With each query, the algorithm can find out a certain player’s value for a certain set of goods.

Then, the question is: how many tiems do we need to query the players? (How many times do we have to ask “oh hey Bob umm how much do you value this set of goods?”)

This becomes especially relevant when valuations are not additive — because in the worst case, we might have to make $$n2^m$$ queries.

BUT, thankfully, we don’t!

The envy-cycle elimination algorithm can be implemented using $$O(nm)$$ queries, even with monotonic (non-additive) valuations:

* It suffices to query the value of each agent for the $$n$$ bundles in each partial allocation to construct the envy graph.
* Since there are $$m$$ partial allocations, the number of queries is $$O(nm)$$

In each iteration, we add an item to an existing bundle — all other bundles remain unchanged. So, we only need to query the players’ valuations for this new bundle. And even during the cycle-elimination phase, the bundles just move around, they don’t actually change. So, the valuations each player has for the bundles don’t change, and we don’t have to re-query them. (We assume that we store the query results each time we get them).

In fact, if we have just 2 agents with monotonic valuations, $$O(\log m)$$ queries suffice (even though the total possible allocations is $$2^m$$ wow!). Proof:

* Arrange the goods on a line, and implement cut-and-choose
* The first agent can find an EF1 cut using binary search

The same technique does NOT work for EFX (even though an EFX for $$n=2$$ does exist, it cannot be found this way), as there may not exist an EFX partition on this specific arrangement of items on the line. e.g. if we have 3 goods, 2 players with identical and additive valuations 1, 2, 1 → there’s no way to make it EFX if we arrange them in the order 1, 2, 1 (we’d have to do 1 1 2 or 2 1 1).

In fact, any deterministic EF1 algorithm needs $$\Omega (\log m)$$ queries. That is, this $$\log m$$ bound is tight, and hence, we can write it as $$\Theta(\log m)$$ .

Proof:

* We will prove that _even_ for identical additive valuations where each player has value 1 for 2 goods, and 0 for the rest, we need $$\Omega(\log m)$$ queries. (And here, the mechanism _knows_ that both players have such valuations — 1 for 2 fixed goods (e.g. gold coins), and 0 for the rest (e.g. stones)) And hence, we need at least these many queries for a more general case, since in general, the mechanism doesn’t know the player’s valuations apriori without querying!
* Notice that in ANY EF1 allocation, the two valuable goods (with valuation = 1 each) must be separated. So, the goal of the mechanism is to create a partition such that each bundle contains 1 of the valuable good.
* We can show that such a mechanism would require $$\Omega(\log m)$$ queries by making use of an adversary argument → assume an adversary whose job is to respond to the queries so that the mechanism takes the maximum number of querie (i.e., the adversary makes life difficult for the mechanism).
* In particular, as the adversary that generates that testcase, we don’t have to decide apriori what the valuable goods are, as long as our answers to the queries are consistent to what we finally declare the valuable goods to be! The mechanism only sees the query results, and only finds out the valuations at the very end (to make sure we weren’t cheating)
* Let $$G'=G$$ initially (the mechanism needs to find a way to split the two valuable goods, so we wil play the role of the adversary and track which set contains both the valuable goods in our head).
* Suppose the algorithm queries $$v(H)$$.
* If $$|G' \cap H| \geq \frac{|G'|}{2}$$, then we answer 2 and replace $$G'$$ by $$G' \cap H$$. That is, okay, we told the mechanism that both valuable goods are in $$H$$, and previously we had told the mechanism that they are in $$G'$$ → hence, to be consistent, they must be in $$G' \cap H$$ now — but we only do this when the set containing both is bigger than the set not containing both, and that’s exactly what we want → we want to force the set containing both the valuable goods to be as large as possible.
* Otherwise, we (the adversary) answer 0 and replace $$G'$$ by $$G' \setminus H$$. (obviously we never want to answer 1 if we can avoid it, otherwise the mechanism can simply output $$H,G \setminus H$$ as the two partitions and its done)
* Since $$|G'| =m$$ at the beginning, and decreases by a factor of at most 2 after each query, at least $$\log_2 m$$ queries are needed to “split” the two valuable goods, in this worst case.
* Hence, any deterministic EF1 algorithm needs $$\Omega(\log m)$$ queries.

{% hint style="info" %}
Any deterministic EFX algorithm needs a number of queries **exponential** in $$m$$ (not just linear, but exponential! Much much harder than EF1)
{% endhint %}
