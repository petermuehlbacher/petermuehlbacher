---
title: "Kelly Betting"
about: "A collection of interesting properties of and arguments for and against the Kelly criterion."
---
Arguably there are already sufficiently many articles on the [Kelly criterion](https://en.wikipedia.org/wiki/Kelly_criterion). Here are a few that I liked:

* [Edward Thorp](https://web.archive.org/web/20090320125959/http://www.edwardothorp.com/sitebuildercontent/sitebuilderfiles/KellyCriterion2007.pdf) provides a great review of the Kelly criterion (with theorems, possible objections and many applications),
  * note also, that [continuous rebalancing à la Kelly typically outperforms choosing your bets once in the beginning](https://colab.research.google.com/drive/12tJ6YD8dahiUh6rMIAwu0bbbhEKkpK3S?usp=sharing).
* [This paper](https://arxiv.org/abs/1201.6655) proves that a market with Kelly bettors
  * has prices that are a wealth-weighted average of traders' beliefs (no surprise so far),
  * learns at the optimal rate and the market price reacts exactly as if updating according to Bayes' Law (a bit of a surprise?),
  * and the market prediction has low worst-case log regret to the best individual participant (quite a surprise, although potentially an artifact of the unrealistic(?) assumption of overly aggressive Kelly traders).
* [Proebsting's "paradox"](https://en.wikipedia.org/wiki/Proebsting%27s_paradox); not really a paradox, but raises questions about applying it in practice,
* [Buck Shlegeris](https://shlegeris.com/2018/04/11/kelly.html) conjectures that any market should eventually be dominated by Kelly bettors,
  * and [this paper finds that the answer is "it depends"](https://dspace.mit.edu/bitstream/handle/1721.1/114640/10818_2017_9253_ReferencePDF.pdf?sequence=2&isAllowed=y).
* [A seasoned PredictIt trader](https://predictingpolitics.com/2021/04/04/why-the-kelly-criterion-kinda-sucks/) presents some arguments why Kelly is actually not so great in practice,
* [SimonM on Twitter](https://twitter.com/SmoLurks/status/1255074440083357699) shows that fractional Kelly is really just (partially) deferring to the market (and then going full-Kelly with the updated probabilities) and more reasons to [never go full-Kelly](https://www.lesswrong.com/posts/TNWnK9g2EeRnQA8Dg/never-go-full-kelly) from the same author on LessWrong.
* An unfortunately rather spread-out discussion on LessWrong (see [Kelly isn't (just) about logarithmic utility](https://www.lesswrong.com/posts/zmpYKwqfMkWtywkKZ/kelly-isn-t-just-about-logarithmic-utility), [Kelly is just about logarithmic utility](https://www.lesswrong.com/posts/DfZtwtGD6ymFtXmdA/kelly-is-just-about-logarithmic-utility), and various replies in the comments/other threads); while "X is (just/not) about Y" hardly seems objectively resolvable, the arguments for and against are interesting:
  * Kelly optimises not only log-wealth, but also any fixed quantile (as the number of bets goes to infinity, by the <abbr title="Central Limit Theorem">CLT</abbr>); it immediately follows that <q>any other strategy can only beat Kelly at most 1/2 the time. (1/2 is optimal since the other strategy could be Kelly)</q>
    * Note that, in general, [for finitely many bets Kelly only minimises the median](https://colab.research.google.com/drive/1Bc-vO9MxrPZQgrfrdA7VMyywA-RYIxm_?usp=sharing). This is because betting more than Kelly increases the variance of the logarithm of our wealth, while decreasing its mean. In some cases betting a bit more than Kelly thus increases >50 quantiles (e.g. what you get in the luckiest 10% of all outcomes).
    * <q>Maximises asymptotic long run growth</q>
    * <q>Given a target wealth, is the strategy which achieves that wealth fastest</q>
    * <q>Asymptotically outperforms any other strategy (ie \\(\mathbb E[X/X_\text{Kelly}] \leq 1\\))</q>
  * A (sub-)logarithmic utility function can be justified with concerns about ergodicity for repeated bets. (In plain terms: Linear utility functions (e.g. maximising expected wealth) leaves us with having to bet everything on a bet where you double your money with 51% and lose everything with 49%. Doing this many times results in tremendous expected wealth, despite the fact that you go bankrupt almost surely. (Sub-)logarithmic utility functions, however, take care of this problem.)
* In finance, [Kelly is essentially the same as optimising the Sharpe ratio](http://epchan.blogspot.com/2014/08/kelly-vs-markowitz-portfolio.html).