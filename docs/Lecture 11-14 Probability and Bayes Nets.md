---
author: Hongyi Chen
date: today
title: Lecture 11-14 Probability and Bayes Nets
---

## CS 180 - Introduction to AI

### Lecture 11-14: Probability and Bayes Nets

#### 1. Probability Recap

* **Conditional probability**
    $$P(x|y) = \frac{P(x, y)}{P(y)}$$

* **Product rule**
    $$P(x, y) = P(x|y)P(y)$$

* **Chain rule**
    $$P(X_1, X_2, \dots, X_n) = P(X_1)P(X_2|X_1)P(X_3|X_1, X_2)\dots$$
    $$= \prod_{i=1}^{n} P(X_i|X_1, \dots, X_{i-1})$$

* **X, Y independent if and only if:**
    $$\forall x, y : P(x, y) = P(x)P(y)$$

* **X and Y are conditionally independent given Z if and only if:**
    $$\forall x, y, z : P(x, y|z) = P(x|z)P(y|z) \qquad X \perp Y|Z$$


#### 2. Bayesian Network Representation

E.g.考虑一个模型，其中我们有如下五个二进制随机变量：

* **B:** Burglary occurs.
* **A:** Alarm goes off.
* **E:** Earthquake occurs.
* **J:** John calls.
* **M:** Mary calls.


![Bayes Net Example](./media/BayesNet%20example.png)

贝叶斯网络图的结构编码了不同节点之间的条件独立关系。

贝叶斯网络节点之间的边并不意味着这些节点之间存在特定的因果关系，也不意味着变量之间必然相互依赖。它只是意味着节点之间可能存在某种关系。

