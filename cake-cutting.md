# Cake Cutting

Last week, we dealt with the problem of trying to fairly divide a set of indisivible goods.

Now, we’re trying to divide a heterogeneous divisible good among interested agents with different preferences.

Here, hetereogeneous means that agents have different preferences for different parts of the good. It’s useful to imagine a slice of cake (from left to right), and agents have different valuation functions throughout the cake.

The cake could represent land (some agents value some parts of the land more than others), machine processing time (some agents may want to use the machine in the morning, others in the afternoon, etc.)

The key thing to note here is that we can always divide the cake into smaller pieces — so, it’s a continuous space, rather than the discrete goods we’ve been dealing with so far.

## Setting

* The cake is represented by the interval $$[0, 1]$$
* Set of agents $$N = \{1, 2, \cdots, N\}$$
* Agents have valuation functions $$v_1, \cdots, v_n$$ over the cake.
* The valuation functions are:
  * Non-negative: There is no bad piece of cake — you would never be worse off if you’re given some additional piece of cake, no matter which part.
  * Additive: Values of disjoint pieces of cakes add up — there is no interaction effect between different pieces of the cake
  * Nonatomic: The value of any single point is 0 — e.g. we don’t allow a cherry that cannot be cut, since that would bring us back to the “indisivible” problem domain
  * Normalized: The value of the whole cake is 1, for each agent — this assumption can be made without loss of generality because we can always scale / normalize the valuation functions. We do this so that the valuations across agents are comparable.
* Allocation $$A = (A_1, \cdots, A_n)$$
* Each $$A_i$$ is a union of finitely many intervals. The allocation $$A$$ is “connected” if each $$A_i$$ is a single interval — that is, each agent only gets a single (connected) slice of cake, and not multiple disjoint slices.

### Valuation Functions

For convenience, we write $$v_i(x, y)$$ instead of $$v_i([x,y])$$ to denote agent $$i$$’s valuation for the interval $$[x,y]$$ of cake, where $$0 \leq x \leq y \leq 1$$.

The valuation function $$v_i$$ is often specified through the density function $$f_i$$ where:

* $$v_i(B) = \int_B f_i(x) dx$$ for $$B \subseteq[0,1]$$ — that is, we can integrate $$f_i$$ over the cake’s interval to get agent $$i$$’s valuation for that interval.
* Normalization: $$\int_0^1 f_i(x)dx = 1$$ — visually, this means that the area under the curve of $$f$$ in the interval $$[0,1]$$ is exactly 1.
* And then we get additivity and non-atomicity “for free” simply by the properties of integration.

## Fairness Notions

And as always, we’re interested in allocations that are “fair”, and so we need to define what that means.

We can use the same notions from last lecture with indivisible goods:

* Proportionality: $$v_i(A_i) \geq \frac 1 n$$ for all $$i \in N$$
* Envy-freeness: $$v_i(A_i) \geq v_i(A_j)$$ for all $$i, j \in N$$ → means that i would not want to have any other agent’s cake instead of my own.

For $$n=2$$, envy-freeness and proportionality are equivalent.

For $$n \geq 3$$, envy-freeness is strictly stronger than proportionality (since it is not only concerned with how much cake you got compared to the whole cake, but you’re also concerned about how the rest of the cake got allocated among the other agents, i.e., you not only care about your own piece of cake, but also everyone else’s piece of cake).

## Two Agents: Cut-and-Choose

Note that the cut-and-choose protocol is always meant only for 2 agents. So, whenever we say cut-and-choose, we’re implicitly assuming there are only 2 agents.

Agent 1 cuts the cake into two (exactly!) equal pieces, according to her opinion. And this is possible because the cake is divisible — note that previously we could not do this, and so we had to say “divide the goods into 2 sets that are as close as possible to each other in value”.

Agent 2 then chooses the piece that he prefers.

Agent 2 is clearly envy-free since he picked the piece, and agent 1 is envy-free because he cut the cake such that both pieces were exactly equal in the first place.

So, this allocation is envy-free (and hence, proportional too).

## Robertson-Webb Model

Cut-and-choose is clearly a “simple” protocol. But how do we quantify how simple it is.

Basically, we’re trying to come up with a way to quantify the complexity of an algorithm like how we did for divisible goods, by making a notion of “query complexity”.

We do nearly the same thing here, using the Robertson-Webb model.

Under this model, our algorithm can only make two types of queries:

* $$Eval_i(x, y)$$ → returns $$v_i(x, y)$$ — the value of agent $$i$$ for the interval $$[x, y]$$
* $$Cut_i(x, \alpha)$$ -. returns the leftmost point $$y$$ such that $$v_i(x, y) = \alpha$$, or states that no such point exists.

Then, for cut-and-choose, we only need to perform 2 Robertson-Webb queries:

* The cutter needs to query $$Cut_1(0, 0.5)$$ and then we get some output $$k$$
* And then the chooser queries $$Eval_2(0, k)$$ to decide whether he prefers the first piece or the second piece (which will have value $$1-$$ (the value of the first piece), so we don’t have to query again.

## Dubins-Spanier Protocol

Achieves proportionality for any number of agents. Notice that this wasn’t even possible in the case of indivisible goods.

Algorithm:

* A referee moves a knife over the cake starting from the left
* Repeat until there is only 1 agent left:
  * When the piece of cake to the left of the knife is worth $$\frac 1 n$$ to some agent, that agent shouts “stop!” and leaves the procedure with that piece of cake.
* The last agent gets the remaining cake.

Btw this algorithm is non-truthful in that players can gain more utility by misreporting — intuitively, if i know that you value only the right half of the cake (uniformly) and i value the entire cake uniformly, i can take 3/4 of the cake before yelling stop; so i get value 3/4 by shouting stop later, and you get value 1/2.

Proof of proportionality: If a player shouts “stop” and gets a piece of cake, he gets (what he thinks is) at least $$\frac 1 n$$ of the cake, so anyone who shouts stop satisfies proportionality. Now, the remaining people did not shout stop — so they think that piece of cake was worth $$\leq \frac 1 n$$ and so the remaining cake must be worth $$\geq \frac{n-1}{n}$$, to be split among $$n-1$$ people (so it’s still possible for everyone to get a proportional share). And we can use inductive logic here, till there are only 2 people, and the last person will also think that he gets $$\geq \frac 1 n$$. Basically, in every round, those who did not yell “stop” think that the remaining cake has enough for everyone else to get a proportional share — and this invariant is true in each round, until the very end.

In fact, every agent gets exactly value $$\frac 1 n$$ and the last agent gets _at least_ $$\frac 1 n$$.

How many Robertson-Webb queries do we need to implement this protocol? $$O(n^2)$$ queries. Don’t think of the algorithm in terms of moving a knife anymore — think of how you would implement using Cut and Eval queries.

Because in each round, we need to ask all the remaining agents their value for $$Cut_i(k, \frac 1 n$$) where $$k$$ was the cut-point in the previous round (in round 1, $$k=0$$). And then the algorithm gives the cake to the agent with the lowest value of $$Cut_i(k, \frac 1 n)$$ becuase he would have shouted “stop” first when the knife reached that point. And then we repeat this for $$n-1$$ rounds in the worst case, so we have the number of queries be: $$n + (n-1) + \cdots + 2 = O(n^2)$$

## Even-Paz Protocol

Achieves proportionality for any number of agents, _with fewer queries_.

Assume for simplicit that $$n$$ is a power of 2 (this is only to make the analysis simpler, but doesn’t actually affect the algorithm).

The idea is to use divide-and-conquer. Instead of Dubins-Spanier, which only reduces the number of players by 1 in each round, we hope to split the agents into 2 groups in each round — one that values the left half more, and the rest who value the right half more. More precisely:

1. Each agent marks the point that divides the cake into two halves of equal value, accorindg to his / her own opinion.
2. Let $$t$$ be mark number $$n/2$$ (median-ish) from the left.
3. Recurse on $$[0, t]$$ with the left $$n/2$$ agents and on $$[t, 1]$$ with the right $$n/2$$ agents.
4. Base case: When we’re down to one agent, that agent gets the whole cake.

It’s very similar to merge-sort / quicksort with median pivot.

The number of queries is clearly: $$Q(n) = 2Q(n/2) + O(n)$$, which yields $$Q(n) = \Theta(n \log n)$$.

In each round, each agent submits their $$Cut_i(0, 0.5)$$ values to the algorithm. And then once the algorithm determines the pivot (about which it will recurse), then it asks for each agent’s $$Eval_i$$ values, depending on whether they’re in the left-set or the right-set (we can technically achieve proportionality even if we don’t call $$Eval_i$$ for all agents, but in Even-Paz, we do this because we need to _recurse_, which means they need to know how much they value this part of the cake). So, we do $$2n$$ queries per round. And we can draw the recursion tree which has height $$\log_2 n$$, and each level has cost $$2n$$. So, the total cost is $$\Theta(n \log n)$$

Actually, we can pick any point to the be “pivot” here, not just the median point, and the algorithm will still be correct in that all players will get their proportional share. In fact, if we always pick the first (left-most) point to be the pivot, then this reduces to Dubins-Spanier algorithm, since in each round, the person with the left-most cut gets the cake! We use median to get the query complexity down to $$n \log n$$ but we could’ve also used any other (constant) fraction instead of $$\frac 1 2$$, e.g. even if we split the cake $$\frac 1 3 : \frac 2 3$$, we would still get a $$n \log n$$ query complexity.

The proof of proportionality is very similar to Dubins-Spanier — in each round, the left-set agents think that their side of the cake is worth more than the right side so they got the better end of the bargain, and vice-versa for the right-set. Each group of players is happy to be in whichever group they’re in, because they think there’s enough cake to be divided among the players in that group. And this holds true during each recursive step, so it holds true even when we reach the base-case of a single player. So, each player gets at least their proportional share.

Implementation detail: If we’re left with two agents, and we recurse on the left-half first, then the left agent gets _exactly_ their proportional share, and the right agent gets _at least_ their prroportional share.

<figure><img src=".gitbook/assets/Screenshot_2024 10 24_at_8.43.48_AM.png" alt=""><figcaption><p>Example run of Even-Paz. Notice that red and yellow get exactly what they think is a quarter of the cake, whereas blue and green get more than their quarter. Also notice that the order of yellow and green cuts swaps from round 1 to round 2 — this is possible (e.g. green might have valued the left half of the cake more than yellow, but with that removed, green’s new “midpoint” for the right half goes much further right).</p></figcaption></figure>



{% hint style="warning" %}
$$O(n \log n)$$ **is the optimal number of queries among ALL proportional protocols, **_**even if the allocation is not required to be connected**_**.**
{% endhint %}

And Even-Paz guaranteees connected pieces too, so it’s really amazing!

## Envy-Freeness

So far, we’ve discussed envy-freeness for 2 agents (using cut-and-choose) and proportionality for any number of agents (Dubins-Spanier and Even-Paz).

Now, we discuss envy-freeness for 3 agents.

Firstly, such an allocation is possible, and is achieved by the Selfridge-Conway protocol. It takes 5 cuts and 9 queries (not worth getting into _how_ it’s done).

The key point is that envy-freeness is MUCH harder than proportionality because each agent cares about everyone else’s pieces too.

Aziz and Mackenzie (2016) proposed the first bounded envy-free protocol for any number of agents, with the bound being $$n^{n^{n^{n^{n^n}}}}$$for both, the number of cuts and queries. So, it’s “bounded” but really only for theoretical interest.

The current best lower bound on the number of cuts and queries is $$\Omega(n^2)$$.

### Envy-Freeness Approximation

And so, even though envy-freeness is always possible to achieve, it might be too slow in many cases.

Luckily, we have an approximation algorithm in which any agent envies any other agent by at most $$\frac 1 3$$. Note that this envy can be considered really huge if we have like $$n=100$$, and each person is supposed to get only $$\frac 1 {100}$$ (and so the relative envy is a lot).

Algorithm (variant of Dubins-Spanier):

1. The referee moves a knife over the cake starting from the left.
2. When the piece of cake to the left of the knife is worth $$\frac 1 3$$ to some agent, that agent shouts “stop”, and leaves the procedure with that piece. (Note that this remains $$\frac 1 3$$ _no matter_ how many agents we have!)
3. Suppose the knife reaches the right end of the cake, but some cake is still unallocated:
   1. Case 1: There are still agents left. The remaining cake is given to one of them arbitrarily.
   2. Case 2: There is no agent left. The remaining cake is given to the agent who recevied the last piece (to ensure connectivity).

Proof that any agent envies another by at most $$\frac 1 3$$:

* Consider an agent that shouts stop, and gets a piece at any step. That agent gets value $$\frac 1 3$$. So, the remaining cake must be worth $$\frac 2 3$$ to this agent. So, in the worst-case, even if the entire remaining cake goes to some other agent, the envy will be at most $$\frac 2 3 - \frac 1 3 = \frac 1 3$$.
* Consider an agent $$i$$ that does not get any cake. In each round, he did not shout stop before any other agent. So, $$i$$’s value for the cake that any other agent gets is at most $$\frac 1 3$$. So, his envy towards any other agent can be at most $$\frac 1 3$$.

In fact, using this approach, $$\frac 1 3$$ is the best we can do (i.e., we’ve chosen this number carefully — because we need to balance the envy in both cases, to minimize the maximum envy. It’s not that hard to see why (think what would happen if we used $$\frac 1 5$$ instead of $$\frac 1 3$$ in step 2).

## Truthfulness

Is the cut-and-choose protocol truthful? No for the cutter, yes for the chooser. Obviously if you know the chooser’s valuation, you can try to abuse that so that you get more than 0.5 of the cake. But as the chooser, you literally just get the piece you pick so you have no incentive to lie.

In fact, none of the mechanisms we’ve seen so far (Dubins-Spanier, or Even-Paz) are truthful for all players — i.e., at least one player has an incentive to lie under some conditions. Hence, the algorithms are not truthful.

Note that whenever we talk about “truthful”, we’re asking “is there ANY possibility that lying is better?” (including the case in which you know your opponent’s valuation / strategy / anything!)

There is a pretty cool / simple truthful mechanism for when ALL agents have a piecewise uniform valuation, i.e., for each agent $$i$$, the denstiy function takes on the values only 0 or some $$s_i$$. Different agents can have different values for $$s_i$$ (that’s why it depends on $$i$$) but each agent can only have 1 non-zero value for $$s_i$$.

<figure><img src=".gitbook/assets/image 2.png" alt=""><figcaption></figcaption></figure>

The algorithm works as follows:

* Agent 1 starts at the left end of the cake, and agent 2 at the right end
* Each agent eats her desired part of the cake at the same speed until they meet. Here, meet is defined as “agent 1 crosses agent 2 on the x-axis, even if it is mid-air when either one is jumping forward”
* Any part that an agent jumps over goes to the other agent immediately. (And the other agent can eat this later, after the algorithm ends)

Basically what the piecewise uniform constraint means is that each agent either doesn’t like the part or it likes it (equally as all other parts it likes). Among the parts it likes, it has no preference for some parts over another. The reason we need this is so that you have no incentive to skip eating a piece you like to move faster towards a piece you like even more — these means that when you’re at a piece you like, you should just eat that piece because you’ll be getting the same amount of utility per unit eating time, and by jumping forward, you will have lesser total time to eat (because you’re going to meet the opponent sooner).

All the time you spend is only to eat the cake (you can teleport to the piece you want to eat instantly), and you both eat at the same speed. So, you have no reason to envy the other player — because you both spend the same amount of time eating, so the amount of value you get (by eating your piece) must be the same (or more) as the value you get if you had eaten the other agent’s intervals of cake (it’s more when he skips some cake that you value, so you get that directly)

Example of the cake-eating algorithm:

<figure><img src=".gitbook/assets/Screenshot_2024 10 24_at_12.36.21_PM.png" alt=""><figcaption></figcaption></figure>

Rough Proof of truthfulness:

* I won’t say i like a piece of cake if I don’t actually like it → because it does not give me any value by eating it, and I’m wasting time eating useless cake
* I won’t skip over any cake that I like → because my valuation function is piecewise uniform, even if i jump to a part, i would like that part equally as much as i like this one. So, i might as well eat what i like now instead of skipping it for later.

Even for a piecewise constant function (i.e., each agent’s valuation for any interval is a constant value), this algorithm does not work. In fact, there cannot be any envy-free truthful mechanism _even with piecewise constant functions with 2 agents_.

This show hows hard (read: impossible) it is to satisfy envy-freeness and truthfulness in the general case.
