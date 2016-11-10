---
layout: post
title: "The Pinching Trick and the Golden-Thompson Inequality"
date: 2016-11-10 11:10:24 +0100
comments: true
categories: 
---

###Pinching###

Let $A$ be a Hermitian matrix, and $A = \sum_j \lambda_j P_j$ be the spectral decomposition of $A$. 
The *pinching map* defined by $A$ is given by

$$
\mathcal{P}_A (X) = \sum_j P_j X P_j ,
$$

for any Hermitian matrix $X$.

**Theorem 1.** Let $A$ be a positive semi-definite matrix and $B$ be a Hermitian matrix. 
The following statements hold.

1. $\mathcal{P}_B (A)$ commutes with $B$.
2. $\mathrm{Tr} ( \mathcal{P}_B (A) B ) = \mathrm{Tr} ( A B )$.
3. *(Pinching inequality)* $\vert \mathrm{spec} (B) \vert \, \mathcal{P}_B (A) \geq A$, where $\mathrm{spec} (B)$ denotes the set of eigenvalues of $B$.

The first two statements are easy to check.
The earliest reference on the pinching inequality I can find is the [classic book by Jacques Dixmier](https://www.elsevier.com/books/von-neumann-algebras/dixmier/978-0-444-86308-9).
A simple proof of the pinching inequality can be found in the [textbook by Masahito Hayashi](http://www.springer.com/us/book/9783540302650).

One main issue in matrix analysis is non-commutativity. 
The first statement in Theorem 1 hints that pinching can be an useful tool to deal with this issue. 
In the next section, the pinching trick is illustrated using the Golden-Thompson inequality as an example.

###A proof of the Golden-Thompson Inequality###

The [Golden-Thompson inequality](https://en.wikipedia.org/wiki/Golden%E2%80%93Thompson_inequality) says that

$$
\mathrm{Tr} ( \exp ( A + B ) ) \leq \mathrm{Tr} ( \exp (A) \exp (B) ) , 
$$

for any two Hermitian matrices $A$ and $B$. 
Obviously, if $A$ commutes with $B$, the Golden-Thompson inequality holds with an equality; however, in general one needs to take non-commutativity into consideration.
Below we present a very elegant proof using the pinching trick from a [recent paper by D. Sutter et al](https://arxiv.org/abs/1604.03023).

The key observation is that $\vert \mathrm{spec} ( A^{\otimes n} ) \vert$ does not grow rapidly with $n$ for any Hermitian matrix $A$.

**Lemma 1.** One has $\vert \mathrm{spec} ( A^{\otimes n} ) \vert = O ( \mathsf{poly} (n) )$ for any Hermitian matrix $A$.

*Proof (Golden-Thompson inequality).*

Let $X$ and $Y$ be two positive definite matrices. 
Then one can write

$$
\log \mathrm{Tr} ( \exp ( \log X + \log Y ) ) = \frac{1}{n} \log \mathrm{Tr} ( \exp ( \log X^{\otimes n} + \log Y^{\otimes n} ) ), 
$$

for any positive integer $n$. 
By the pinching inequality, one has

$$
\frac{1}{n} \log \mathrm{Tr} ( \exp ( \log X^{\otimes n} + \log Y^{\otimes n} ) ) \leq \frac{1}{n} \log \mathrm{Tr} \{ \exp [ \log ( \vert \mathrm{spec} ( Y^{\otimes n} ) \vert \, \mathcal{P}_{Y^{\otimes n}} ( X^{\otimes n} ) ] + \log Y^{\otimes n} ) \}
$$

By the first two statements in Theorem 1 and Lemma 1, one has

$$
\begin{align}
\text{RHS} & = \frac{1}{n} \log \mathrm{Tr}\, ( \mathcal{P}_{Y^{\otimes n}} ( X^{\otimes n} ) Y^{\otimes n} ) + \frac{\log \mathsf{poly} (n)}{n} \notag \\
& = \frac{1}{n} \log \mathrm{Tr} ( X^{\otimes n} Y^{\otimes n} ) + \frac{\log \mathsf{poly} (n)}{n} \notag \\
& = \log \mathrm{Tr}\, ( X Y ) + \frac{\log \mathsf{poly} (n)}{n} . 
\end{align}
$$

Then one obtains the Golden-Thompson Inequality by letting $n \to \infty$.
