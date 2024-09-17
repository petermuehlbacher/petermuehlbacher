---
title: "High-dimensional Statistics"
about: "Notes about where intuition for high-dimensional statistics fails."
---
# Local extrema
While local extrema in one and two dimensions are often depicted as (local) minima/maxima, they "typically" are saddle-points in high dimensions. This is the topic of [my second Bachelor thesis](https://github.com/petermuehlbacher/thesis2/blob/master/thesis2.pdf) where I survey LeCun's [The Loss Surfaces of Multilayer Networks](https://arxiv.org/pdf/1412.0233.pdf) and Auffinger, Ben Arous, Černý's [Random Matrices and Complexity of Spin Glasses](https://arxiv.org/pdf/1003.1129.pdf). The former paper relates the energy landscape (loss function to be minimised) of a toy model of neural networks to that of certain models dealt with in the mathematical physics literature (the latter paper). [This paper](https://arxiv.org/pdf/cond-mat/0611023v1.pdf) even goes a step further by suggesting that the distribution of eigenvalues of the Hessian of critical points is a shifted semicircle. That is, the closer the value at a critical point is to the global minimum, the higher the expected fraction of directions in which our loss function increases (if it increases in all directions, it is a local minimum). 

# Neural networks
Large (deep) neural networks have lots of counterintuitive properties, of which I will list only few:

<!--* [Grokking 'grokking'](https://beren.io/2022-01-11-Grokking-Grokking/) is a nicely written blog post-->
* [Adversarial vulnerability for any classifier](https://arxiv.org/pdf/1802.08686.pdf) shows <q>fundamental upper bounds on the robustness of any classifier to perturbations,
which provides a baseline to the maximal achievable robustness. When the latent space of
the data distribution is high dimensional, our analysis shows that any classifier is vulnerable
to very small perturbations.</q>

# <abbr title="Markov chain Monte Carlo">MCMC</abbr> algorithms
The analysis of the mixing time of Markov chains (how long do I have to run my <abbr title="Markov chain Monte Carlo">MCMC</abbr> algorithms until they spit out representative samples?) is a beautiful subject combining many different fields in mathematics, such as probability theory (random walks, couplings), geometry (Cheeger's inequality, Ricci curvature), representation theory, spectral theory, and physics (electrical networks)/computer science (Boolean analysis).

Coming up with decent algorithms remains a particularly hard challenge in cases where the support of the measure we want to sample from is quite sparse; e.g. certain graphical representations of quantum spin systems.

One aspect, however, remains to be overlooked surprisingly frequently: The space of irreversible Markov chains is much bigger than that of reversible Markov chains and often one can find an irreversible one that mixes a lot faster, see e.g. [<abbr title="Event chain Monte Carlo">ECMC</abbr>](https://www.youtube.com/watch?v=BZKN6ZoFOcQ) and [Markov Chain Monte Carlo Method without Detailed Balance](https://arxiv.org/pdf/1007.2262.pdf).

# Cauchy–Schwarz
Cauchy-Schwarz is both a very simple and a very powerful bound. I won’t even try to give an extensive list of possible applications, but concentrate instead on a classical one in probability theory.

First notice that Cauchy-Schwarz can simply be interpreted as a fancy way of saying that \\(|\cos(x)|\leq 1\\) for all real \\(x\\) by the following identity:
\\[|\langle a,b\rangle|=|\cos(\theta)|\|a\|\|b\|\leq\|a\|\|b\|,\\]
where \\(\theta=\angle(a,b)\\) is the angle between \\(a\\) and \\(b\\).

#### On \\(\mathbb R^n,n\gg1\\), how far off is Cauchy–Schwarz "typically"?
* The intuition is that in higher dimensions it becomes increasingly unlikely for two vectors to be approximately parallel (i.e. θ≈0 or θ≈π in which case Cauchy-Schwarz is sharp) since there are "too many directions". 
* More rigorously, we may fix \\(a=(1,0,\dots,0)\\) and sample \\(b\\) from the Haar measure on the unit sphere. 
* Note that for \\(n\gg1\\) we can do this by sampling \\(n\\) <abbr title="independent, identically distributed">iid</abbr> centered normal variables \\((x_i)_{i=1}^n\\) with variance \\(1\over n\\). 
  * <span markdown="0">This ensures that squares have expectation \(\mathbb E x_i^2 = \text{Var}(x_i)-(\mathbb Ex_i)^2 = {1\over n}\) and thus, by linearity of expectation we have that \(\mathbb E\|(x_i)_{i=1}^n\|_2=1\). It is not too hard to see that the variance of \(\|(x_i)_{i=1}^n\|_2\) vanishes as \(n\to\infty\).</span>
* Thus, we see that each (in particular the first) entry of \\(b\\) is of order \\(O(n^{-{1\over2}})\\) and thus 
\\[\mathbb E|\langle a,b\rangle|=O(n^{-{1\over2}}).\\]

#### How tight is \\(|\mathbb Ef|^2\leq\mathbb E[|f|^2]\\)
* First note that this inequality can be seen as another manifestation of Cauchy–Schwarz (although the observation that the variance cannot be negative is a quicker way to see it):
  * Every probability measure \\(\mathbb P\\) induces an inner product on the space of random variables with finite second moment by setting \\(\langle f,g\rangle:=\mathbb E f\bar g\\).
  * <span markdown="0">Now apply Cauchy–Schwarz to the constant one function and an arbitrary other function to get \[|\langle 1,f\rangle|^2 = \cos(\theta)\langle f,f\rangle\leq \mathbb E[|f|^2].\]</span>
  * We get another curious identity when trying to convert this additive correction into a multiplicative one by using \\(\sin^2+\cos^2=1\\):
  \\[|\langle 1,f\rangle|^2=\langle f,f\rangle-\text{Var}f = \langle f,f\rangle\left(1-{\text{Var}f\over\langle f,f\rangle}\right) \Rightarrow {\text{Var}f\over\langle f,f\rangle}=\sin(\theta)^2.\\]
* The takeaway is:
> <span markdown="0">The smaller the variance (of \(f\)), the tighter the bound \(|\mathbb Ef|^2\leq\mathbb E[|f|^2]\).</span>
* Using some [Poincaré-type inequalities](https://en.wikipedia.org/wiki/Poincar%C3%A9_inequality#Poincar%C3%A9%E2%80%93Wirtinger_inequality) we find another rule of thumb, namely that "all else equal",
> the smoother \\(f\\), the smaller the variance of \\(f\\).


