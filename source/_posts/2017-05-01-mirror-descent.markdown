---
layout: post
title: "Minimizing Strongly Convex Functions by Mirror Descent"
date: 2017-05-01 14:38:13 +0200
comments: true
categories: 
---

Mirror descent is an algorithm to numerically solve convex optimization problems, very popular in machine learning.
Surprisingly, there are very few existing results on mirror descent applied to strongly convex objective functions. 
This post summarizes one such result, which I found in [a review paper by Juditsky and Nemirovski](http://www2.isye.gatech.edu/~nemirovs/MLOptChapterI.pdf).

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
* Let $( \eta_1, \eta_2, \ldots )$ be a given sequence of positive numbers. 
* For $t = 1, 2, \ldots$, compute 

$$
x_{t+1} = \mathrm{prox}_\omega ( \eta_t f' ( x_t ) ; x_{t} ) . 
$$

* Set $x^t$ to be the weighted average of $x_1, \ldots, x_t$ as

$$
x^t := \frac{\sum_{\tau = 1}^t \eta_\tau x_\tau}{\sum_{\tau = 1}^t \eta_\tau} . 
$$

Notice that $f$ is not necessarily differentiable; $f' (x)$ is understood as a subgradient of $f$ at $x$.

**Example.** The following might be the most well-known example.
Suppose that $\mathcal{X}$ is the probability simplex in $\mathbb{R}^d$. 
Choose $\omega ( x ) = \sum_{i = 1}^d x_i \log x_i - \sum_{i = 1}^d x_i$, which, by Pinsker's inequality, is strongly convex with respect to the $\ell_1$-norm.
Then $x_{t + 1}$ has a closed-form: 

$$
( x_{t + 1} )_i = \frac{ ( x_t )_i \mathrm{e}^{- \eta_t ( f' ( x_t ) )_i} }{ \sum_{j = 1}^d ( x_t )_j \mathrm{e}^{- \eta_t ( f' ( x_t ) )_j} } ,  \quad i = 1, 2, \ldots, d , 
$$

where $( v )_i$ denotes the $i$-th element of any vector $v$.

**Example.** Notice that choosing $\omega$ to be the $\ell_2$-norm is always valid. 
In this case, we recover the [projected subgradient method](https://en.wikipedia.org/wiki/Subgradient_method#Projected_subgradient).

### Minimizing Lipschitz Continuous Functions

It is well-known that mirror descent achieves $O ( 1 / \sqrt{k} )$ convergence rate, as long as $f$ is Lipschitz continuous (and, of course, convex).
Below is the proof given in [a classic paper by Beck and Teboulle](https://doi.org/10.1016/S0167-6377(02)00231-6). 

**Proposition.** For any $u \in \mathcal{X}$, 

$$
f ( x^t ) - f ( u ) \leq \frac{ D ( u, x_1 ) + \frac{1}{2} \sum_{\tau = 1}^t \eta_\tau^2 \Vert f' ( x_\tau ) \Vert_*^2 }{ \sum_{\tau = 1}^t \eta_\tau } . 
$$

*Proof.* By convexity of $f$, we have

$$
f ( x^t ) - f ( u ) \leq \frac{1}{ \sum_{\tau = 1}^t \eta_\tau } \sum_{\tau = 1}^t \eta_\tau \left( f ( x_\tau ) - f ( u ) \right) \leq \frac{1}{ \sum_{\tau = 1}^t \eta_\tau } \sum_{\tau = 1}^t \eta_\tau \langle f' ( x_\tau ) , x_\tau - u \rangle . 
$$

Now we derive an upper bound of $\eta_\tau \langle f' ( x_\tau ), x_\tau - u \rangle$.
The optimality condition says

$$
\langle \eta_\tau f' ( x_\tau ) + \omega'( x_{\tau + 1} ) - \omega' ( x_\tau ) , u - x_{\tau + 1} \rangle \geq 0 , 
$$

from which we obtain

$$
\eta_\tau \langle f' ( x_\tau ), x_\tau - u \rangle \leq \eta_\tau \langle f' ( x_\tau ), x_\tau - x_{\tau + 1} \rangle + \langle \omega' ( x_{\tau + 1} ) - \omega' ( x_\tau ) , u - x_{\tau + 1} \rangle . 
$$

Applying the *three-point equality* (which is easy to check by direct calculation), we obtain

$$
\eta_\tau \langle f' ( x_\tau ), x_\tau - u \rangle \leq \eta_\tau \langle f' ( x_\tau ), x_\tau - x_{\tau + 1} \rangle + D_\omega ( u, x_\tau ) - D_\omega ( u, x_{\tau + 1} ) - D_\omega ( x_{\tau + 1}, x_\tau ) . 
$$

By the strong convexity of $\omega$, we write

$$
\eta_\tau \langle f' ( x_\tau ), x_\tau - x_{\tau + 1} \rangle - D_\omega ( x_{\tau + 1}, x_\tau ) \leq \eta_\tau \Vert f' ( x_\tau ) \Vert_* \Vert x_\tau - x_{\tau + 1} \Vert - \frac{1}{2} \Vert x_\tau - x_{\tau + 1} \Vert^2 \leq \frac{\eta_\tau^2}{2} \Vert f' ( x_\tau ) \Vert_*^2 , 
$$

where $\Vert \cdot \Vert_*$ denotes the norm dual to $\Vert \cdot \Vert$.
Now we have

$$
\eta_\tau \langle f' ( x_\tau ), x_\tau - u \rangle \leq D_\omega ( u, x_\tau ) - D_\omega ( u, x_{\tau + 1} ) + \frac{\eta_\tau^2}{2} \Vert f' ( x_\tau ) \Vert_*^2 . 
$$

Summing over all $\tau$, the proposition follows. 
*Q.E.D.*

Notice that $\Vert f' ( x_t ) \Vert_* \leq L$ for all $t$.
The following corollary is obvious.

**Corollary.** Set 

$$
\eta_t = \frac{\eta}{\Vert f' ( x_t ) \Vert_* \sqrt{t}} , 
$$

for some $\eta > 0$. Then it holds that, for any $u \in \mathcal{X}$, 

$$
f ( x^t ) - f ( u ) \leq \frac{L}{2 \sqrt{t}} \left[ \frac{D ( u, x_1 )}{\eta} + \frac{\eta}{2} \left( \log ( t ) + 1 \right) \right] . 
$$

The following corollary is technically more involved, and can be found in, e.g., [the paper by Beck and Teboulle](https://doi.org/10.1016/S0167-6377(02)00231-6). 

**Corollary.** Suppose that

$$
\Omega := \max \lbrace D_\omega ( u, x_1 ) | u \in \mathcal{X} \rbrace 
$$

is finite.
Set 

$$
\eta_t = \frac{\sqrt{2 \Omega}}{ L \sqrt{t} } . 
$$

Then it holds that, for any $u \in \mathcal{X}$, 

$$
f ( x^t ) - f ( u ) \leq L \sqrt{ \frac{2 \Omega}{t} } . 
$$

### Minimizing Lipschitz Continuous and Strongly Convex Functions

When the objective function is not only Lipschitz continuous but also strongly convex, it is reasonable to expect a convergence rate better than $O ( 1 / \sqrt{t} )$. 
The proof below, given in [a review paper by Juditsky and Nemirovski](http://www2.isye.gatech.edu/~nemirovs/MLOptChapterI.pdf), shows that then $O ( 1 / t )$ can be achieved. 

Let $\omega$ be a DGF for $\mathbb{R}^d$ (instead of merely $\mathcal{X}$!), with respect to a norm $\Vert \cdot \Vert$.
For any $R > 0$, define $\omega_R ( \cdot ) := \omega ( R^{-1} \cdot )$; then $\omega_R$ is $1$-strongly convex with respect to $\Vert \cdot \Vert_R := R^{-1} \Vert \cdot \Vert$.
The corresponding Bregman divergence is given by

$$
D_{\omega, R} ( y, x ) := \omega_R ( y ) - \omega_R ( x ) - \langle \omega_R' ( x ), y - x \rangle. 
$$

The prox-mapping is given by

$$
\mathrm{prox}_{\omega, R} ( \xi; x ) := \mathrm{arg\, min} \lbrace \langle \xi, u \rangle + D_{\omega, R} ( u, x ) | u \in \mathcal{X} \rbrace . 
$$

Fix $x_1 \in \mathcal{X}$.
For any given sequence $(\eta_1, \eta_2, \ldots)$, define

$$
x_{t + 1} = \mathrm{prox}_{\omega, R} ( \eta_t f' ( x_t ); x_t ), \quad t = 1, 2, \ldots , 
$$

and 

$$
x^t ( R, x_1 ) := \frac{\sum_{\tau = 1}^t \eta_\tau x_\tau}{\sum_{\tau = 1}^t \eta_\tau} . 
$$

**Proposition.** Suppose that $f$ is $L$-Lipschitz continuous and $\mu$-strongly convex on $\mathcal{X}$.
Suppose that

$$
\Omega := \max \lbrace D_\omega ( u, x_1 ) | u \in \mathcal{X} \rbrace 
$$

is finite.
Set $R$ such that $R \geq \Vert x_1 - x^\star \Vert$, where $x^\star$ denotes the minimizer of $f$ on $\mathcal{X}$. 
Set 

$$
\eta_\tau = \frac{\sqrt{2 \Omega}}{R L \sqrt{t}}, \quad \tau = 1, 2, \ldots, t . 
$$

Then it holds that

$$
f ( x^t (R, x_1) ) - f ( x^\star ) \leq \frac{R L \sqrt{2 \Omega}}{\sqrt{t}}, \quad \Vert x^t (R, x_1) - x^\star \Vert^2 \leq \frac{2 R L \sqrt{ 2 \Omega }}{\mu \sqrt{t}} . 
$$

*Proof.* Following the proof in the previous sectioin (with the norm $\Vert \cdot \Vert_R$), we have

$$
f ( x^t (R, x_1) ) - f ( x^\star ) \leq \frac{ \Omega + \frac{R^2 L^2}{2} \sum_{\tau = 1}^t \eta_\tau^2 }{ \sum_{\tau = 1}^t \eta_\tau } , 
$$

which implies the first inequality with the specific choice of $( \eta_1, \eta_2, \ldots, \eta_t )$.
The second inequality follows from the strong convexity of $f$.
*Q.E.D.*

Notice that the convergence speed depends on $R$, the initial distance between $x_1$ and $x^\star$.
When the current iterate is close enough to $x^\star$, which is guaranteed to happen with a large enough $t$, we can *restart* with a smaller value of $R$, in order to get a better convergence speed.

Consider the following algorithm.
Choose $y_0 \in \mathcal{X}$.
Set $R_0$ such that $R_0 \geq \Vert y_0 - x^\star \Vert$.
For $k = 1, 2, \ldots$, 

1. Set $N_k = \left\lceil \frac{ 8 L^2 \Omega}{\mu^2 R_{k - 1}^2} \right\rceil$. 
2. Compute $y_k = x^{N_k} ( R_{k - 1}, y_{k - 1} )$, with $\eta_\tau = \frac{\sqrt{2 \Omega}}{R_{k - 1} L \sqrt{N_k}}$ for $\tau = 1, 2, \ldots, N_k$. 
3. Set $R_k^2 = 2^{-1} R_{k - 1}^2$.
