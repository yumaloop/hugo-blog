---
title: Counterfactual Regret Minimization
draft: false
date: 2020-02-10

categories:
- StatML
tags:
- RL
- Game Theory
- Regret
- Python
---

In this post, I introduce you the Counterfactual Regret Minimization (CFR Algorithm). It is mainly used for the algorithm to figure out the optimal strategy of a extensive-form game with incomplete information such as Poker and Mahjong.


### Extensive-form Game

- Set, variables
  - $N: $ set of players
    - $i \in N$: player
  - $A :$ set of actions
    - $a \in A: $ action
  - $H: $set of sequences
    - $h \in H: $ sequences (= possible history of actions, $h = (a_1, \dots, a_t$)
    - $Z \subseteq H: $ set of terminal histories. $Z = \{z \in H \vert \forall h \in H, z \notin h \}$
    - $z \in Z$: sea
- Function, relations
  - $u_i: Z \to \mathbb{R}: $ utility function of player $i$
  - $\sigma_i: A \to [0,1]$ a strategy of player $i$, probability distribution on action set $A$.
  - $\sigma~: A^N \to [0,1]$ a strategy profile, $\sigma := (\sigma_1, \dots, \sigma_N)$
  - $\pi^{\sigma}_i: H \to [0,1]: $ probability of history $h$ under a strategy $$\sigma_$ of player $i$
  - $\pi^{\sigma}: H^N \to [0,1]: $ probability of history $h$ under a strategy profile $\sigma$
  
Then, you can also interplate $u_i$ as the function mapping a storategy profile $\sigma$ to its utility. 

$$
\begin{align}
u_i(\sigma) 
&= \sum_{h \in Z} u_i(h) \pi^{\sigma}(h) \\\\
&= \sum_{h \in Z} u_i(h) \prod_{i \in N} \pi^{\sigma}_i(h)
\end{align}
$$
\
<br>

### Nash equilibrium

**Definition:** $(\text{Nash equilibrium})$

In $N$-player extensive game, a strategy profile $\acute{\sigma} := (\acute{\sigma_1}, \dots, \acute{\sigma_N})$ is the Nash equilibrium if and only if the followings holds.

$$
\begin{aligned}
u_1(\acute{\sigma_1}, \dots, \acute{\sigma_N}) 
&\geq \underset{\sigma_1}{\rm max} ~ u_1(\sigma_1, \acute{\sigma_{-1}}) \\\\
u_2(\acute{\sigma_1}, \dots, \acute{\sigma_N}) 
&\geq \underset{\sigma_2}{\rm max} ~ u_2(\sigma_2, \acute{\sigma_{-2}}) \\\\
&~ \vdots \\\\
u_N(\acute{\sigma_1}, \dots, \acute{\sigma_N}) 
&\geq \underset{\sigma_N}{\rm max} ~ u_N(\sigma_N, \acute{\sigma_{-N}})
\end{aligned}
$$


**Definition: $\text{(}\varepsilon\text{-Nash equilibrium)}$**

In $N$-player extensive game, a strategy profile $\acute{\sigma} := (\acute{\sigma_1}, \dots, \acute{\sigma_N})$ is the $\varepsilon$-Nash equilibrium if and only if the followings holds when $\forall \varepsilon \geq 0$ is given.

$$
\begin{aligned}
u_1(\acute{\sigma_1}, \dots, \acute{\sigma_N}) + \varepsilon
&\geq \underset{\sigma_1}{\rm max} ~ u_1(\sigma_1, \acute{\sigma_{-1}}) \\\\
u_2(\acute{\sigma_1}, \dots, \acute{\sigma_N}) + \varepsilon
&\geq \underset{\sigma_2}{\rm max} ~ u_2(\sigma_2, \acute{\sigma_{-2}}) \\\\
&~ \vdots \\\\
u_N(\acute{\sigma_1}, \dots, \acute{\sigma_N}) + \varepsilon
&\geq \underset{\sigma_N}{\rm max} ~ u_N(\sigma_N, \acute{\sigma_{-N}})
\end{aligned}
$$

<br>

### Regret matching

- Average overall regret of player $i$ at time $T$：

$$
R_i^T 
:= \underset{\sigma_i^*}{\rm max} ~
\frac{1}{T} \sum_{t=1}^{T} \left( u_i(\sigma_i^*, \sigma_{-i}^{t}) - u_i(\sigma_i^t, \sigma_{-i}^{t}) \right)
$$

- Average strategy for player $i$ from time $1$ to $T$：

$$
\begin{align}
\overline{\sigma}\_i^t(I)(a) 
&:= \frac{\sum\_{t=1}^{T} \pi_i^{\sigma^t}(I) \cdot \sigma^t(I)(a)}{\sum\_{t=1}^{T} \pi\_i^{\sigma^t}(I)} \\\\
&= \frac{\sum\_{t=1}^{T} \sum\_{h \in I} \pi\_i^{\sigma^t}(h) \cdot \sigma^t(h)(a)}{\sum\_{t=1}^{T} \sum_{h \in I} \pi\_i^{\sigma^t}(h)}
\end{align}
$$

If the average overall regret holds $R_i^T \leq \varepsilon$, the average strategy $\overline{\sigma}_i^t(I)(a) $ is $2 \varepsilon$-Nash equilibrium for player $i$ in time $t$. So that, in order to derive Nash equilibrium, we should minimize the average overall regret $R_i^T$ or its upper bound $\varepsilon$ according to $R_i^T \to 0 ~~ (\varepsilon \to 0)$.

<br>

### CFR Algorithm

- Counterfactual utility：

$$
\begin{align}
u_i(\sigma, I) = \frac{\sum_{h \in H, h' \in Z} \pi_{-i}^{\sigma}(h)\pi^{\sigma}(h,h')u_i(h) }{\pi_{-i}^{\sigma}(I)}
\end{align}
$$

- immediate counteractual regret of action $a$ in Information set $I$:

$$
\begin{aligned}
R_{i,imm}^{T}(I, a)
:=
\frac{1}{T} \sum_{t=1}^{T} 
\pi_{-i}^{\sigma^t}(I)
\left( 
u_i(\sigma^t_{I \to a}, I) - u_i(\sigma^t, I) 
\right)
\end{aligned}
$$

- Immediate counterfactual regret of Information set $I$：

$$
\begin{aligned}
R_{i,imm}^{T}(I)
&:= \underset{a \in A(I)}{\rm max} ~
\frac{1}{T} \sum_{t=1}^{T} 
\pi_{-i}^{\sigma^t}(I)
\left( 
u_i(\sigma^t_{I \to a}, I) - u_i(\sigma^t, I) 
\right) 
\end{aligned}
$$

The following inequality holds for **the average overall regret** $R_i^T $ and **the immediate counterfactual regret**  $R_{i,imm}^{T}(I)$:

$$
\begin{aligned}
R_i^T 
\leq \sum_{I \in \mathcal{I}_i} &R_{i,imm}^{T,+}(I) \\\\
where ~~~
&R_{i,imm}^{T, +}(I) 
:= max(R_{i,imm}^{T}(I), 0)
\end{aligned}
$$

So that, we obtain the sufficient condition of $R_{i,imm}^{T}(I)$  for the average strategy $\overline{\sigma}_i^t(I)(a)$ to become a Nash equilibrium strategy as below. 

$$
\sum_{I \in \mathcal{I}_i} R_{i,imm}^{T,+}(I) \to 0 ~~~ \Rightarrow ~~~ R_i^T \to 0 ~~~ \Rightarrow ~~~ \varepsilon \to 0.
$$

Now all we need is to minimize the immediate counterfactual regret  $R_{i,imm}^{T}(I)$. 

In addition, as can be seen from the above formula, the computational complexity of the CFR algorithm depends on the number of information sets $I$. Also, to avoid the complete search of game tree (searching all information sets $I$), subsequent algorithms such as CFR + propose an abstraction of the game state.


### Python code to run CFR algorithm for Kuhn Poker

```python
import numpy as np

# Number of actions a player can take at a decision node.
_N_ACTIONS = 2
_N_CARDS = 3

def main():
    """
    Run iterations of counterfactual regret minimization algorithm.
    """
    i_map = {}  # map of information sets
    n_iterations = 10000
    expected_game_value = 0

    for _ in range(n_iterations):
        expected_game_value += cfr(i_map)
        for _, v in i_map.items():
            v.next_strategy()

    expected_game_value /= n_iterations
    display_results(expected_game_value, i_map)


def cfr(i_map, history="", card_1=-1, card_2=-1, pr_1=1, pr_2=1, pr_c=1):
    """
    Counterfactual regret minimization algorithm.
    Parameters
    ----------
    i_map: dict
        Dictionary of all information sets.
    history : [{'r', 'c', 'b'}], str
        A string representation of the game tree path we have taken.
        Each character of the string represents a single action:
        'r': random chance action
        'c': check action
        'b': bet action
    card_1 : (0, 2), int
        player A's card
    card_2 : (0, 2), int
        player B's card
    pr_1 : (0, 1.0), float
        The probability that player A reaches `history`.
    pr_2 : (0, 1.0), float
        The probability that player B reaches `history`.
    pr_c: (0, 1.0), float
        The probability contribution of chance events to reach `history`.
    """
    if is_chance_node(history):
        return chance_util(i_map)
    if is_terminal(history):
        return terminal_util(history, card_1, card_2)

    n = len(history)
    is_player_1 = n % 2 == 0
    info_set = get_info_set(i_map, card_1 if is_player_1 else card_2, history)

    strategy = info_set.strategy
    if is_player_1:
        info_set.reach_pr += pr_1
    else:
        info_set.reach_pr += pr_2

    # Counterfactual utility per action.
    action_utils = np.zeros(_N_ACTIONS)

    for i, action in enumerate(["c", "b"]):
        next_history = history + action
        if is_player_1:
            action_utils[i] = -1 * cfr(i_map, next_history, card_1, card_2, pr_1 * strategy[i], pr_2, pr_c)
        else:
            action_utils[i] = -1 * cfr(i_map, next_history, card_1, card_2, pr_1, pr_2 * strategy[i], pr_c)

    # Utility of information set.
    util = sum(action_utils * strategy)
    regrets = action_utils - util
    if is_player_1:
        info_set.regret_sum += pr_2 * pr_c * regrets
    else:
        info_set.regret_sum += pr_1 * pr_c * regrets

    return util

def is_chance_node(history):
    """
    Determine if we are at a chance node based on tree history.
    """
    return history == ""

def chance_util(i_map):
    expected_value = 0
    n_possibilities = 6
    for i in range(_N_CARDS):
        for j in range(_N_CARDS):
            if i != j:
                expected_value += cfr(i_map, "rr", i, j, 1, 1, 1/n_possibilities)
    return expected_value/n_possibilities


def is_terminal(history):
    possibilities = { "rrcc": True, "rrcbc": True,
                     "rrcbb": True, "rrbc": True, "rrbb": True}
    return history in possibilities

def terminal_util(history, card_1, card_2):
    n = len(history)
    card_player = card_1 if n % 2 == 0 else card_2
    card_opponent = card_2 if n % 2 == 0 else card_1

    if history == "rrcbc" or history == "rrbc":
        # Last player folded. The current player wins.
        return 1
    elif history == "rrcc":
        # Showdown with no bets
        return 1 if card_player > card_opponent else -1

    # Showdown with 1 bet
    assert(history == "rrcbb" or history == "rrbb")
    return 2 if card_player > card_opponent else -2

def card_str(card):
    if card == 0:
        return "J"
    elif card == 1:
        return "Q"
    elif card == 2:
        return "K"

def get_info_set(i_map, card, history):
    """
    Retrieve information set from dictionary.
    """
    key = card_str(card) + " " + history
    info_set = None

    if key not in i_map:
        info_set = InformationSet(key)
        i_map[key] = info_set
        return info_set

    return i_map[key]

class InformationSet():
    def __init__(self, key):
        self.key = key
        self.regret_sum = np.zeros(_N_ACTIONS)
        self.strategy_sum = np.zeros(_N_ACTIONS)
        self.strategy = np.repeat(1/_N_ACTIONS, _N_ACTIONS)
        self.reach_pr = 0
        self.reach_pr_sum = 0
        
    def next_strategy(self):
        self.strategy_sum += self.reach_pr * self.strategy
        self.strategy = self.calc_strategy()
        self.reach_pr_sum += self.reach_pr
        self.reach_pr = 0

    def calc_strategy(self):
        """
        Calculate current strategy from the sum of regret.
        """
        strategy = self.make_positive(self.regret_sum)
        total = sum(strategy)
        if total > 0:
            strategy = strategy / total
        else:
            n = _N_ACTIONS
            strategy = np.repeat(1/n, n)

        return strategy

    def get_average_strategy(self):
        """
        Calculate average strategy over all iterations. This is the
        Nash equilibrium strategy.
        """
        strategy = self.strategy_sum / self.reach_pr_sum

        # Purify to remove actions that are likely a mistake
        strategy = np.where(strategy < 0.001, 0, strategy)

        # Re-normalize
        total = sum(strategy)
        strategy /= total

        return strategy

    def make_positive(self, x):
        return np.where(x > 0, x, 0)

    def __str__(self):
        strategies = ['{:03.2f}'.format(x)
                      for x in self.get_average_strategy()]
        return '{} {}'.format(self.key.ljust(6), strategies)

def display_results(ev, i_map):
    print('player 1 expected value: {}'.format(ev))
    print('player 2 expected value: {}'.format(-1 * ev))

    print()
    print('player 1 strategies:')
    sorted_items = sorted(i_map.items(), key=lambda x: x[0])
    for _, v in filter(lambda x: len(x[0]) % 2 == 0, sorted_items):
        print(v)
    print()
    print('player 2 strategies:')
    for _, v in filter(lambda x: len(x[0]) % 2 == 1, sorted_items):
        print(v)

if __name__ == "__main__":
    main()

```
