---
title: Löb's Theorem
about: Open source game theory makes non-trivial use of a result from mathematical logic.
---

After a long time of struggling to find an example of formal logic saying something non-trivial other than "self-references are dangerous, maybe we shouldn't naively allow them", I finally found a "real" application: [open-source game theory](https://acritch.com/osgt-is-weird/).

Consider "bots" (rules) that can look into each others' source codes and then decide whether to _cooperate_ or to _defect_. (For a usual game theoretic analysis we would have to know the payoff structure of this game, but in what follows this turns out to be irrelevant.)

For simplicity's sake, let us consider the simpler case with unbounded computational resources and the following two bots:

{% highlight Python %} def CoopBot(opponent):
  return C {% endhighlight %}
{% highlight Python %} def FairBot(opponent):
  if there exists a proof that opponent(FairBot) = C:
    return C 
  else:
    return D {% endhighlight %}

Obviously `FairBot` cooperates with `CoopBot`, since `CoopBot` always cooperates with everyone.

But does `FairBot` cooperate with another copy of itself? My intuition was something along the lines of "both `FairBot(FairBot)=C` and `FairBot(FairBot)=D` are logically consistent, so there cannot be a proof for `FairBot(FairBot)=C` and hence `FairBot(FairBot)=D`, i.e. both will defect".  

Enter [Löb's theorem](https://en.wikipedia.org/wiki/L%C3%B6b%27s_theorem): \\(\square(\square P\rightarrow P)\rightarrow\square P\\), or in words <q>in Peano arithmetic (PA) (or any formal system including PA), for any formula P, if it is provable in PA that _'if P is provable in PA then P is true'_, then P is provable in PA</q>.

Hence `FairBot` will cooperate with another copy of `FairBot`!

Time to revisit my notes on mathematical logic.

<!--
This sounds odd, so let's follow Wikipedia's proof step by step, plugging in our `FairBot`. Here `FairBot`, a function accepting instances of its own type and returning `C` and `D` (which we identify with `True` and `False`, respectively), corresponds to \\(P\\) since `FairBot(FairBot)=True` (if and) only if it is provable that `FairBot(FairBot)=True` -- denoting `FairBot(FairBot)=True` by \\(P\\) we thus get: \\(\square P\leftrightarrow P\\).

1. So we obtained a "modal sentence" \\(P\\) such that \\(\vdash\square P\rightarrow P\\), i.e. if it is provable that `FairBot` playing itself cooperates, then `FairBot` playing itself will cooperate. Makes sense so far.
2. Using diagonalisation one can prove that for each modal formula \\(F(X)\\) (in particular \\(F(X)=X\rightarrow P\\)) with propositional variable \\(X\\), there must exist a sentence \\(\Psi : \vdash \Psi\leftrightarrow (\square \Psi\rightarrow P)\\). 


WTF
need to revise logic notes-->