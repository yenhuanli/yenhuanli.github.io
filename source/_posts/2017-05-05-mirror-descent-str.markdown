---
layout: post
title: "Minimizing a Strongly Convex Function by Mirror Descent"
date: 2017-05-05 15:45:42 +0200
comments: true
categories: 
---

Mirror descent (MD) is a famous convex optimization algorithm. 
When the objective function is Lipschitz continuous, the convergence rate of MD is known to be $O ( t^{-1/2} )$. 
When the objective function is also strongly convex, intuitively, a better convergence rate should be possible.
Suprisingly, there are very few related results in existing literature, to the best of my knowledge. 
This post summarizes one such result in a [review article by Juditsky and Nemirovski](http://www2.isye.gatech.edu/~nemirovs/MLOptChapterI.pdf).

We will consider a standard constrained convex optimization problem: 

$$
f^\star = \mathrm{arg\, min} \left\lbrace f ( x ) | x \in \mathcal{X} \right\rbrace , 
$$

where $f$ is a convex function, and $\mathcal{X}$ is a compact convex set in $\mathbb{R}^d$. 
We assume that $f$ is $L$-Lipschitz continuous w.r.t. some norm $\Vert \cdot \Vert$ on $\mathbb{R}^d$. 

### Brief Review of Mirror Descent

Let $\omega$ be a continuously differentiable $1$-strongly convex function on $\mathcal{X}$, w.r.t. $\Vert \cdot \Vert$.
Define the corresponding *Bregman divergence*

$$
D ( y, x ) := \omega ( y ) - \left[ \omega ( x ) + \left\langle \omega' ( x ), y - x \right\rangle \right] , 
$$

and *prox-mapping* 

$$
\mathrm{prox} ( \xi; x ) := \mathrm{arg\, min} \left\lbrace \left\langle \xi, u \right\rangle + D ( u, x ) \, \middle| \, x \in \mathcal{X} \right\rbrace . 
$$

Let $x_1 \in \mathcal{X}$, and $( \eta_1, \eta_2, \ldots )$ be a sequence of *step sizes*. 
The *standard* MD iterates as follows. 

$$
x_{t + 1} = \mathrm{prox} ( \eta_t f' ( x_t ) ; x_t ) , \quad t = 1, 2, \ldots .
$$

Define 

$$
x^t := \frac{ \sum_{\tau = 1}^t \eta_\tau x_\tau }{ \sum_{\tau = 1}^t \eta_\tau } . 
$$

The following convergence guarantee can be found in, e.g., [the classic paper by Beck and Teboulle](https://web.iem.technion.ac.il/images/user-files/becka/papers/3.pdf).

**Proposition 1.** Fix a positive integer $T$.
Set 

$$
\Omega \geq \max \left\lbrace D ( u, x_1 ) \, \middle| \, u \in \mathcal{X} \right\rbrace , \quad \eta_t = \frac{\sqrt{2 \Omega }}{ L \sqrt{T}} \,\, \text{ for } t = 1, 2, \ldots, T . 
$$

Then 

$$
f ( x^T ) - f^\star \leq \frac{L \sqrt{2 \Omega}}{\sqrt{T}} . 
$$

### A Modified MD for Minimizing Strongly Convex Functions

Now assume that $f$ is also $\mu$-strongly convex w.r.t. $\Vert \cdot \Vert$.
How do we exploit this additional information? 

Let us choose $\omega$ such that it is strongly convex on the whole $\mathbb{R}^d$, *instead of merely $\mathcal{X}$*. 
For any $R > 0$, define $\omega_R ( x ) := \omega ( R^{-1} x )$; denote by $D_R$ the corresponding Bregman divergence, and $\mathrm{prox}_R$ the corresponding prox-mapping.

Let us define an iteration rule very similar to the standard MD; the only difference is that now we replace $\omega$ by $\omega_R$: 
Let $( \eta_1, \eta_2, \ldots )$ be a given sequence of step sizes.
For any $x_1 \in \mathcal{X}$, we compute 

$$
x_{t + 1} = \mathrm{prox}_R ( \eta_t f' ( x_t ) ; x_t ) , \quad t = 1, 2, \ldots .
$$

Define

$$
x^t ( R, x_1 ) := \frac{ \sum_{\tau = 1}^t \eta_\tau x_\tau }{ \sum_{\tau = 1}^t \eta_\tau } .
$$

**Proposition 2.** Fix a positive integer $T$.
Set 

$$
\Omega \geq \max \left\lbrace D ( u, x_1 ) \, \middle| \, u \in \mathcal{X} \right\rbrace , \quad R_0 \geq \max \left\lbrace \left\Vert u - x^\star \right\Vert \, \middle| \, u \in \mathcal{X} \right\rbrace , \quad \eta_t = \frac{\sqrt{2 \Omega }}{ R_0 L \sqrt{T}} \,\, \text{ for } t = 1, 2, \ldots, T . 
$$

Then 

$$
f ( x^T ( R_0, x_1 ) ) - f^\star \leq \frac{R_0 L \sqrt{2 \Omega}}{\sqrt{T}}, \quad \left\Vert x^T ( R_0, x_1 ) - x^\star \right\Vert^2 \leq \frac{R_0 L \sqrt{8 \Omega}}{\mu \sqrt{T}} , 
$$

where $x^\star$ is the unique minimizer of $f$ on $\mathcal{X}$.

*Proof.*
Notice that $\omega_R$ is $1$-strongly convex w.r.t. $\Vert \cdot \Vert_R := R^{-1} \Vert \cdot \Vert$. 
The first inequality follows from Proposition 1, using the norm $\Vert \cdot \Vert_R$. 
The second inequality follows from the following two inequalities, obtained by the strong convexity of $f$ and the optimality of $x^\star$, respectively: 

$$
f ( x^T ) - f^\star \geq \left\langle f' ( x^\star ) , x^T - x^\star \right\rangle + \frac{\mu}{2} \Vert x^T - x^\star \Vert^2, \quad \left\langle f' ( x^\star ), x^T - x^\star \right\rangle \geq 0 .
$$

*Q.E.D.*

Notice that now we have an error bound depending on $R_0$; the smaller $R_0$ is, the smaller the error bounds are.
Also notice that the bound of the distance between $x^T$ and $x^\star$ is strictly decreasing with $T$. 
These observations motivate the following *restarting* strategy: 
Set $y_0 \in \mathcal{X}$. 
For $k = 1, 2, \ldots$, 

1. Set $T_k$ such that $\left\Vert x^{T_k} ( R_{k - 1}, y_{k - 1} ) - x^\star \right\Vert^2 \leq 2^{-1} R_{k - 1}^2$. 
2. Compute $y_k = x^{T_k} ( R_{k - 1}, y_{k - 1} )$, with $\eta_t = \frac{\sqrt{2 \Omega }}{ R L \sqrt{T_k}}$ for all $t$.
3. Set $R_k^2 = 2^{-1} R_{k - 1}^2$. 

By the proposition we have just proved, it suffices to choose

$$
T_k = \left\lceil \frac{32 L^2 \Omega}{\mu^2 R_{k - 1}^2} \right\rceil . 
$$

**Proposition 3.** Define $M_k = \sum_{\kappa = 1}^k T_{\kappa}$. 
Let $k^\star$ be the smallest $k$ such that

$$
k \leq \frac{L^2 \Omega}{\mu^2 R_0^2} 2^{k + 5} . 
$$

Then for $k < k^\star$, 

$$
f ( y_k ) - f^\star \leq 2^{-(0.5 M_k + 1)} \mu R_0^2, \quad \left\Vert y_k - x^\star \right\Vert^2 \leq 2^{- 0.5 M_k} R_0^2 ; 
$$

for $k \geq k^\star$, 

$$
f ( y_k ) - f^\star \leq \frac{32 L^2 \Omega}{\mu M_k}, \quad \left\Vert y_k - x^\star \right\Vert^2 \leq \frac{64 L^2 \Omega}{\mu^2 M_k} .
$$

*Proof.* It is easily checked, by Proposition 2, that

$$
f ( y_k ) - f^\star \leq 2^{-(k + 1)} \mu R_0^2, \quad \left\Vert y_k - x^\star \right\Vert^2 \leq 2^{-k} R_0^2 . 
$$

By our choice of $T_k$, it holds that

$$
M_k \leq k + \sum_{\kappa = 1}^k \frac{32 L^2 \Omega}{\mu^2 R_{\kappa - 1}^2} = k + \sum_{\kappa = 1}^k \frac{32 L^2 \Omega}{\mu^2 2^{-(\kappa - 1)} R_0^2} \leq k + \frac{L^2 \Omega}{\mu^2 R_0^2} 2^{k + 5} . 
$$

Therefore, $M_k \leq 2 k$ for $k < k^\star$, and 

$$
M_k \leq \frac{L^2 \Omega}{\mu^2 R_0^2} 2^{k + 6} 
$$

for $k \geq k^\star$. 
*Q.E.D.*

Notice that to compute each $y^k$, we need to compute $T_k$ additional prox-mappings.
Therefore, the effective number of iterations is represented by $M_k$, instead of $k$, and the convergence rate is understood as

$$
O ( 1 / ( \text{effective number iterations} ) ) .
$$
