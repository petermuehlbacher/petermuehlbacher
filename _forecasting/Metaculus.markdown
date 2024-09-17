---
title: "Metaculus"
about: "Metaculus is an information aggregation website which allows users to compare their probabilistic forecasts to those of others. My track record can be found here."
updated: 2022-02-14
category: Forecasting
---
# My track record

My binary calibration on Metaculus questions:
![Binary Calibration on Metaculus](/assets/metaculus_binary_calibration.png)

I am [`peter_m`](https://www.metaculus.com/accounts/profile/118245/) (click for a list of my comments) on Metaculus.

Up until now not enough questions (16 binary questions, 5 real-valued questions) have resolved to provide proof beyond doubt that I am <em>[well-calibrated](https://en.wikipedia.org/wiki/Calibration_(statistics)#In_prediction_and_forecasting)</em> or <em>sharp</em> (=not too over-/underconfident; for statisticians: <a href="https://statmodeling.stat.columbia.edu/2019/09/05/gneiting-on-calibration-and-sharpness/"><q>By way of analogy to point estimators, calibration is like unbiasedness and sharpness is like precision (i.e., inverse variance)</q></a>.)

In fact, data weakly indicates that I am underconfident.

# Finding "arbitrage opportunities"

Since Metaculus is not a market, but an information aggegrator, "arbitrage opportunity" does not appear to be a well-defined concept. Thinking of arbitrage as logical inconsistency (the kind of inconsistency that allows you to extract risk-free money from a market; or at least a positive amount of money in expectation), however, we might simply interpret this as a question of whether the community prediction is "consistent over time". 

The key idea is that the evolution of forecasts over time \\((p_t)_{t}\\) must be given by a martingale. *This implies various bounds and equalities that are **model and data agnostic!*** (I.e. we don't have to make any assumptions whatsoever for these bounds&equalities to be necessarily true for sensible forecasts.)

* [In this notebook](https://colab.research.google.com/drive/1Axi7hf6fOJozq5VaMXFhHZz7TBN4O1dw) I find that the Metaculus community overreacts a little bit, whereas a weighted forecast (better predictors' forecasts contribute more) performs better.
* [In this notebook](https://colab.research.google.com/drive/1z9khR2P2CY-B1OvlJC-tzqPvzxr4Bqt5) I discuss the above ideas in more detail and work out what it implies for actual betting markets, e.g. how to come up with your own betting strategies to capitalise on different martingale violations.

<!--Before diving into the data, we need to make a small detour on what "being consistent over time" implies for a forecast:

## Optional stopping bounds on big updates
<span markdown="0">Denote by \(\mathcal F=(\mathcal F_t)_t\) a <a href="https://en.wikipedia.org/wiki/Filtration_(probability_theory)">filtration</a> of our filtered probability space \((\Omega,\mathcal A,\mathcal F,\mathbb P)\). Informally, \(\mathcal F_t\) stands for the set of events whose occurrence you are aware of at "time" \(t\) (i.e. you know whether they happened or not), or, in other words, it <q>models the information that is available at a given point</q>.</span>

<span markdown="0">Assume we are interested in whether some political party gets elected in the upcoming election; call this event \(A\in\mathcal A\). Our (subjective, at time \(t\)) probability \(p_t\) of \(A\) being true can be written as a conditional expectation \[p_t=\mathbb E[1_A\mid\mathcal F_t].\]
It is well known that \(t\mapsto p_t\) is a martingale.</span>

<span markdown="0">Upon learning more about the world (e.g. as time goes on and new polls are published), we need to update our forecasts. Clearly, if, at some point in time \(s<t\), we have \(p_s=0.01\), then the probability of \(p_t=1\) (i.e. \(A\) ever coming true) for any \(t>s\) is \(0.01\). But martingales give us more! We can also bound the probability of \(\sup_{t>s}p_t>x\) for any \(x\). Intuitively, if at some point a well-calibrated forecaster assigned very low probability to some event, their probabilities should hardly ever exceed a significantly higher probability ever again. The optional stopping theorem allows us to make this rigorous and indeed we get for any \([0,1]\)-valued martingale \((X_t)_t\) with \(X_T\in\{0,1\}\) (i.e. the question is resolved by time \(T\)) and stopping time \(\tau=T\land \inf\{t>0:X_t\geq p\}\) that \[\mathbb EX_0 = \mathbb E X_\tau = \underbrace{\mathbb P(X_\tau\geq p)}_{=:q}\underbrace{\dots}_{\in[p,1]}+(1-q)0 \Rightarrow \mathbb P(\sup_{t>0}X_t\geq p)\in [X_0,X_0/p]. \]
</span>

## Comparing theory with data

<span markdown="0">Now we might, for example, look at questions where the community forecast (consisting of at least 10 different forecasts, say) was below 10% at some point and see how often it exceeded 90% after that again. By the above bound, this ought to happen with probability between 0.1 and 0.1/0.9≈0.11, i.e. for roughly 1 in 10 resolved questions. Now this is not very informative since it is essentially a question about whether the community prediction is well-calibrated. Replacing 90% with 50%, say, gives us the less trivial bound of about 10%–20% of all resolved questions going from ≤10% to ≥50%.</span>

So what does the data say? I fetched data from the [(inofficial?) JSON API](https://www.metaculus.com/api2/questions/?limit=400), but this takes a while since we can't download everything in one go. I restricted myself to

* binary questions (`entry['possibilities']['type'] == 'binary'`),
* that are resolved (`entry['resolution']==True`),
* with predictions by at least 15 different users (`entry['distribution']['num']>=15`).

From these datasets (unfortunately only 329 at the time) I took the `['community_prediction']` field from the time series, and appended the `['resolution']` as the "final community prediction". 

What I found was mostly in line with what the above martingale bound predicts, with some outliers that may as well be due to chance since the sample size was not big enough. -->