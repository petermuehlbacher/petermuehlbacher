---
title: "Aggregating Forecasts"
about: "Thoughts on different methods to combine different forecasts."
updated: 2022-02-14
category: Forecasting
---
# A (non-exhaustive) list of ways how to combine forecasts
...or, more generally, probability measures.

* **Weighted average**. For binary questions we have even more freedom: Do we average the probabilities \\(\mathbb P(A)\\) or do we average the odds \\(\mathbb P(A)\over\mathbb 1-P(A)\\)? Log-odds? Using
  * *linear combinations*, more commonly known as <em>Bayesian Model averaging</em>, or
  * *geometric combinations* (for binary questions)?
* **Extremising**. For binary questions. If forecasters with independent sources arrive at the same conclusion—say, some event will happen with probability 80%—then we should in fact update towards something _higher_ than 80%.
* **With restrictions on the probability distribution**. Sometimes we want the resulting distribution to be in a certain class of distributions, e.g. Gaussian.
* **Via markets/betting**. Note that beliefs (probability distributions) generally induce a set of bets you should be happy to take and vice versa. Now imagine a number of traders with a certain amount of money to bet on some event derivatives market. Clearing prices in this market will be moved by these bets and will, in turn, reveal the market participants' combined beliefs (weighted by the amount of money they traded with). The way we combine beliefs, however, depends a lot on how traders act: 
The first line in this paragraph already makes the assumption that they are rational (e.g. if they believe an event is virtually certain to happen, they won't take a bet where they lose 1$ if it happens and win 1$ otherwise). 
But what about their utility functions? Are they Kelly bettors? Fractional Kelly bettors? Or, more realistically, much more conservative? 
What about their trades on other markets? Are some of their trades hedges? It turns out that, in an idealised setting where all traders are Kelly-bettors, markets are not only a wealth-weighted average of traders' beliefs, but <a href="https://arxiv.org/abs/1201.6655"><q>the market learns at the optimal rate, the market price reacts exactly as if updating according to Bayes' Law</q></a>. The same paper suggests that such a market also performs relatively well compared to the best trader -- although I suspect that this might be an artifact of the unrealistic assumption of particularly aggressive traders (i.e. Kelly bettors).

# Some comparisons
* Effective Altruism forum user Jsevillamol wrote [three posts](https://forum.effectivealtruism.org/s/hjiBqAJNKhfJFq7kf) collecting theoretical properties of, and comparing different aggregation methods (both theoretically and empirically); primarily for binary questions.
  * <span markdown="0">He concludes that the <strong>geometric mean of odds \(\sqrt[n]{o_1\dots o_n}\),</strong> where \(o_i = {p_i\over 1-p_i}\), <strong>should be one's first choice</strong> – this method of combining probabilities \(p_i\) has some theoretical appeal (it satisfies "external Bayesianity") and performs well on actual data.</span>
  * **Extremising may improve on this**, but by how much (if at all) you want to extremise is very situation-dependent. (Sometimes you'll want to extremise because markets are underconfident, a psychological bias, sometimes because different market participants actually have access to different pieces of information.)
  * <span markdown="0">He also notes that <q>arithmetic mean of probabilities ignores information from extreme predictions</q>, i.e. uncertainty overrides nuanced predictions. For example, \({13\%+0.0001\%\over2}\approx{13\%+1\%\over2}\), but the difference between 0.0001% (one in a million) and 1% (one in a hundred) can be huge.</span>
  * One of the main takeaways (from [user Simon_M in the comments](https://forum.effectivealtruism.org/s/hjiBqAJNKhfJFq7kf/p/sMjcjnnpoAQCcedL2?commentId=HNtKuSWwcyB5gdeE3)), however, is that **<q>weighting is much more important than how you aggregate your probabilities.</q>** That is, you want to give more weight to better forecasters and more recent forecasts.

# More on (betting) markets
Markets have lots of nice properties: 

* If someone is obviously wrong (e.g. willing to bet $100 at 1:1 odds that the next roll of a die will come up 6), a sufficiently liquid market will quickly correct for it by clearing that offer. 
* If two related markets have incompatible odds, e.g. "Will animal farming be banned by 2025?" being more likely than "Will animal farming be banned by 2050?", someone will quickly spot an arbitrage opportunity and correct the prices. 
* People who are more skilled tend to win more and can thus correct more. This is certainly quite different from a more egalitarian prediction aggregator like the community median of Metaculus (but closer to its proprietary forecast which gives more weight to users with a better track record). 

One interesting aspect, however, is that putting money on the line also indicates how confident someone is in their beliefs (being better than the market's). 

(If this doesn't seem like a big deal to you, recall that we are in a world with limited computational resources: While, ideally, a forecast about whether some event \\(A\\) will happen or not, will be \\(\mathbb P(A\mid\mathcal F_t)\\), where \\(\mathcal F_t\\) is the sigma algebra encoding the information available to you at time \\(t\\) (now), computing this is hard. For example, take the question [Kyiv to fall to Russian forces by April 2022](https://www.metaculus.com/questions/9939/kyiv-to-fall-to-russian-forces-by-april-2022/). Surely, getting \\(\mathbb P(\text{Kyiv to fall by April 2022}\mid\mathcal F_t)\\) should make use of all the information available to you; this includes not only staying on top of news, but also <abbr title="Open-source intelligence">OSINT</abbr>, lists like [this](https://twitter.com/WarSignals), and much more. Next to working a full-time job, let alone forecasting on various other questions as well, this seems impossible, so often one ends up deferring to the community/people with a good track record who seem to spend more time on this.)

The result of using approximations like this, however, is that the interpretation of forecasts becomes more difficult on platforms like Metaculus: Imagine forecasting on some problem you believe to have an almost deterministic answer if only you spent enough time to figure it out. In absence of a reasonable community median, lack of thorough research for a question on Metaculus typically pushes you closer to a 50% forecast -- the extreme case being that you suddenly don't understand English anymore and can't tell whether it asks about \\(A\\) or \\(\lnot A\\). The community now can't tell apart your 50% forecast (you believe that it's either close to 0% or close to 100%) from someone who forecasts 50% because they believe it's inherently really random. 

In markets, the same situation looks very different: You (not having done your research, hence deferring to the community) simply wouldn't bet any money, while someone who believes it's inherently really random is happy to bet at (approximately) 1:1 odds, even (/especially) after doing their research. 