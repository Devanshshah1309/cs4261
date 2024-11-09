# Rent Division

## Setting

We want to faily distribute rooms and rents amongst roommates.

* Roommates (players): $$N = \{1, \cdots, n \}$$
* $$r$$ : rent price for the whole apartment
* $$v_{ij}$$ ; player $$i$$’s value for room $$j$$ such that $$\sum_j v_j = r$$
  * The sum of the rents across all rooms must be equal to the rent of the apartment, otherwise you don’t think the apartment is worth it (and if your value is more, you’lll end up paying more per room, so. you should just scale everything down to reach a sum of $$r$$)

And then, siimlar to auctions, our mechanism needs to determine 2 things:

* Who gets what? In this case, a room allocation $$\sigma : N \to N$$, i.e., each player should be assigned a room.
* Who pays what? Rent division $$\vec p = p_1, \cdots, p_n$$ such that $$\sum_j p_j = r$$ where $$p_j$$ is rent for room $$j$$ (and so, the player assigned to room $$j$$ must pay $$j$$).

Note that we can’t just auction off the rooms one by one, because then players might not be truthful, and it might not lead to an envy-free allocation (and it wouldn’t even maximize utility).

Also, note that the requirement that $$\sum_j v_{ij} = r$$ means that players can’t encode views such as “I prefer room 1, but can’t afford to pay more than $$500 for it”. And it also does not take into account that some players might have more disposable income than others, i.e,. some might be willing to pay more for their favorite rooms such that$$\sum\_j v\_{ij} > r\$$ (but this is not allowed).

## Envy-Free Outcome

In this context of rent division, the definition of envy-free is basically the same, but it needs to take into account the price that each person is paying. That is, each player should think “I do not prefer your room for the price you’re being charged, to my room for the price i’m being charged.”

Another easier way to look at it is that: you would not wnat to change your position for anyone else’s position (which is what envy-free means even in english).

More concretely, if the outcome is $$<\sigma, \vec p>$$ then, for all $$i, j \in N$$, we must have: $$v_{i \sigma(i)} - p_{\sigma(i)} \geq v_{ij} - p_j$$. Note that we assume players have a quasilinear utility function (i.e., getting $$x gives you the same marginal utility no matter how much$$you currently have).

## EF Allocation

It’s natural to ask: does an EF allocation even exist??

And it turns out that the answer is YES, and can be efficiently computed. But such an EF allocation is not unique.

In fact, this algorithm is an example of a provably fair solution — this means that given the outcome, if anyone objects, you can easily prove / convince him that he’s wrong (in an economic sense, he wouldn’t be better off swapping places with anyone else).

Also, this mechanism guarantees efficiency in the economic sense (pareto optimality) — it is impossible to find another assignment of rooms to players that beenfits a roommate without making another player worse off.

The general framework for coming up with an EF allocation is a 2 step procedure:

1.  Deciding who gets what, i.e., room assignment

    Compute a socially optimal allocation of players to rooms, by using weighted-MCBM (Maximum Cardinality Bipartite Matching) ← this step that can have multiple room assignments that all yield the same social welfare.
2.  Decide who pays what, i.e,. rent division.

    Find an EF price vector using linear programming — you have a set of constraints and you want to maximize a function

Both weighted-MCBM and LP can be solved in polynomial time.

## Beyond EF

But… in some cases, even EF is not enough (pun intended). What we mean by this is that EF is not enough to for the division to feel “fair”. So, we want to capture some other notions of fairness too.

Consider the following example:

<figure><img src=".gitbook/assets/Screenshot_2024 10 28_at_6.00.55_PM.png" alt=""><figcaption></figcaption></figure>



It’s pretty clear who gets what, but what if Alice pays the entire rent of \$$3000? This is still envy-free. But… it feels like Bob and Claire are much happier than Alice in this scenario (both of them have utility = 2000, and Alice has utility = 0).

(Note that at this point, the rooms have already been allocated, and we’re only trying to play around with how much rent each person pays to make it as “even” as possible.)

So, we want the players’ utilities to be “close to each other”, while also satisfying EF (of course, we don’t want to lose the very nice EF property — we’re trying to make it even stronger). There are two similar ways we can quantify this:

* Equitability (under EF constraint): Define $$D(\vec p) = \max_{i, j \in N} (u_i (\vec p) - u_j (\vec p))$$ to be the maximum difference in the utilities of any 2 players under the rent division $$\vec p$$. Then, we’re trying to find $$\vec p$$ to minimize $$D(\vec p)$$ such that $$\vec p$$ is EF. This is sort of trying to distribute the utility as equitably as possible.
* Maximin (under EF constraint): Basically egalitarian, trying to maximize the minimum utility of any player. Formally, $$U(\vec p) = \min_i u_i (\vec p)$$ and then we’re trying to find $$\vec p$$ to maximize $$U(\vec p)$$ such that $$\vec p$$ is EF. So, the least happy person should be as happy as possible.

Luckily for us…

{% hint style="warning" %}
**There is a unique maximin price vector, and it is also equitable**
{% endhint %}

which means that we don’t have to choose between the two notions of fairness — we can have both satisfied simultaneously!

However, note that there exists equitable price vectors that are not maximin. (For $$n=2$$, they are equivalent though).

And it turns out that humans do care about equitability / maximin in addition to just envy-freeness, as demonstrated by a study conducted by [spliddit.org](http://spliddit.org)
