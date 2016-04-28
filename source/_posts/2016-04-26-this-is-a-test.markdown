---
layout: post
title: "This is a test"
date: 2016-04-26 23:25:33 +0200
comments: true
categories: 
---
### Hello world! ##
Hello world!

### Empirical processes ##
For a *smart enough* guy, studying empirical processes only requires knowing two fundamental results. 

1.  **Markov's inequality:** Let $X$ be a non-negative random variable. Markov's inequality says that for any $t > 0$, we have $\mathsf{P} \left( X \geq t \right) \leq t^{-1} \mathsf{E}\, X$. By Markov's inequality, it is not difficult to see the Cramer-Chernoff theorem, and then it should be *natural* to rediscover the [entropy method](http://www.ams.org/books/surv/089/){:target="_blank"}.
2.  **Union bound:** Let $\mathcal{E}_1$ and $\mathcal{E}_2$ be two events. The union bound says that $\mathsf{P} ( \mathcal{E}_1 \cup \mathcal{E}_2 ) \leq \mathsf{P} ( \mathcal{E}_1 ) + \mathsf{P} ( \mathcal{E}_2 )$. Applying the union bound in a *smart enough* way, one can immediately rediscover the [generic chaining argument](http://www.springer.com/us/book/9783642540745){:target="_blank"}.

Unfortunately it seems that I'm not smart enough :D

### The PAC learning framework ###
The standard formulation of a learning problem consists of four ingredients: 

1.  **Training data:** The training data is given by a sequence of i.i.d. random variables $Z_i = ( X_i, Y_i ) \in \mathcal{Z} = \mathcal{X} \times \mathcal{Y}$, each of which follows certain *unknown* probability distribution $\mathbb{P}$. For convenience, let us define $\mathcal{D}_n = \lbrace Z_i : 1 \leq i \leq n \rbrace$.
2.  **Hypothesis class:** A hypothesis class is a set $\mathcal{H}$ of functions $h: \mathcal{X} \to \mathbb{Y}$.
3.  **Loss function:** A loss function is a function $f: \mathcal{H} \times \mathcal{Z} \to \mathbb{R}$.
4.  **Risk:** Let $Z$ be an independent copy of $Z_1$. The risk is a function $F: \mathcal{H} \to \mathbb{R}$ defined as $F ( h ) = \mathsf{E}\, f ( h, Z )$.

The goal of learning is to find a *good* hypothesis $\hat{h}_n \in \mathcal{H}$, based on $\mathcal{D}_n$, such that the resulting risk $F ( \hat{h}_n )$ is small. While at first glance we only need to solve the optimization problem 
    $$h^\star \in \arg \min_{h \in \mathcal{H}} F ( h )$$, 
since the probability distribution $\mathbb{P}$ is assumed unknown, $F$ is not explicitly specified and we cannot directly solve this optimization problem.

One example of a learning problem is binary classification, in which each $X_i$ may represent an image, each $Y_i \in \mathcal{Y} = \lbrace -1, 1 \rbrace$ may indicate whether there is a cat in $X_i$ or not, the hypothesis class is a set of *classifiers* taking values in $\mathcal{Y}$, and the loss function is given by $f ( h, z ) = \iota_{\lbrace h(x) \neq y\rbrace}$, where $\iota$ denotes the indicator function. It is easy to check that then the risk is the probabiity of false classification.

We say that a hypothesis class $\mathcal{H}$ is **agnostic probably approximately correctly (PAC) learnable**, if there exists an algorithm $\mathcal{A}: \mathcal{Z}^n \to \mathcal{H}$ and a function $n_{\mathcal{H}} ( \varepsilon, \delta )$, such that for every probability distribution $\mathbb{P}$ and every $\varepsilon, \delta \in ( 0, 1 )$, we have 
$$
\begin{equation}
F ( \mathcal{A} ( \mathcal{D}_n ) ) - \inf_{h \in \mathcal{H}} F ( h ) \leq \varepsilon 
\end{equation}
$$
with probability at least $1 - \delta$ whenever $n \geq n_{\mathcal{H}} ( \varepsilon, \delta )$.
