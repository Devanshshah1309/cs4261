# Nash Equilibria

A game can be represented by:

* Players $$N = \{1, \cdots n \}$$
* Actions → players can do something to affect the world
* Outcomes → players’ actions lead to outcomes
* Preferences over outcomes → each player $$p_i$$ has a preferences / ranking of the outcomes (in terms of utility)

This provides a general, abstract framework for **strategic interaction**.

### Pure Nash Equilibrium

Given everyone else’s actions $$\vec a_{-i}$$, the **best response** set of player $$i$$ is:

$$
BR_i(\vec a_{-i}) = \{ b \in A_i | b \in \argmax u_i(\vec a_{-i}, b) \}
$$

That is, a player $$i$$’s best response can be any of the actions that give him the maximum possible utility, given everyone else’s actions.

An action profile $$\vec a$$ is a (pure) Nash equilibrium if:

$$
\forall i \in N, a_i \in BR_i(\vec a_{-i})
$$

That is, every player has picked their best response given everyone else’s actions. You can also think of it as: each player picks the action as if they were the last one to do so, and they knew everyone else’s actions.

> _Every player thinks “I’m doing the best I can, given everyone else’s actions.”_

### Mixed Nash Equilibrium

Instead of choosing a single action, one can play a random mix of them (to be unpredictable, otherwise if someone knows you well): $$\vec p_i \in \Delta(A_i)$$ is a probability distribution (from the set of all possible probability distributions) over player $$i$$’s actions.

(Note: In general, we use $$\Delta(X)$$ to represent the set of all possible distributions over X)

A (not necessarily pure) strategy profile can be represented as:

$$
\vec p = (\vec p_1, \cdots, \vec p_n) \in \Delta(A_1) \times \cdots \times \Delta(A_n)
$$

Then, a player’s utility (for a given mixed strategy) is given by:

$$
u_i(\vec p) = \sum_{\vec a \in A} u_i(\vec a)\Pr[\vec a] = E_{\vec a \sim \vec p}[u_i(\vec a)]
$$

In particular, note that the player’s ONLY care about the **expected value** of their strategy, NOT the variance / risk that their strategy runs. e.g. they would be indifferent to a strategy that guarantees them $50 vs. flipping a coin for $100  — both have the same expected value, and being “risk-neutral”, they wouldn’t care which one they did. That is, they are neutral / indifferent to risk → they don’t care if they take risk or don’t (they don’t avoid it - by taking the gauranteed money - nor do they seek it - by taking the coin flip; they’re just indifferent).

Then, $$\vec p$$ is a (not necessarily pure) nash equilibrium is when: for all players $$i \in N$$, and all possible strategy (possibly containing mixed actions) of each player $$q_i \in \Delta(A_i)$$, $$u_i(\vec p) \geq u_i(\vec p_{-i}, \vec q_i)$$ → their current strategy is at least as good as any other.

In these notes, we use “action” to refer to a pure strategy (only one possible action, played with probability = 1), and the general term “strategy” to refer to possibly mixed actions (i.e., playing multiple actions, each with some probability)

{% hint style="warning" %}
**Nash’s Theorem**: Unlike a _pure_ Nash Equilibrium, a (not necessarily pure) Nash Equilibrium ALWAYS EXISTS.
{% endhint %}

The way to compute nash equilibria in a 2x2 games (2 players, each have 2 actions) is by considering 2 possible cases:

1. Case 1 → Compute all NE (if any) in which at least one player plays a pure strategy (do this exhaustively, e.g. “row player plays action X while col player mixes between A and B with probability q and (1-q), then find the value(s) of q for which the best response is X — then you’ve found a NE)
2. Case 2 → Compute all NE (if any) in which both players mix between both strategies

Nash’s theorem says that the union of the set of NE’s found in case 1 and case 2 is non-empty → it doesn’t say whether the NE is pure / not.

The key idea behind case 2 is that the only time that a player would mix between 2 strategies is when (given the opponent’s strategy), he is indifferent to his 2 actions. That is, no matter what action he picks, his utility is the same _given the opponent’s strategy_. This allows us to equate the expected utilities of the two actions for player 1, to find the “mixing probabilities” for player 2 😲 (and vice versa).

### Dominant Strategies

We say that a strategy $$\vec p \in \Delta(A_i)$$ (weakly) dominates $$\vec q \in \Delta(A_i)$$ if:

$$
\forall \vec p_{-i} \in \Delta(A_{-i}) : u_i(\vec p_{-i}, \vec p) \geq u_i(\vec p_{-i}, \vec q)
$$

In words: no matter what everyone else does, picking $$\vec p$$ is at least as good as picking $$\vec q$$.

**Strict** domination is when $$u_i(\vec p_{-i}, \vec p) > u_i(\vec p_{-i}, \vec q)$$ for every $$\vec p_{-i}$$ → no matter what everyone else does, strategy $$\vec p$$ is STRICTLY better than $$\vec q$$ (and so, you should never even consider picking $$\vec q$$ !!).

{% hint style="warning" %}
**Theorem**: If an action $$a \in A_i$$ is STRICTLY dominated by some strategy $$\vec p \in \Delta(A_i)$$, then action $$a$$ is NEVER PLAYED with any positive probability in ANY Nash equilibrium.
{% endhint %}

Intuitively, if you would move all the probability out of the lamer / weaker actions into the ones that dominate it, and you strictly increase your utility. Hence, picking $$a$$ (with any non-zero probability) can never be a “best” response 🤯

**Iterated Removal of Strictly Dominated Strategies**

If you’re trying to find Nash Equilibria, then you can remove action profiles that contain a strictly dominated strategy (from either player’s side).

Note:

* This ONLY works if the action is _strictly_ dominated by another strategy (can be a strategy involving multiple actions — using mixed probabilities too)
* If you’re left with a single action / strategy profile at the end, then by Nash Theorem, it must be a NE.
* The reason this works is simply because those cells which contain a strictly dominated strategy (from either player’s side) are never going to happen, i.e., they can never be an equilibrium (because at least one player would change their strategy to the one which strictly dominates it)
* When considrering whether a strategy $$S$$ (may be mixed) strictly dominates an action $$P$$ for the row player, we don’t have to consider all possible strategies of the col player — we can just look at the individual actions and ensure that $$S$$ strictly dominates $$P$$ for every action. Then, since any strategy is a linear combination of actions, if $$S$$ strictly beats $$P$$ for every action of the col player, then $$S$$ strictly beats $$P$$ for every strategy involving those actions too. (Moreover, we only have to lok at the actions of the col players that are not already strictly dominated by other strategies — since col player would not play the weaker actions otherwise).



