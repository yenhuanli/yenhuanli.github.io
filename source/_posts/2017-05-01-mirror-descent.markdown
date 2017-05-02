---
layout: post
title: "Minimizing Strongly Convex Functions by Mirror Descent"
date: 2017-05-01 14:38:13 +0200
comments: true
categories: 
---

Mirror descent is an algorithm to numerically solve convex optimization problems, very popular in machine learning.
Surprisingly, there are very few existing results on mirror descent applied to strongly convex objective functions. 
Below is a result I found in [a paper by Juditsky and Nemirovski](http://www2.isye.gatech.edu/~nemirovs/MLOptChapterI.pdf).

### Mirror Descent

Consider the convex optimization problem

$$
f^\star = \min \lbrace f ( x ) | x \in \mathcal{X} \rbrace, 
$$

where $f$ is a real-valued Lipschitz continuous convex function, and $\mathcal{X}$ is a closed convex set in $\mathbb{R}^d$.

To define mirror descent, we need the notion of a *distance generating function (DGF)*.
Fix a norm $\Vert \cdot \Vert$ on $\mathbb{R}^d$. 
A DGF $\omega$ is a real-valued convex function continuously differentiable on $\mathcal{X}$, such that for all $x, y \in \mathcal{X}$, 

$$
\omega ( y ) \geq \omega ( x ) + \langle \omega' ( x ), y - x \rangle + \frac{1}{2} \Vert y - x \Vert^2 . 
$$

Fix a DGF $\omega$.
The corresponding *Bregman divergence* is given by

$$
D_\omega ( y, x ) := \omega ( y ) - \omega ( x ) - \langle \omega' ( x ) , y - x \rangle . 
$$

The corresponding *prox-mapping* is given by

$$
\mathrm{prox}_\omega ( \xi; x ) := \mathrm{arg\, max} \lbrace \langle \xi, u \rangle + D_\omega ( u, x ) \vert u \in \mathcal{X} \rbrace . 
$$

Mirror descent iterates as follows. 

* Choose $x_1 \in \mathcal{X}$.
* Let $( \gamma_1, \gamma_2, \ldots )$ be a given sequence of positive numbers. 
* For $t = 1, 2, \ldots$, compute 

$$
x_{t+1} = \mathrm{prox}_\omega ( \gamma_t f' ( x_t ) ; x_{t} ) . 
$$

* Set $x^t$ to be the weighted average of $x_1, \ldots, x_t$ as

$$
x^t := \frac{\sum_{\tau = 1}^t \gamma_\tau x_\tau}{\sum_{\tau = 1}^t \gamma_\tau} . 
$$

**Example.** The most well-known example of mirror descent might be the following one.
Suppose that $\mathcal{X}$ is the probability simplex in $\mathbb{R}^d$. 
Choose $\omega ( x ) = \sum_{i = 1}^d x_i \log x_i - \sum_{i = 1}^d x_i$, which, by Pinsker's inequality, is strongly convex with respect to the $\ell_1$-norm.
Then $x_{t + 1}$ has a closed-form: 

$$
( x_{t + 1} )_i = \frac{ ( x_t )_i \mathrm{e}^{- \gamma_\tau^{-1} ( f' ( x_t ) )_i} }{ \sum_{j = 1}^d \mathrm{e}^{- \gamma_\tau^{-1} ( f' ( x_t ) )_j} } ,  \quad i = 1, 2, \ldots, d , 
$$

where $( v )_i$ denotes the $i$-th element of any vector $v$.
Notice that unlike projected gradient descent, mirror descent does not need to compute projections onto $\mathcal{X}$. 

### Minimizing Lipschitz Continuous Functions by Mirror Descent

It is well-known that mirror descent achieves $O ( 1 / \sqrt{k} )$ convergence rate, as long as $f$ is Lipschitz continuous (and, of course, convex).

**Proposition.** For any $u \in \mathcal{X}$, 

$$
f ( x^t ) - f ( u ) \leq \frac{ D ( u, x_1 ) + \frac{1}{2} \sum_{\tau = 1}^t \gamma_\tau^2 \Vert f' ( x_\tau ) \Vert_*^2 }{ \sum_{\tau = 1}^t \gamma_\tau } . 
$$

*Proof.* By convexity of $f$, we have

$$
f ( x^t ) - f ( u ) \leq \frac{1}{ \sum_{\tau = 1}^t \gamma_\tau } \sum_{\tau = 1}^t \gamma_\tau f ( x_\tau ) - f ( u ) \leq \frac{1}{ \sum_{\tau = 1}^t \gamma_\tau } \sum_{\tau = 1}^t \gamma_\tau \langle f' ( x_\tau ) , x_\tau - u \rangle . 
$$

Now we derive an upper bound of $\gamma_\tau \langle f' ( x_\tau ), x_\tau - u \rangle$.
The optimality condition says

$$
\langle \gamma_\tau f' ( x_\tau ) + \omega'( x_{\tau + 1} ) - \omega' ( x_\tau ) , u - x_{\tau + 1} \rangle \geq 0 , 
$$

from which we obtain

$$
\gamma_\tau \langle f' ( x_\tau ), x_\tau - u \rangle \leq \gamma_\tau \langle f' ( x_\tau ), x_\tau - x_{\tau + 1} \rangle + \langle \omega' ( x_{\tau + 1} ) - \omega' ( x_\tau ) , u - x_{\tau + 1} \rangle . 
$$

Applying the *three-point equality* (which is easy to check by direct calculation), we obtain

$$
\gamma_\tau \langle f' ( x_\tau ), x_\tau - u \rangle \leq \gamma_\tau \langle f' ( x_\tau ), x_\tau - x_{\tau + 1} \rangle + D_\omega ( u, x_\tau ) - D_\omega ( u, x_{\tau + 1} ) - D_\omega ( x_{\tau + 1}, x_\tau ) . 
$$

By the strong convexity of $\omega$, we have

$$
\gamma_\tau \langle f' ( x_\tau ), x_\tau - x_{\tau + 1} \rangle - D_\omega ( x_{\tau + 1}, x_\tau ) \leq \gamma_\tau \Vert f' ( x_\tau ) \Vert_* \Vert x_\tau - x_{\tau + 1} \Vert - \frac{1}{2} \Vert x_\tau - x_{\tau + 1} \Vert^2 \leq \frac{\gamma_\tau^2}{2} \Vert f' ( x_\tau ) \Vert_*^2 , 
$$

where $\Vert \cdot \Vert_*$ denotes the norm dual to $\Vert \cdot \Vert$.
We now have

$$
\gamma_\tau \langle f' ( x_\tau ), x_\tau - u \rangle \leq D_\omega ( u, x_\tau ) - D_\omega ( u, x_{\tau + 1} ) + \frac{\gamma_\tau^2}{2} \Vert f' ( x_\tau ) \Vert_*^2 . 
$$

Summing over all $\tau$, the proposition follows. 
*Q.E.D.*

Notice that $\Vert f' ( x_t ) \Vert_* \leq L$ for all $t$.
The following corollary is obvious.

**Corollary.** Set 

$$
\gamma_t = \frac{\gamma}{\Vert f' ( x_t ) \Vert_* \sqrt{t}} , 
$$

for some $\gamma > 0$. Then it holds that, for any $u \in \mathcal{X}$, 

$$
f ( x^t ) - f ( u ) \leq \frac{1}{2 L \sqrt{t}} \left( \frac{D ( u, x_1 )}{\gamma} + \frac{\gamma}{2} \log ( t + 1 ) \right) . 
$$