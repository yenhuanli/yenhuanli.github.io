---
layout: post
title: "Some Interesting Talks at NIPS 2016"
date: 2016-12-12 22:57:59 +0100
comments: true
categories:
---

NIPS 2016 was quite successful, in the sense that most of the papers interesting to me were chosen as oral presentations:)
The list below is of course non-exhaustive and biased.
The order is alphabetical, according to the last names of the presenters/first authors.

* "Kernel-based Methods for Bandit Convex Optimization" by S. Bubeck
    * Bubeck wrote a blog article on this paper: [part 1](https://blogs.princeton.edu/imabandit/2016/08/06/kernel-based-methods-for-bandit-convex-optimization-part-1/), [part 2](https://blogs.princeton.edu/imabandit/2016/08/09/kernel-based-methods-for-convex-bandits-part-2/), and [part 3](https://blogs.princeton.edu/imabandit/2016/08/10/kernel-based-methods-for-convex-bandits-part-3/).
    * It seems that Bubeck had given basically the same talk in the Simon's Institute [(youtube video)](https://youtu.be/fV4qd43OsY8).

* ["Supervised learning through the lens of compression"](http://papers.nips.cc/paper/6490-supervised-learning-through-the-lens-of-compression) by O. David, S. Moran, and A. Yehudayoff
    * Roughly speaking, a function class is learnable if it allows (approximate) compression. Notice that the [compression](https://users.soe.ucsc.edu/~manfred/pubs/T1.pdf) is not defined in the Shannon-theoretic way.

* ["Generalization of ERM in stochastic convex optimization: The dimension strikes back"](http://papers.nips.cc/paper/6467-generalization-of-erm-in-stochastic-convex-optimization-the-dimension-strikes-back) by V. Feldman
    * This paper shows that minimizing the empirical average is not an optimal strategy for stochastic approximation in general.  

* "Safe testing: An adaptive alternative to p-value-based testing" by P. Grunwald
    * Grunwald proposed the notion of safe test, which is robust against possible abuse of statistical methods, such as collecting data until the p-value is large enough. He also provided an algorithm for safe testing, based on the so-called reverse I-projection.
    * Unfortunately, it seems that the paper has not been available on the internet.     

* ["Theory and algorithms for forecasting non-stationary time series"](https://nips.cc/Conferences/2016/Schedule?showEvent=6206) by V. Kuznetsov and M. Mohri
    * This is a tutorial talk mainly based on their [NIPS'15 paper](http://papers.nips.cc/paper/5836-learning-theory-and-algorithms-for-forecasting-non-stationary-time-series) and [COLT'16 paper](http://www.jmlr.org/proceedings/papers/v49/kuznetsov16.html).

* ["Without-replacement sampling for stochastic gradient methods"](http://papers.nips.cc/paper/6245-without-replacement-sampling-for-stochastic-gradient-methods) by O. Shamir
    * This paper provides convergence guarantees for the setting mentioned in its title.

* ["Stochastic optimization: Beyond stochastic gradients and convexity: Part 2"](https://nips.cc/Conferences/2016/Schedule?showEvent=6200) by S. Sra
    * The [slides](http://suvrit.de/talks/vr_nips16_sra.pdf) can serve as a good bibliography on solving non-convex finite-sum optimization problems.

* ["MetaGrad: Multiple learning rates in online learning"](http://papers.nips.cc/paper/6268-metagrad-multiple-learning-rates-in-online-learning) by T. van Erven and W. M. Koolen
    * This paper proposes a somewhat universally optimal scheme for online learning. The idea is to discretize the interval of possible step sizes, treat each candidate step size as an expert, and then do prediction with experts to choose the step size.
