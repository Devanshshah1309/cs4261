# Committee Voting

## Problem

Unlike auctions where everyone gets different things depending on what they want, in a voting setting, everyone gets the same thing (although they have different preferences). So, we’re trying to select a public outcome that is shared by everyone. e.g. everyone gets the same president.

In general, there are many different types of voting ballots (i.e., inputs):

* ranking → each voter submits an ordering for the candidates
* score → each voters submits a number for each candidate
* approval → each voter submits a subset of approved candidates
  * basically, each voter submits 1 for the candidates they approve, and 0 for the rest.
  * advantage: it’s simple and less cognitive effort is required from the voters’ perspectives (they just have to think whether or not they approve the voter, no relative comparisons needed);
  * disadvantage: it does not allow refined or combinatorial preferences (e.g. i can’t say “i want to approve A and B, but only if they are chosen together” or “i like A more than B”)

And there can be many different kinds of outputs of voting:

* single winner → e.g. president of a student council
* multiwinner / committee e.g. student council committee, dishes to serve in a reunion party. in this case, all the selected members of the committee are “equal” so the decision is just whether or not to include or exclude from the committee
* ranked committee → e.g. a committee consisting of president, vice-president and a secretary, where there’s some ordering in the positions of each member of the committee.

In this lecture, we’re only looking at approval voting in a multi-winner category

## Approval Committee Voting Setting

* Voters $$N = \{1, \cdots, n\}$$
* Candidates $$C$$, where $$|C| =m$$
* Voter $$i$$ approves a set of candidates $$A_i \subseteq C$$
* We want to choose a committee $$W$$ of size $$k$$ where $$k \leq m$$ is given (that is, we don’t get to decide the size of the committee — $$k$$ is an input to our mechanism, not something we can control)
* Voter $$i$$’s utility is: $$u_i(W) = |A_i \cap W|$$, i.e., how many members of the selected committee did voter $$i$$ approve (which makes sense, because you feel happier if people you approve of are selected)

Note that the above definition of voter utility remains the same even when we consider any other metrics / scores to optimize.

Also, we can think of some natural metrics to evaluate how good a committee selection is:

* (Utilitarian) Social Welfare → total number of approvals obtained by members of the committee (i.e., “excellence”). Notice that this is the same as the total utility of all the voters, because we can swap the order of the sums.
* Coverage: number of voters who approve at least one committee member (”diversity”). This is “kind of” an egalitarian metric in that we’re trying to minimize the number of voters with zero utility, but we don’t distinguish between any non-zero values so it’s not _really_ egalitarian.

Also, if a voter approves a candidate, we say that the candidate covers the voter (since picking this candidate is enough to make this voter have non-zero utility).

## Approval Voting (AV) and Chamberlin-Courant (CC)

Then, we can select committees trying to maximize these two metrics:

* Approval Voting (AV) → select a committee maximizing social welfare
* Chamberlin-Courant (CC) → select a committee maximizing coverage

Of course, there need not be a unique committee in each of these cases.

Moreover, AV is a truthful mechanism (since you’re trying to boost the number of votes your preferred candidates have) whereas CC is not (e.g. it’s possible to get more utility by reporting a truncated set of approved candidates).

## Beyond AV / CC

Consider this example:

* $$n = 301, m = 5, k = 3$$
* First 200 voters approve $$\{a, b, c \}$$
* Next 100 voters approve $$\{d\}$$
* And the last voter approves $$\{ e\}$$

Then, AV chooses $$\{a, b, c\}$$ and CC chooses $$\{a, d, e\}$$.

But, neither feels “proportional” or fair. Because these committees don’t really reflect the “approval profile”.

A more proportional committee would be something like: $$\{a, b, d \}$$ → since $$\frac 2 3$$ of the voters agree on $$a$$ and $$b$$, they should get $$\frac 2 3$$ of the seats on the committee, while only $$\frac 1 3$$ of the voters agree on $$d$$, so they should get $$\frac 1 3$$ of the seats.

Intuitively, a sufficiently large group of voters that agree on sufficiently many candidates should be correspondingly / proportionately represented / satisfied in the committee.

## Justified Representation

There are $$n$$ voters and $$k$$ committee slots, so a group of $$n/k$$ voters “deserves” one slot (of representation).

**First attempt**: For a group of voters $$S \subseteq N$$ such that $$|S| \geq \frac n k$$ and $$|\cap_{i \in S} A_i| \geq 1$$, we have $$|(\cap_{i \in S} A_i) \cap W| \geq 1$$. Basically, we’re saying that if we have a big enough group that together deserve at least one slot, and that they agree on at least one candidate, then at least one of the candidates they agree on should be selected in the committee.

This turns out to be too strong of a requirement, which cannot always be satisfied. For example:

* $$n=m=4, k = 2$$
* $$A_1 = \{a, b \}, A_2 = \{b, c \}, A_3 = \{c, d \}, A_4 = \{a, d \}$$
* Then, if we apply the rule above, then we would be forced to include all of $$a, b, c, d$$, but this exceeds the committee size.

To make our vocabulary / notation simpler, let’s call a group of voters $$S \subseteq N$$ such that $$|S| \geq \frac n k$$ and $$|\cap_{i \in S} A_i| \geq 1$$ to be a **cohesive group**. Basically a cohesive group makes up for at least one-slot-worth of representation, and agrees on at least one candidate (if they don't agree, they aren't "cohesive" and we can't do much to please them anyway).

Then, a justified representation (JR) is any committee that satisfies the following property: for any cohesive group of voters $$S \subseteq N$$, there exists $$i \in S$$ such that $$|A_i \cap W| \geq 1$$.

Basically, JR says that in any cohesive group, at least one voter should have non-zero utility. So, no cohesive group should go unrepresented. But It kind of feels… too weak? Since we only require _one_ voter to feel happy (in fact, just non-zero happiness), _no matter how big the cohesive group is_.

Also, it’s easy to see that AV may fail JR. Consider $$n=k=3, m = 4$$ where the first 2 voters approve $$a,b,c$$ and the last voter approves $$d$$, then AV picks $$a,b,c$$ but then the last voter is a cohesive group and has zero utility, hence does not satisfy JR).

Claim: CC always satisfies JR. Proof:

* Suppose for contradiction that it does not.
* Then, let $$S$$ be a cohesive group of voters that is unrepresented by the CC committee $$W$$, and let $$x$$ can be a candidate approved by all voters in $$S$$.
* Consider the marginal contribution of each $$w \in W$$ to the coverage. This is the number of voters who approve $$w$$ but no one else in $$W$$.
* Since teh coverage of $$W$$ is $$< n$$, the marginal contribution of some $$w^* \in W$$ is less than $$\frac n k$$ (otherwise, if everyone’s marginal contribution is at least $$\frac n k$$, then all voters should be covered).
* So, we can remove $$w^*$$ and add $$x$$ to obtain a higher coverage, which contradicts the fact that $$W$$ is CC. So, any CC committee also satisfies JR.

Hence, JR can be satisfied for any $$n, k, m$$ and approval profile.

## GreedyCC

Computing a CC committee is NP-hard (can be shown via a simple reduction from SET-COVER).

Fortunately, there is a greedy variant that runs in polynomial time (of course this does not guarantee maximal coverage, but it’s not bad).

The GreedyCC algorithm is quite simple: In each step, choose a candidate that covers as many uncovered voters as possible (i.e,. ignore the voters that are already happy and focus on the ones you want to cover). Repeat this until $$k$$ candidates have been chosen.

We claim that GreedyCC satisfies JR, and the proof follows exactly as above (i.e., if not, then there must’ve been a candidate who covers $$\frac n k$$ voters but we didn’t pick him in favor of someone else who satisfies less than $$\frac n k$$voters; this can’t happen because of the greedy nature of our algorithm).

## Extended JR

But one can ask the same question as asked AV and CC to JR: is JR sufficient?

Consider this example: Suppose have 100 voters, and 20 candidates and $$k=10$$. The first 50 voters like the first 10 candidates, and the next 50 voters like the next 10 candidates. Then, intuitively, we want to pick 5 out of 10 candidates from each group to be “proportional”. But even picking 9 from group 1 (i.e., that the first 50 voters approve) and 1 from group 2 satisfies JR.

So, our requirement of JR is too weak! So, we use **Extended Justified Representation (EJR)**.

To explain what EJR means, let’s strengthen our notion of cohesive groups first.

For a positive integer $$t$$, call a group of voters $$S \subseteq N$$ such that $$|S| \geq t \cdot \frac n k$$ and $$|\cap_{i ]in S} A_i | \geq t$$ a $$t$$-cohesive group. (Then, the previous definition of cohesive group is simply when $$t=1$$.)

Also, note that a $$p$$-cohesive group is always an $$r$$-cohesive group for any $$r \leq p$$, i.e., increasing the “order” of the cohesive group just strengthens the requirement, which is clear from the definition.

Then, a committee is said to satisfy EJR when: for any positive integer $$t$$ and any $$t$$-cohesive group of voters $$S \subseteq N$$, _there exists_ $$i \in S$$ such that $$|A_i \cap W| \geq t$$.

Notice that we still require only one of the voters in $$S$$ to have a utility of at least $$t$$ (others might get utility = 0 and that’s fine).

(Well, not really, we can show that under EJR, in any $$t$$-cohesive group $$S$$, the average utility of each person is more than $$\frac{t-1}{2}$$, so this definition is actually quite strong, even if it doesn’t seem like it.)

And this fixes the problem with JR.

## Proportional Approving Voting (PAV)

Fix an infinite non-increasing sequence $$s_1, s_2, \cdots$$

Thiele methods: A family of methods that involve choosing a committee $$W$$ by maximizing the score: $$\sum_{i \in N} (s_1 + s_2 + \cdots + s_{u_i(W)}$$.

So, basically this sequence $$s_1, s_2, \cdots$$ captures the utility function of the players. If a voter $$i$$ has $$p$$ of his approved candidates in the committee $$W$$, then his utility is $$s_1 + \cdots + s_k$$. In particular, the marginal utility to voter $$i$$ of the $$k$$th approved candidate being part of $$W$$ is $$s_k$$.

(Note that this sequence is from the _mechanism's_ point of view -- i.e., it is the mechanism's assumption of how the voter utility functions behave. BUT from the voter's point of view, the term "voter's utility" still just represents the number of approved candidates in the committee for any voter. To de-confuse, we can use "score" to represent a voter's sum $$s_1 + \cdots + s_k$$.)

This makes intuitive sense, because we want to capture the effect of diminishing marginal utility of the voters. Once you already have 5 approved candidates in $$W$$, then the 6th one cannot give you more (marginal) utility than the first one did. This is why the sequence needs to be non-increasing.

Notice that each voter has the same sequence — this is done to ensure that we treat all voters symmetrically, i.e., we don’t favor any voter over any other.

Then, AV is just a special case of this with $$s_i = 1$$for all $$i$$. Even CC is just a special case of this with $$s_1 = 1, s_i = 0$$ for all $$i > 1$$.

In Proportional Approval Voting (PAV), we set: $$s_i = \frac 1 i$$ for each $$i$$.

(So the utility function for each player follows a kind of $$\log$$ graph, since $$\int_x \frac 1 x dx = \ln x$$)

This means that if a voter approves $$r$$ candidates in the committee, the voter contributes $$1 + \frac 1 2 + \cdots + \frac 1 r$$ to the score of the committee.

The number $$1 + \cdots + \frac 1 r$$ is the $$r$$th harmonic number, usually denoted by $$H_r$$.

But… why harmonic numbers? Why does this work?

Because they result in a roughly proportional committee. The intuition is that AV is precisely utilitarian, CC is kind of egalitarian (up to a certain point), and PAV is kind of like Nash.

And the reason that PAV is like Nash is because maximizing $$\sum_{i \in N} H_{u_i(W)}$$ is very similar to maximizing $$\sum_{i \in N} \ln(u_i(W))$$ because $$H_r \approx \ln r + \gamma$$, where the approximation becomes better in the limit as $$r \to \infty$$. Moreover, since $$\ln$$ is a strictly monotonic increasing function, it’s equivalent to maximizing $$\prod_{i \in N} u_i(W)$$, because $$\sum_{i \in N} \ln {u_i(W)} = \ln (\prod_{i \in N} u_i(W))$$.

And we can’t directly use Nash because it’ll face a similar problem as CC, since it’s really scared of anyone getting 0 utility — so, it’ll first focus on covering all voters, before thinking of anything else). PAV is okay with some voters having utility if enough other voters are compensated in return.

Also….

Claim: PAV satisfies EJR. Proof sketch:

* Suppose for contradiction that $$u_i(W) < t$$ for all voters belonging to some $$t$$-cohesive group $$S$$.
* There is a candidate $$x$$ which is approved by all members of $$S$$ but not included in $$W$$.
* By adding $$x$$ to $$W$$, the PAV score increases by at least $$\frac 1 t \cdot \frac{tn}{k} = \frac n k$$, since $$x$$ must add at least $$\frac 1 t$$ worth of utility to all the members of this cohesive group.
* And now, we need to show that there is another candidate $$y \in W$$ such that by removing $$y$$ from $$W \cup \{x\}$$, the PAV score decreases by less than $$\frac n k$$. (We skip how to show that such a $$y$$ exists).
* And then, we would’ve picked $$x$$ instead of $$y$$ in $$W$$ to get a higher PAV score. So, the original $$W$$ could not have been PAV to start with. Contradiction.

But unfortunately, PAV is NP-hard to compute, and a greedy variant of PAV might not even satisfy JR.

So, we’re still trying to find a polynomial time algorithm that satisfies EJR… Luckily, we have it.

## Method of Equal Shares (MES)

* Each voter has a budget of \$$$\frac k n$$ (regardless of how many candidates there are)
* Each candidate costs $1. The voters who approve this candidate have to “pool” their money to add this candidate to the committee.

Then, the algorithm runs as follows:

* Start with an empty committee.
* In each round, we want to add a candidate whose approved voters have a total budget of $$\geq 1$$ left.
* If there are several such candidates, choose one such that teh maximum amount that any agent has to pay is minimized.
* If no more candidates can be afforded but committee still has size $$< k$$, fill in the rest of the committee by using some tie-breaking criterion (e.g. by maximizing approval score).

Clearly, MES never chooses more than $$k$$ candidates (since the total budget is $$n \cdot \frac k n = k$$ and each candidate costs $1, so you can't buy more than $$k$$ candidates)

{% hint style="warning" %}
**MES satisfies EJR and can be implemented in poly-time.**
{% endhint %}

Proof that MES satisfies EJR:

1. Suppose for contradiction that $$u_i(W) < t$$ for all voters $$i$$ belonging to some $$t$$-cohesive group $$S$$.
2. When MES stops, some voter $$i \in S$$ must have budget $$< \frac k {tn}$$ left. Why?
   1. Otherwise, if everyone has at least $$\frac{k}{tn}$$ budget, then they can all pool together and buy a candidate that they approve of. (And the committee could not have already been full without this candidate, since it’s only full iff _all_ voters pay _all_ their money to buy candidates, because it’s only possible to buy $$k$$ candidates with $$k$$ dollars)
3. So, $$i$$ has used budget $$> \frac k n - \frac{k}{tn} = \frac{(t-1)k}{tn}$$. Moreover, since we know that $$i$$ has approved $$\leq (t-1)$$ candidates, so for some chosen committee member, $$i$$ must have paid more than $$\frac{1}{t-1}\frac{(t-1)k}{tn} = \frac{k}{tn}$$.
4. Consider the _first_ committee member $$x$$ such that some voter in $$S$$ paid more than $$\frac{k}{tn}$$ for it.
5. Before $$x$$ was added, each voter in $$S$$ has $$\leq (t-1)$$ approved candidates and paid $$\leq \frac{k}{tn}$$ for each of them.
6. Thus, each voter in $$S$$ has budget at least $$\frac k n - (t-1)\cdot \frac{k}{tn} = \frac{k}{tn}$$ remaining.
7. Since $$|S| \geq \frac{tn}{k}$$, the voters in $$S$$ could afford a commonly approved candidate by paying $$\leq \frac{k}{tn}$$ each.
8. So, no voter in $$S$$ should have paid more than $$\frac{k}{tn}$$ for $$x$$, a contradiction!
