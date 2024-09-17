---
title: "Post-mortems"
about: "Here I try to analyse where some of my worst forecasts went wrong."

---

Please keep in mind that these examples are cherry-picked from my _worst_ predictions. (Worst in the sense that someone with access to the same information at the time could have pointed out my mistake.)

## [Long COVID post hospitalisation](https://www.metaculus.com/questions/6589/long-covid-post-hospitalisation/)

![](/assets/metaculus_longcovidposthospitalisation_pdf.png)

One of my first predictions on Metaculus for which my reasoning can be found [here](https://www.metaculus.com/questions/6589/long-covid-post-hospitalisation/#comment-66160). At the time of writing this question hasn't resolved yet, so what went wrong? I came up with a simple model which deviated significantly from the community forecast and then shifted the pdf a bit to the left to account for the possibility of the community knowing something I didn't. Despite deviating from the community in what looks like the right direction, I completely failed to think about the tails of my distribution, however, which [could potentially prove fatal](https://www.metaculus.com/questions/6589/long-covid-post-hospitalisation/#comment-75918).
This seems to be a case of being too invested in one particular model without accounting for the possibility of model failure. <!--([<q>[The] tails are often dominated by model failure</q>](https://www.lesswrong.com/posts/GMCs73dCPTL8dWYGq/use-normal-predictions?commentId=rSybMo3qeQDTddqET))-->

## [Russian invasion of Ukraine before 2023](https://www.metaculus.com/questions/8898/russian-invasion-of-ukraine-before-2023/)

![](/assets/metaculus_ukraine_timeseries.png)

A lot is to be said about this question; see [arguments for an invasion as early as in December](https://twitter.com/DAlperovitch/status/1473362460673515527), [base rates](https://tellingthefuture.substack.com/p/notes-on-forecasting-the-invasion), track records of media & US intelligence calling it relatively early, ["good" forecasts (on the invasion) correlating strongly with "bad" forecasts (on the invasion's success)](https://astralcodexten.substack.com/p/ukraine-warcasting) (and vice versa).

In short, there were signals pointing in different directions and even in hindsight it doesn't seem obvious what a good forecast should have been. However, I can identify where _I_ went wrong and this was relying too much on the following heuristic:

> At the end of the day, once the sabre-rattling is done, leaders/nations typically end up not doing anything that's grossly misaligned with their economic goals. 

This heuristic worked in the past for questions about <abbr title="Organization of the Petroleum Exporting Countries">OPEC</abbr> production cuts, countries' reactions to abuse by their biggest trading partner, etc. Two possible reasons for it to fail come to mind:

1. This heuristic is simply wrong or doesn't apply to leaders whose plan is to go down in history instead of being reelected (fairly).
2. Like many Western (often conservative) forecasters who were more confident about an impending invasion, Putin believed that the invasion wouldn't be met with much resistance, neither by Ukrainians, nor in the form of economic sanctions; so in his eyes this might not have been misaligned with economic goals at all. 

The takeaway thus seems to be to
* account for other (more personal) motives, especially in a non-democratic environment,
* account for leaders surrounding themselves with people telling them what they want to hear (in particular not having access to good intelligence services),
* take the [Madman theory](https://en.wikipedia.org/wiki/Madman_theory) a bit more seriously.

<!--My reasoning for being more or less consistently below the community average was basically due to a mix of 

* not doing enough research (e.g. [this thread](https://twitter.com/DAlperovitch/status/1473362460673515527) gave solid arguments for an invasion as early as in December, but I only saw it after the invasion),
* thus relying on base rates too much (all out war being rare in Europe, Russian troops have already [occupied some areas](https://en.wikipedia.org/wiki/Occupied_territories_of_Ukraine) for a while without launching a full-blown invasion alienating the rest of Europe),
* [recency bias](https://en.wikipedia.org/wiki/Recency_bias) (see the dip around mid-February),
* thinking that media and US intelligence, not exactly having the best track record so far, are overreacting to a bluff.

I was saved (to some extent) by being aware of my ignorance and thus mostly deferring to the community.

That being said, Robert de Neufville wrote [a much more in-depth post-mortem on this question](https://tellingthefuture.substack.com/p/notes-on-forecasting-the-invasion) which is worth reading in its entirety, but I also want to present some arguments for why overly certain forecasts, despite being proven correct a posteriori, might not have been that good either: 

> <q>There are strong arguments that 85% was too high. Wars on this scale are historically rare, so we should probably be skeptical they’ll happen. It is also hard to say with certainty what will happen when it largely depends on a single person’s decision—as it did in this case—unless you have a very clear window into that person’s thinking. The fact that Putin was trying to obscure his intentions made it even harder. Russia’s actions in Georgia and Crimea foreshadowed Russia’s recent invasion of Ukraine, but those invasions were also on a significantly smaller scale. They seemed calibrated to stay below the threshold of triggering a unified international response. Even Putin’s inner circle reportedly thought he was bluffing. The US intelligence assessments that Russia would invade were compelling—and were consistent with open-source intelligence—but were clearly part of a messaging campaign designed to box Russia in, so they couldn’t be taken entirely at face value. Even if the decision to invade had already been made, it could still have been stopped at any time. Was there really less than a 1-in-6 chance that it could be deterred or an agreement could be reached?</q>

Scott Alexander wrote another excellent post-mortem [here on ACX](https://astralcodexten.substack.com/p/ukraine-warcasting), where he highlights how a lot of "pundits" seemed to happen to be on the right/wrong side of maybe mostly due to their political convictions and less so because of principled reasoning.-->

## [When will the UK reach herd immunity >53.3m for Covid-19?](https://www.metaculus.com/questions/6105/uk-covid-herd-immunity-533m-date/)

A classic: I predicted on the title, while the resolution criteria actually state that <q>this question will resolve as the date when <strong>a credible media report stating that the >53.3m immunity threshold has been reached is published</strong>, rather than the date when this threshold is reached.</q>

The takeaway is obviously to read the resolution criteria carefully and take into account how they might fail to capture the intended question. I actually started doing this and it didn't take too long before I stumbled upon [a similar problem](https://www.metaculus.com/questions/7544/pr%25C3%25B3spera-at-10000-residents-before-2035/#comment-76861): If a question resolves ambiguously under some conditions, then the question effectively becomes a conditional probability, which might skew probabilities a lot.

A much more extreme example: I was told about a real money (crypto) market on whether Biden would be president at some date. Clearly this was supposed to be a proxy for whether Biden would win the election versus Trump, but the date was one year in the past! Clearly a typo, but that didn't keep traders who didn't read carefully from betting (and losing) a lot of money on it.