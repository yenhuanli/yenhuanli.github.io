---
layout: post
title: "Minimizing Exp-Concave Functions by Mirror Descent"
date: 2017-05-08 17:15:34 +0200
comments: true
categories: 
---

A real-valued function $f$ is called *exp-concave*, if $x \mapsto \mathrm{e}^{-f ( x )}$ is concave.
Let us fix a exp-concave function $f$ throughout this post.
Assume that $f$ is well-defined and continuously differentiable on some convex compact set $\mathcal{X} \subset \mathbb{R}^d$. 

### Brief Introduction to Exp-Concave Functions

Let us study what exp-concavity can bring us first.
Obviously, $f$ is convex, as $x \mapsto \mathrm{e}^{-x}$ is "very convex"; therefore, there is a lower bound of the function value: 

$$
f ( y ) \geq f ( x ) + \left\langle f' ( x ) , y - x \right\rangle, \quad \text{for all } x, y \in \mathcal{X} . 
$$

In fact, a tighter bound can be obtained. 

**Lemma 1.** It holds that

$$
f ( y ) \geq f ( x ) - \log \left( 1 - \left\langle f' ( x ), y - x \right\rangle \right), \quad \text{for all } x, y \in \mathcal{X} . 
$$

*Proof.* Define $g: x \mapsto \mathrm{e}^{-f (x)}$. 
The lemma follows from the inequality

$$
g ( y ) \geq g ( x ) + \left\langle g' ( x ), y - x \right\rangle , \quad \text{for all } x, y \in \mathcal{X} , 
$$

due to the concavity of $g$.

By a second-order approximation of the logarithmic function, [Hazan et al.](https://link.springer.com/article/10.1007%2Fs10994-007-5016-8?LI=true) proved the following inequality, as a corollary of Lemma 1.

**Corollary 2.** There exists some $\beta > 0$, such that 

$$
f ( y ) \geq f ( x ) + \left\langle f' ( x ), y - x \right\rangle + \frac{\beta}{2} \left\langle \nabla f' ( x ), y - x \right\rangle^2 , \quad \text{for all } x, y \in \mathcal{X} . 
$$

If we view $f' ( x ) ^{\mathrm{T}} f' ( x )$ as an approximation of $f'' ( x )$, Corollary 2 is very similar to the inequality for *strongly convex* functions.
Indeed, [Hazan et al.](https://link.springer.com/article/10.1007%2Fs10994-007-5016-8?LI=true) showed that both strongly convex functions and exp-concave functions allow logarithmic regret algorithms for on-line convex optimization.
This result, together with some other existing results about exp-concavity, hints a naive insight.

> Somehow exp-concave functions are as easy for convex optimization as strongly convex functions.

The statement is of course not precise at all, but is quite useful when you are asked to make some "educated guess" to questions about exp-concavity.

Finally, there is an interesting fact about exp-concavity: A self-concordant function is a self-concordant barrier, if and only if it is exp-concave (see, e.g., [this classic](http://dx.doi.org/10.1137/1.9781611970791)).
Unfortunately, I have not observed any deep application of this fact.

### Minimizing Exp-Concave Functions

Consider the convex optimization problem: 

$$
f^\star = \mathrm{arg\, min} \left\lbrace f ( x ) \, \middle| \, x \in \mathcal{X} \right\rbrace .
$$

Let $\Vert \cdot \Vert$ be a norm on $\mathbb{R}^d$.
Let $\omega : \mathbb{R}^d \to \mathbb{R}$ be a continuously differentiable function, strongly convex w.r.t. $\Vert \cdot \Vert$ on $\mathbb{R}^d$, i.e., 

$$
\left\langle \omega' ( y ) - \omega' ( x ), y - x \right\rangle \geq \left\Vert y - x \right\Vert^2 , \quad \text{for all } x, y \in \mathbb{R}^d. 
$$

For any $R > 0$, define $\omega_R ( x ) := \omega ( R^{-1} x )$.
Define the corresponding *Bregman divergence* and *prox-mapping*: 

$$
D_R ( y, x ) := \omega_R ( y ) - \left[ \omega_R ( x ) + \left\langle \omega_R' ( x ), y - x \right\rangle \right] , \quad \mathrm{prox}_R ( \xi; x ) := \mathrm{arg\, min} \left\lbrace \left\langle \xi, u \right\rangle + D_R ( u, x ) \, \middle| \, u \in \mathcal{X} \right\rbrace . 
$$

Notice that $\omega_R$ is strongly convex w.r.t. $\Vert \cdot \Vert_R := \Vert R^{-1} \cdot \Vert$.
