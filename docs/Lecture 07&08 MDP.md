---
author: Hongyi Chen
date: today
title: Lecture 07&08 MDP
---

## CS 180 - Introduction to AI

### Lecture 7&8: Markov Decision Processes

#### 1. Non-deterministic Search

An MDP is defined by:
* **A set of states $s \in S$**
* **A set of actions $a \in A$**
* **A transition function $T(s, a, s')$** 引入了非确定性动作的可能性
    * Probability that $a$ from $s$ leads to $s'$, i.e., $P(s'|s, a)$
    * Also called the **model** or the **dynamics**
* **A reward function $R(s, a, s')$**

* **A start state**
* **Maybe a terminal state**

**Discount factor $\gamma$**
Sooner rewards probably have higher utility than later rewards!
$$
\gamma \in [0,1]
$$

Markovianess: 无记忆性， 如果我们知道当前状态，那么了解过去并不能给我们提供更多关于未来的信息。

#### 2. Bellman Optimality Equation

首先我们要为状态定价：“当前”状态的价值，是由“未来”状态的价值来定义的。
$$\text{Bellman equation: \\   \  } V^*(s) = \max_a Q^*(s, a)$$
**含义：** 一个状态 $s$ 的最优价值 等于从这个状态出发，所能采取的所有动作 $a$ 中，能产生的那个最优的动作价值 ($Q^*(s, a)$)。
$$Q^*(s, a) = \sum_{s'} T(s, a, s')[R(s, a, s') + \gamma V^*(s')]$$


#### 3. Value Iteration

如何实际计算这些最优值？
我们需要time limited values!

对于时间限制为 $k$ 个时间步的状态 $s$ ，时间限制值表示为 $V_k(s)$ ，它表示在所考虑的马尔可夫决策过程终止于 $k$ 个时间步的情况下，从 $s$ 可获得的最大预期效用。

等效地，这也是在 MDP 的搜索树上运行深度为 $k$ 的 expectimax 函数所返回的结果。

运行方式如下：

1.  $\forall s \in S$，初始化 $V_0(s) = 0$。这是很直观的，因为设置一个 0 步的时限意味着在终止前不能采取任何行动，因此也无法获得任何奖励。
2.  重复以下更新规则直到收敛：
    $$\forall s \in S, V_{k+1}(s) \leftarrow \max_a \sum_{s'} T(s, a, s')[R(s, a, s') + \gamma V_k(s')]$$

在价值迭代的第 $k$ 次迭代中，我们使用每个状态的时限为 $k$ 的值来生成时限为 $(k+1)$ 的值。本质上，我们使用已计算的子问题解（所有的 $V_k(s)$）来迭代地构建更大子问题的解（所有的 $V_{k+1}(s)$）；这就是价值迭代成为**动态规划算法**的原因。

**Policy Extraction**

原理：如果你处于状态 $s$ ，你应该采取动作 $a$ ，以获得最大的预期效用。
$$
\forall s \in \mathcal{S}, \pi^*(s) = \underset{a}{\text{argmax}} Q^*(s, a) = \underset{a}{\text{argmax}} \sum_{s'} T(s, a, s')[R(s, a, s') + \gamma V^*(s')]
$$

**Q-Value Iteration**

Q 值迭代是一种计算时间受限的 Q 值的动态规划算法——只能玩 N 步（比如10步），游戏必定结束。目标是在这有限的 N 步内，让总收益最大化。
$$
Q_{k+1}(s, a) \leftarrow \sum_{s'} T(s, a, s')[R(s, a, s') + \gamma \max_{a'} Q_k(s', a')]
$$

#### 4. Policy Iteration

流程：
1. 定义一个**初始策略**。这个策略可以是任意的（但是如果初始策略更接近最优策略，收敛会更快）。
2. 重复以下步骤直到收敛：
    * 通过 **策略评估**（policy evaluation）来评估当前策略。对于一个策略 \(\pi\)，策略评估意味着计算所有状态 \(s\) 的 \(V^\pi(s)\)，其中 \(V^\pi(s)\) 是指在状态 \(s\) 开始并遵循 \(\pi\) 时所期望的效用：
    $$V^\pi(s) = \sum_{s'} T(s, \pi(s), s')[R(s, \pi(s), s') + \gamma V^\pi(s')]$$
    由于我们为每个状态固定了一个单一的行动，我们不再需要 \(\max\) 运算符。
    或者，我们也可以使用以下更新规则，像价值迭代（value iteration）一样，迭代计算 \(V^{\pi_i}(s)\) 直到收敛：
    $$V_{k+1}^{\pi_i}(s) \leftarrow \sum_{s'} T(s, \pi_i(s), s')[R(s, \pi_i(s), s') + \gamma V_k^{\pi_i}(s')]$$
    然而，这第二种方法在实践中通常较慢。
    * 一旦我们评估了当前策略，就使用**策略改进**（policy improvement）来生成一个更好的策略。
    策略改进使用对策略评估产生的状态价值（values of states）进行策略提取，从而生成这个新的、改进后的策略：
    $$\pi_{i+1}(s) = \underset{a}{\text{argmax}} \sum_{s'} T(s, a, s')[R(s, a, s') + \gamma V^{\pi_i}(s')]$$
    **如果 \(\pi_{i+1} = \pi_i\)，则算法已经收敛**，我们可以得出结论：\(\pi_{i+1} =\pi_i = \pi^*\)
    

