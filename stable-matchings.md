# Stable Matchings

Until now, we had been talking about markets with money. For example, auctions include items on sale, and players can bid money to buy them. The shapley value and core dealt with how to distribute the value generated (in the form of money) to the different players in the group.

But now, we consider markets without any money. Instead, we consider markets in which we have two groups (think: bipartite graph) and each of them have preferences for members of the other group. Here, “preferences” is asusmed to be a total order (aka ranking).

Examples;

* Two-sided markets: Dating websites (guys have preferences over girls, and girls have preferences over guys), College Admission (students have preferences over colleges and colleges have preferences over students), Job markets, etc.
* One-sided markets: Students bidding for courses (students have preferences over courses, but courses umm, hopefully, don’t), dormitory room assignment (students have preferences over rooms, but a room don’t care which student occupies it)
* Barter Exchanges: Kidney exchange

### What makes for a Good Market?

Again (like with Shapley value), we’re trying to make our desires explicit (about what we would like to achieve through the mechanism) and then we will try to find an algorithm that achieves these properties.

* Thick — lots of buyers, lots of sellers, lots of options.. and everyone is aware of all the options available to them (i.e., transparency)
* Timely — not too fast (people to have the time to weigh out decisions), not too slow (offers are processed quickly, and new offers arrive quickly)
* Safe — people cannot be hurt by revealing preferences (aka truthfulness / strategyproof), outcomes are fair, and people are better off by participating than not (i.e. playing should have a positive payoff)

An example of a bad equilibium is companies trying to get the best students to sign full-time contracts long before they graduate so that other companies can’t “steal” them — this is bad for companies since they have less signal about the students at the time of offer, and bad for students because they don’t have all the options available to them. Another example is “exploding offers” by companies.

### Stable Matchings

Consider the problem of medical students being assigned to hospitals (for their residency program) — it’s a two-sided market, and we hope to find a “fair” matching.

So, we have:

* A set of students $$S = \{s_1, \dots, s_n\}$$
* A set of hospitals $$H= \{h_1, \cdots, h_m \}$$
* Each student $$s \in S$$ has a strict preference order over $$H$$, denoted by $$\succ_s$$
* Each hospital $$h \in H$$ has a strict preference order over $$S$$, denoted by $$\succ_h$$
* A matching $$M : S \to H$$ maps each student to a hospital.
* Assume for simplicity that we have the same number of hospitals and students, though this is not an issue in practice because we can just “duplicate” a hospital node $$m$$ times to indicate that the hospital is willing to accept upto $$m$$ students; and we can insert dummy nodes too, if needed,

Then, a pair $$(s, h) \in S \times H$$ **blocks** a matching $$M$$ if $$h \succ_s M(s)$$ and $$s \succ_h M^{-1}(h)$$. That is, both $$s$$ and $$h$$ prefer each other over their current partners. So… they would rather run away from this mechanism and not participate (maybeee it’s easier to think of the dating app example — if boy x and girl y prefer each other over their current matches, they would (probably?) ditch their current partners, and go out with each other). It measures a notion of “deviation” — you don’t want any pair of players to have the incentive to ignore the matching’s advice, and deviate from the group.

A matching $$M$$ is called **stable** if there are no blocking pairs.

### Gale-Shapley Algorithm

First of all, appreciate the difficulty of the problem we’re trying to solve. We’re trying to find a stable matching _for any given preference order_.

It’s not even obvious that such a matching exists in the first place! And even if it does exist, it’s not obvious whether it can be found in polynomial time.

Well… that’s why Gale and Shapley got the Nobel Price in Economics for coming up with this algorithm!

Btw, it’s also called (by very few folks) the “deferred acceptance” algorithm (since the assignments made during the algorithm are “temporary’ and is only “accepted” at the very end; hence, the acceptance has been deferred).

The algorithm is actually very simple (and elegant):

1. Start with all students unassigned.
2. While there are unassigned students
   1. Each unassigned student proposes to his/her favorite not-yet-proposed-to hospital (i.e., a student cannot propose to the same hospital twice — hmm not bad advice even for the dating example)
   2. Each hospital looks at the list of students that proposed to it in this round + whoever is assigned to it right now, and it chooses its most preferred student. Everyone else remains unassigned.
3. Return the resulting matching.

**Theorem: The G-S algorithm terminates in at most** $$n^2$$ **iterations with a stable matching**.

Proof:

* $$n^2$$ iterations, because in each round there is at least one unassigned student, who will propose to a hospital — and since no student proposes to the same hospital twice, we can have at most $$n^2$$ proposals, and hence, $$n^2$$ iterations.
* Terminates with a perfect matching (i.e., everyone is matched):
  * If not, some student (say, $$x$$) is rejected from all $$n$$ hospitals
  * A hospital only rejects a student in favor of a better student
  * So, once a hospital is matched, it remains matched throughout (meaning that there is some student attached to it, even though which student it is can change, it will only change to someone better)
  * Since $$x$$ was rejected from all hospitals, then all hospitals are matched (because they would only reject $$x$$ if they found someone better).
  * Contradiction! If all $$n$$ hospitals are matched, and each hospital is matched with a unique student, then all the $$n$$ students must be matched too.
* The perfect matching is stable:
  * Consider a student $$s$$ and a hospital not matched to each other
  * Case 1: $$s$$ never proposed to $$h$$ This means that $$s$$ got matched to a better hospital than $$s$$, since $$s$$ goes down her list of preferred hospitals
  * Case 2: $$h$$ rejected $$s$$ But $$h$$ would only reject $$s$$ if it found a better student than $$s$$.
  * So, we’ve shown that the only way a $$(s,h)$$ pair is not matched is when at least one of them prefers their current partner to the other.

### How Fair is the Gale-Shapley Algorithm?

Suppose that students’ preferences are such that each student ranks a different hospital first. Then, G-S terminates in a single round with a perfect stable matching. Sounds great right? Well… not really — because the hospitals didn’t get any say in this! They were only ever given the choice of a single student, and so it’s very “unfair” to them. In this case, the students get their top choice but the hospitals might not.

Note that the two groups (students and hospitals) are not treated the same way by the algorithm — that is, there is a “proposing” group and the other group that receives the proposals.

So, in general, when we “swap” the roles of the two groups, we shouldn’t expect to find the same stable matching. In fact, this is a way to test whether there exists only one unique stable matching — run the algorithm 2 times, one with students proposing and the other with hospitals proposing, and if they yield the same matching, then there is a unique matching.

Intuitively, the proposing group has more “power” (kind of?) because each of its members has the _choice_ to propose to _any of the other group members_ but the other group only sees the proposals it receives. So, it seems as if the algorithm “favors” the proposing group.

And indeed, this intuition turns out to be correct!

Given a student $$s \in S$$, a hospital $$h \in H$$ is called **valid** if there exists some stable matching $$M$$ such that $$M(s)=h$$. Let $$best(s)$$ be the most highly ranked valid hospital for $$s$$.

Define a valid student for a hospital similarly. And let $$worst(h)$$ be the least highly-ranked valid student for $$h$$.

💡 **Theorem: The G-S algorithm (with students proposing) assigns:**

* **Each student** $$s \in S$$ **to the hospital** $$best(s)$$**, and**
* **Each hospital** $$h \in H$$ **to the student** $$worst(h)$$

Firstly, this is not an obvious claim — we’re saying that ALL the students get matched to their best valid hospital _simultaneously_ (in a single matching!). So, looking at the matching determined by G-S, all students will find that they’ve reached the best scenario (given stability constraints).

Again, the intuition is that students go down their priority lists, so they only propose to a worse hospital IF they have been rejected from a better one… So, they get the best hospitals they can (we’ll prove it formally in a bit). And the opposite is true for the hospitals, because they don’t have any optionality here.

It’s clearly much better for the proposing side and worse for the side that receives proposals (i wonder if this is true even in the dating example hmm 🤔). So, it’s not “really’” fair because both sides would fight to be the proposing side in this algorithm.

Note: As the algorithm progresses, the quality of matches that a particular hospital gets increases (as it rejects students in favor of better students), and the quality of matches that a particular student gets decreases (as it gets rejected from hospitals and has to propose to worse hospitals). That is, hospitals go up their preference list, and students go down theirs.

We’ll only prove one part of the theorem above (the second is similar).

Claim: each student $$s$$ is assigned to $$best(s)$$

Proof: Asusme for contradiction that this is not the case.

* There is a student $$s$$ that is NOT assigned to $$best(s)$$
* Of course, $$s$$ cannot be matched with any hospital above $$best(s)$$ in the final matching because that would contradict the definition of $$best(s)$$.
* So, $$s$$ must be matched to a hospital below $$best(s)$$. But this can only happen if $$s$$ proposed to $$best(s)$$ AND got rejected from it.
* Further let $$s$$ be the _first_ student rejected by her best valid hospital (i.e., none of the other students have been rejected by their respective best valid hospitals yet). Precisely, $$\forall x \in S \setminus \{s\}, x \text{ has not been rejected from } best(x) \text{ yet}$$. And let $$best(s)=h$$ (and by definition, $$h$$ must be a valid hospital — $$(s,h)$$ must be part of _some_ stable matching).
* Suppose that $$s$$ ends up matched to some $$h'$$, where $$h \succ_s h'$$.
* $$h$$ must’ve rejected $$s$$ in favor of some better student, $$s'$$ (since $$s' \succ_h s$$)
* Since $$s$$ is the first student to be rejected by her best valid hospital, $$h \succcurlyeq_{s'} best(s')$$ — $$s'$$ could not have been rejected by $$best(s')$$ yet, and since students go down their list while proposing.
* There exists another stable matching $$M'$$ where $$s$$ is matched to $$h$$ — since $$best(s)=h$$ needs to be “valid”.
* And in that matching $$M'$$, suppose that $$s'$$ is matched to $$h'' \neq h$$ (can’t be $$h$$ because $$h$$ is taken by $$s$$).
* So, we have: $$h'' \preccurlyeq_{s'} best(s') \preccurlyeq_{s'} h$$, so $$h'' \prec_{s'} h$$
* But since $$h$$ rejected $$s$$ in favor of $$s'$$ (i.e., $$h$$ likes $$s'$$ more than $$s$$) AND $$s'$$ likes $$h$$ more than $$h''$$, then $$(s', h)$$ forms a blocking pair in $$M'$$ — $$h$$ would rather choose $$s'$$ over $$s$$, and $$s'$$ would rather choose $$h$$ over $$h''$$.
* But this contradicts the fact that $$M'$$ is a stable matching.
* Hence, $$s$$ is never rejected from $$best(s)$$ — and this holds true for any $$s$$.

Some other important Results:

* Gale-Shapley is non-truthful for non-proposing participants, but truthful from the point of view of the proposing dside. But successful mirepresentation requires knowledge of the other agents’ preferences too — without such knowledge, misrepresentation could give an agent a worse assignment. So, it’s a “regret-free truth-telling mechanism” in that even after seeing the final matching, an agent cannot deduce a strategy that could guarantee a better outcome in hindsight.
* There is a known impossibility result that it is IMPOSSIBLE to design a mechanism that is simultaneously truthful for both sides AND always produces a stable matching. (i.e., G-S is the next best thing we can hope for)
