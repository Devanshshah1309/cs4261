# Nash Bargaining Solution

In a Nash bargaining problem, players have to reach an agreement on their payoffs that must satisfy some constraints. If these constraints are not satisfies, then the players don’t get anything.

<figure><img src=".gitbook/assets/Screenshot_2024 10 10_at_7.50.20_AM.png" alt=""><figcaption></figcaption></figure>



More generally, we can have a inequalities as the constraint too — where the “valid” region is a convex, compact set.

Convex → Any line segment drawn between 2 points on the boundary of the shape must lie entirely within the shape itself.

Compact → The shape (set of possible solutions) must be bounded and includes the boundary.

<figure><img src=".gitbook/assets/Screenshot_2024 10 10_at_7.53.20_AM.png" alt=""><figcaption></figcaption></figure>



In the above diagram, $$(d_1, d_2)$$ is the “disagreement point” — i.e., the payoffs that the two agents get in the event that they both disagree with each other and can’t come to a consensus on any point in $$S$$. That’s why in any rule-based function $$f$$ that needs to pick a “fair” outcome, it’s important to know what the payoffs are in the event of a disagreement. (e.g. if $$d_1$$ is more than any point in $$S$$, then player 1 has incentive to ignore whatever player 2 says and just violate the constraint so that he can get $$d_1$$ value)

## Pareto Optimality

An outcome $$(x_1, y_1)$$ Pareto dominates another outcome $$(x_2, y_2)$$ if $$x_1 \geq x_2$$ and $$y_1 \geq y_2$$ and at least one of these two inequalities is strict. That is, at least one of the players is strictly better off without the other being worse off.

In this case, $$(x_1, y_1)$$ is said to be a Pareto improvement of $$(x_2, y_2)$$.

An outcome $$(x,y)$$ is **Pareto optimal** if it is not Pareto dominated by any other outcome. That is, no one can improve without hurting someone else. (This is a notion of economic effciency).

The **Pareto frontier** is the set of all such Pareto optimal outcomes.

<figure><img src=".gitbook/assets/Screenshot_2024 10 10_at_7.56.14_AM.png" alt=""><figcaption></figcaption></figure>



## Desirable Properties

Here,

* $$\vec d$$ is the disagreement point that is picked in the event that the mechanism cannot find any solution in $$S$$ (i.e., it is the default alternative to an agreement — similar to BATNA in negotiations). Generally, $$\vec d =0$$ so that if the players can’t agree on something, they both get nothing.
* $$f_1$$ and $$f_2$$ are the components of the rule / function / mechanism $$f$$ that decides what point to pick given the constraint set $$S$$ and the outcome in the case of disagreement, $$d$$. e.g. $$f$$ can be the rule that tries to maximize the total welfare (utilitarian) or the product of the players’ utilities (nash), etc.

In such a “bargaining” solution, we are looking for the following properties:

* **Efficiency**: No outcome $$(v_1,v_2)$$ Pareto dominates our chosen $$(f_1(S, \vec d), f_2(S, \vec d))$$ Here, we’re referring to “economic efficiency” in the sense that no one can be improved without someone else being worse off, not computational efficiency.
* **Symmetry**: Let $$S^T = \{(y, x) : (x, y) \in S\}$$ and $$\vec d^T = (d_2, d_1)$$; then $$(f_1(S^T, \vec d^T), f_2(S^T, \vec d^T)) = (f_2(S, \vec d), f_1(S, \vec d))$$. That is, we don’t favor one player over the other (e.g. there’s no special treatment given to the “first player” or the “second player” — the labels assigned to the players shouldn’t matter. And hence, if we change the names of the players, the outcome should just be reversed).
* **Independence of Irrelevant Alternatives (IIA)**: Let $$S' \subseteq S$$ be such that $$(f_1(S, \vec d), f_2(S, \vec d))\in S'$$’. Then, $$(f_1(S', \vec d), f_2(S', \vec d)) = (f_1(S, \vec d), f_2(S, \vec d))$$. If the solution including a larger set of outcomes is $$x$$, and and then we eliminate some of the solutions but the set still contains $$x$$, the optimal solution should still be $$x$$. Removing irrelevant options shouldn’t change your final answer. e.g. if you’re at an ice-cream shop, and you prefer chocolate > vanilla > strawberry, then even if you remove strawberry, you will still pick chocolate over vanilla. So, removing strawberry should not make you suddenly pick vanilla instead. Adding / removing an option should not change the pairwise ranking of the other options.
*   **Invariance under Equivalent Representations (IER)**: for any $$\alpha_1, \alpha_2 \in \mathbb{R}, \vec \beta \in \mathbb{R}^2$$, we have:

    $$
    f_i((\alpha_1, \alpha_2)S + \vec \beta, (\alpha_1, \alpha_2)\vec d + \beta) = \alpha_if_i(S, \vec d) + \beta_i
    $$

    If you stretch / scale and shift the space, the solution should still be preserved under linearity. Note that $$(\alpha_1 \alpha_2)S = \{(\alpha_1 x_1, \alpha_2 x_2) : (x_1, x_2) \in S \}$$ (And this is not trivial to satisfy — e.g. utilitarian solution does not satisfy IER.)

    *   Example of Utilitarian solution not satisfing IER: Suppose we try to maximize x+y subject to x2+y2=1. What is the maximum? Now, suppose we stretch the x-axis by a factor of 2, so we're now maximizing x+y subject to (x/2)2+y2=1 (in other words, we stretch the circle into a "horizontal oval"). What is the maximum?

        ***

        Compare this with the Nash bargaining solution. Suppose we try to maximize xy subject to x2+y2=1. What is the maximum? Now, suppose we stretch the x-axis by a factor of 2, so we're now maximizing xy subject to (x/2)2+y2=1. What is the maximum?

A pretty interesting read is: [Arrow’s Impossibility Theorem](https://en.wikipedia.org/wiki/Arrow's\_impossibility\_theorem).

## Nash Bargaining Solution

(Read [this](https://www.sciencedirect.com/science/article/pii/S0165176524004427?via%3Dihub) paper to understand some of the key terms as used in literature.)

In Nash bargaining solution, we try to find $$\max(v_1 - d_1)(v_2-d_2)$$ s.t. $$(v_1, v_2) \in S$$

Generally, $$d_1 = d_2 = 0$$ (i.e., if players disagree and violate the constraint, then they both get nothing) so we’re trying to maximize the product $$v_1 v_2$$.

{% hint style="warning" %}
**Theorem: The Nash bargaining solution satisfies all 4 properties — efficiency, symmetry, IIA, and IER.**
{% endhint %}

It’s not that hard to show that this is true.

But a much stronger claim is this:

{% hint style="warning" %}
**Theorem: The Nash bargaining solution is the ONLY solution that satisfies efficiency, symmetry, IIA, and IER.**
{% endhint %}

That is, these 4 properties characterize the Nash bargaining solution — if you want these 4 properties, you have no choice but to use this.

## Other Solutions

There are different notions of fairness and “optimality” too.

* **Utilitarian**: tries to maximize total social welfare, i.e., $$\max \sum_i u_i(A)$$
* **Nash**: tries to maximize the product, i.e., $$\max \prod_i u_i(A)$$
* **Egalitarian**: tries to maximize the minimum utility (to reduce inequality), i.e., $$\max \min_i u_i(A)$$

Intuiitvely, Nash lies in the middle of utilitarian (just maxiimze total welfare without caring about the distribution) and egalitarian (care more about the poor people).

To find all the solutions for these different constraints, we can use the technique of Lagrange multiplliers (watch the videos on Khan Academy for a great explanation).

Note: remember to check the boundary points separately! e.g. if you try to maximize $$x^3$$ in the range $$[-1, 1]$$ by setting the derivative equal to 0, you’ll only get $$x=0$$ as the critical point. BUT, the true maximium is at $$x=1$$, and the true minimum is at $$x=-1$$. This happens because the function might be increasing / decreasing when the interval cuts it (so its derivative at that point will not be zero, but it can still be the maximum / minimum in that range). Setting derviative = 0 just gives you the local extremas (or critical points).
