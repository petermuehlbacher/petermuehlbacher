---
title: "Useful Priors"
about: "A collection of priors useful for forecasting."
---
Many examples are from this [Effective Altruism forum post](https://forum.effectivealtruism.org/posts/SBbwzovWbghLJixPn/what-are-some-low-information-priors-that-you-find). A more exhaustive (but less accessible) list of priors, edited by Andrew Gelman, can be found [here](https://github.com/stan-dev/stan/wiki/Prior-Choice-Recommendations). Without much further ado:

# Practical priors

* [Basic (reading) comprehension failure seems to be around 2%.](https://www.lesswrong.com/posts/LhetQpmDDhX5LrrMv/what-are-some-low-information-priors-that-you-find?commentId=yMZLwmz8xwMo9FErZ)
  * Curiously, this is around half of the [Lizardman's constant](https://en.wikipedia.org/wiki/Slate_Star_Codex#Lizardman's_Constant) which is ~4% -- <q>referring to the approximate percentage of responses to a poll, survey, or quiz that are comedic or malicious in nature</q>.
* [<q>There is a 93.75% chance that the median of a population is between the smallest and largest values in any random sample of five from that population.</q>](https://forum.effectivealtruism.org/posts/SBbwzovWbghLJixPn/what-are-some-low-information-priors-that-you-find?commentId=msoLj8nqPqmuryXGA)
* [Laplace's rule of succession](https://www.lesswrong.com/posts/ea7CGqF3pmqpebogK/laplace-s-rule-of-succession). 
  * <abbr title="Too Long; Didn't Read">TL;DR</abbr>: After observing some (few) independent samples of the same binary random variable ("coin tosses", say, with H occurrences of heads and T occurrences of tails) then we should estimate the probability to get heads not by the frequentist guess \\(H\over H+T\\), but with \\(H+1\over H+1+T+1\\).
  * This boils down to being more Bayesian than frequentist, i.e. not only considering data before us, but also starting with a prior beta prior.
  * Another prior gives Jeffrey's prior, where we estimate the probability of getting heads with \\(H+{1\over2}\over H+T+1\\).
* [Gott's principle/The Copernican principle](https://www.nature.com/articles/363315a0.pdf).
  * Often summarised as: <q>With 50% probability, things will last twice as long as they already have.</q>
  * This is a continuous analogue of the special case of the [German tank problem](https://en.wikipedia.org/wiki/German_tank_problem) where we observed one tank.
  * An easy derivation providing more information: 
    * Let \\(U={t\over T}\\), where \\(t\\) is the time something existed until now and \\(T\\) is the (unknown; to be estimated) total time of existence.
    * In absence of further information, we should assume \\(U\\) to be uniformly distributed in \\([0,1]\\) ("there is nothing special about this moment right now").
    * Thus \\(T = {t\over U}\\), a special case of a [Pareto distribution](https://en.wikipedia.org/wiki/Pareto_distribution#Random_sample_generation).
  * Some properties of this distribution (without loss of generality, normalise time so that \\(t=1\\)):
    * \\(\mathbb P(T=x)={1\over x^2}dx\\) for all \\(x\geq 1\\), 
      * since \\(\mathbb P(T=1/U\geq x) = \mathbb P(U\leq 1/x) = 1/x\\) (take the derivative to get the density)
    * This distribution is heavy-tailed, in particular it doesn't even have a finite expectation.
* <a href="https://forum.effectivealtruism.org/posts/SBbwzovWbghLJixPn/what-are-some-low-information-priors-that-you-find?commentId=y7K6oSeBPavWuAoi3"><q>Things will stay mostly as they have.</q></a>
* Liquid markets without too much friction (withdrawal costs, maximal deposits, trading fees, etc.) imply probabilities for everything they trade on. 
  * <a href="https://twitter.com/CharlesD353/status/1312290608917118977"><q>Anyone who uses predictit as a proxy for what the market odds are instead of Betfair (where more than $100m has traded and there are no limits, all in fees are 2% and most participants won't owe taxes on winnings) is being absurd.</q></a>
  * Caveat: <a href="https://www.lesswrong.com/posts/5jA3Tvxh2jFcFBzqR/risk-premiums-vs-prediction-markets"><q>When everyone cares about an outcome, it skews the market odds.</q></a>

# Theory

* **[The principle of maximum entropy](https://en.wikipedia.org/wiki/Principle_of_maximum_entropy)**
  * For example the [maximum entropy distribution](https://en.wikipedia.org/wiki/Maximum_entropy_probability_distribution)...
    * ...on a finite set is the uniform distribution,
    * ...on the positive reals with fixed mean is the exponential distribution,
    * ...on the reals with fixed mean and variance is the normal distribution.
* **Universality/Looking for fixed points**
  * If some quantity to be estimated can be seen to be (roughly) invariant under some flow, its probability distribution has to be (close to) a fixed point of that flow. 
  * The by far most common instances of this are the "flows" of adding/multiplying another independent variable and normalising. The multiplicative case can be reduced to the additive one by taking logarithms.
    * In the case of finite variances, the additive case leads to a **normal distribution**, by the central limit theorem. With infinite variances, we [may follow a similar approach](https://arxiv.org/abs/1908.03580) to get **[α-stable Lévy processes](https://en.wikipedia.org/wiki/Stable_distribution)**.
    * Example: The usual derivation of stock market prices being modelled as geometric Brownian motions.
  * Another set of examples is that of flows whose fixed points are [scale invariant](https://en.wikipedia.org/wiki/Scale_invariance). In this case we expect **[power laws](https://en.wikipedia.org/wiki/Power_law)** to appear.
    * In physics, we often have some sort of scale invariance at criticality, so that many systems with "self-organised criticality" or self-similarity at different scales (fractals) exhibit power laws.
    * In mathematics, we often encounter power laws in the context of [scale free networks](https://en.wikipedia.org/wiki/Scale-free_network); these are often the result of [preferential attachment](https://en.wikipedia.org/wiki/Preferential_attachment) (<q>the rich get richer</q>) or [fitness](https://en.wikipedia.org/wiki/Fitness_model_(network_theory)) dynamics. Alternatively, they are in some sense the only distribution exhibiting a certain kind of scale-invariance, as outlined in [this post by T. Tao](https://terrytao.wordpress.com/2009/07/03/benfords-law-zipfs-law-and-the-pareto-distribution/).
    * Empirically, we see that power laws (or more specifically: [Pareto distributions](https://en.wikipedia.org/wiki/Pareto_distribution)) are a good fit for 
      * income distribution (Pareto noticed that approximately 80% of Italy's land was owned by 20% of the population),
      * [1% of internet users generate content, 9% update, 90% only consume](https://en.wikipedia.org/wiki/1%25_rule_(Internet_culture)),
      * population distribution of cities/countries,
      * [frequency of words in natural languages](https://en.wikipedia.org/wiki/Zipf%27s_law),
      * [distribution of leading digits of large numbers](https://en.wikipedia.org/wiki/Benford%27s_law); this is because log-normal distributions with large variance look like power law distributions.

# Combining the above to get a forecast
More often than not, the questions we are interested in are more complex than any of the above. However, we may be able to break them down into subquestions for which the above priors are helpful, see e.g. [Fermi problems](https://www.lesswrong.com/posts/PsEppdvgRisz5xAHG/fermi-estimates). 

Note, however, that the more subquestions there are to be estimated, the bigger the expected error (although, if the indiviual estimates are unbiased and independent, this error is smaller than one might think). Moreover, one needs to be careful as soon as one ends up using information coming from the tail of a distribution (e.g. a 95% or 99% confidence interval); slightly paraphrasing [SimonM](https://www.lesswrong.com/posts/GMCs73dCPTL8dWYGq/use-normal-predictions?commentId=rSybMo3qeQDTddqET):

1. People (and their models) in general are not calibrated well, especially at the tails.
2. If they are, it takes a while to tell apart those that are from those that are not.
3. Tails are often dominated by model failure, so asking about 95% <abbr title="confidence interval">CI</abbr>s tells you more about their model than about "real" probabilities.


<!--# Updating on priors
The easy (and lazy) answer to questions about updating forecasts is Bayes' theorem, or, as [ACX](https://astralcodexten.substack.com/) puts it: <q>P(A|B) = [P(A)*P(B|A)]/P(B), all the rest is commentary.</q>
A lot is to be said about what Bayes' theorem implies (you just got a positive test with low false positivity rate for an incredibly rare disease; should you be nervous?) and how it appears naturally in various situations (e.g. arbitrage arguments in betting markets, as in [Ten Great Ideas About Chance](https://press.princeton.edu/books/hardcover/9780691174167/ten-great-ideas-about-chance)), but here I will focus on martingales:

-->