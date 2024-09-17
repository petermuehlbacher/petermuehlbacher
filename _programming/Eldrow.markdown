---
title: "Eldrow"
about: "Eldrow is a puzzle about how 'unlucky' one can get when playing Wordle on hard-mode."
updated: 2022-02-14
---
[Eldrow](https://www.janestreet.com/puzzles/eldrow-index/) is a Jane Street puzzle—see the link for a detailed explanation.
<!--I liked the idea of [Norvig's <q>pytudes</q>](https://github.com/norvig/pytudes), a collection of elegant solutions to puzzles, so I attempted something similar, albeit initially inevitably less impressive.-->

# Observations

<!--#### Constraints on future guesses are a function of the past guesses and the solution
  * Green fields obviously restrict the space of future guesses to those words with the correct letters in the correct positions.
  * Grey and yellow fields
    * rule out their letters at their position, and
    * give upper and lower bounds on the number of occurrences of their letters in the solution, respectively.
  * In particular we may encode `constraints(guess,solution)` in the following set of variables:
    * `fixed`, a string like `????R` if the solution is `ROVER` and the guess was `ERROR`,
    * `at_least`, a dictionary(/list) like `{'E':1,'R':2,'O':1}`, i.e. the solution must contain at least 1 `E`, 2 `R`s, etc. (for the example above),
    * `at_most`, a dictionary(/list) like `{'R':2}`, i.e. the solution must contain at most 2 `R`s (for the example above),
    * `not`, a list like `[['E'],['R'],['R'],['O'],[]]`, i.e. the solution's first letter cannot be an `E`, its second letter can't be an `R`, etc. (for the example above).
  * Alternatively, one could write regex selectors like [`^(?=^E.[^I].E$)(?!.*[PO].*)(?:(?!L).)*(?:L(?:(?!L).)*){1,3}(?:(?!I).)*(?:I(?:(?!I).)*){1,}$`](https://regex101.com/r/KjwfcV/1)
    * This selects only 5 character strings that start and end with an `E`, do not have an `I` as their third letter, do not have any `P`s or `O`s, have between 1 and 3 `L`s and at least 1 `I`.
  * *For an exhaustive search, however, it turns out that we don't have to compute any of that.*-->

#### Pool of potential solutions

<!--* Once a word is not an admissible guess (after some other guesses), it will never be an admissible guess again.
*--> Given a sequence of guesses, think of the "pool of admissible next guesses":
  * This set, as a function of "time", i.e. the number of guesses, is strictly decreasing; in other words, one more guess can't increase the number of admissible future guesses.
  * This set can be partitioned into up to <span markdown="0">|{🟩,🟨,⬛}\(^5=3^5=243\)</span> equivalence classes. In particular there is a 1-to-1 correspondence between the marking of a guess and the equivalence class of the next guess.
  * So instead of computing the marking (by already assuming a solution), we may just fix the next guess; this also determines the marking.

#### Greedy strategies need not be optimal

  * Intuitively, the best initial guess ought to keep as many words as possible on the table; i.e. given a solution, we might want to choose an initial guess A with 200 possibly correct second guesses (given the constraints after guessing A) over an initial guess with 100 possibly correct second guesses.
  * To see why this is not necessarily correct, imagine playing Eldrow in a different language "English++" that consists of all words `AAAAA`,`AAAAB`, ..., `AAAAZ`, and the words of the English language. Assume the solution is `AAAAZ`. Guessing `AAAAA` "only" leaves us with a pool of 25 possible correct guesses, while guessing any English word without `A`s, say `BIRCH`, leaves us with however many English words there are without any of the letters `B`,`I`,`R`,`C`, and `H` -- presumably significantly more than 25. But by design any of the made-up words except `AAAAZ` (the solution) would have lead to the longest possible sequence of admissible guesses, whereas having ruled out the letters `B`,`I`,`R`,`C`, and `H` almost certainly will leave us with a strictly shorter longest possible sequence of admissible guesses.
<!-- Note that this also rules out _proving_ that we necessarily need to start with all grey fields. It does not mean, however, that this is a bad idea either; just that it depends on the language we are using. It does not seem obvious whether English (or rather the subset of English words accepted by Eldrow) is such a language.-->

#### Exhaustive strategies are _slow_

  * In the made-up language that consists of words of the form `AAAA?`, where `?` may be any letter from `A` to `Z`, any exhaustive strategy will have to loop through \\(26!\approx 4\times10^{26}\\) possibilities, whereas any human will immediately spot that any permutation of these "words" yields an admissible sequence of guesses attaining the maximal length. 
  * It seems hard to estimate this for the English language: From eyeballing [3Blue1Brown's video](https://www.youtube.com/watch?v=v68zYyaEmEA) it looks like "typically" a word reveals between 2-5 bits; this gives a (geometric) average of about 3 bits or, equivalently, the claim that "a typical word roughly cuts the pool into an \\(1\over8\\)th its size". But the total number of sequences can be written as a multiple of an expectation over a product of not really independent random variables, each \\(\geq 1\\), so this is only of limited use: One might expect such an expectation to be dominated by the tails of the individual random variables—this is somewhat similar to the ["ergodicity problem in betting/economics"](https://www.nature.com/articles/s41567-019-0732-0/figures/2).

#### When _can_ we be greedy?

…in order not to have to construct every possible sequence of guesses. More precisely, what are conditions on a guess \\(g\\), the pool of admissible subsequent guesses \\(P\\), and \\(a\in P\\) such that the set of sequences of guesses attaining maximum length (with the given constraints) contains a sequence that continues with \\(g,a,\dots\\)?

(Some notation: Call a position \\(i\in\{1,2,3,4,5\}\\) _fixed_ if \\(\forall b\in P: a_i = g_i = b_i\\).)
  * **The following criterion is sufficient:** <span markdown="0">(I.e. if the following holds for guesses \(g\) and \(a\in P\), then we have that for every admissible sequence of guesses \(g,b,b',b'',\dots\), where \(b,b',b'',\dots\in P\setminus\{a\}\), that \(g,a,b,b',b'',\dots\) is an admissible sequence of guesses as well. Hence we may proceed greedily and assume \(a\) to be the next guess.)</span>
    * Without loss of generality, we assume that no position is fixed (so we potentially have fewer than 5 positions), 
      * e.g. for \\(g=\\)`QUACK`, \\(a=\\)`QUINA`, and \\(P=\\){`QUOTE`, `QUEUE`, `QUINA`}, we identify \\(g\\) with `ACK`, \\(a\\) with `INA`, and \\(P\\) with {`OTE`,`EUE`,`INA`}.
    * All such "reduced words" (with potentially less than 5 letters) satisfy for each letter \\(\ell\\) that
      1. <span markdown="0">\(\forall b\in P\setminus\{a\}: \#\{\ell\in b\} \geq \#\{\ell\in g\}\), if \(\# \{\ell\in a\}\geq \# \{\ell\in g\}\),</span>
      2. <span markdown="0">\(\forall b\in P\setminus\{a\}: \#\{\ell\in b\} = \#\{\ell\in g\}\), if \(\#\{\ell\in a\}< \#\{\ell\in g\}\),</span>
      3. <span markdown="0">\(\forall b\in P\setminus\{a\},i: a_i=b_i\Leftrightarrow g_i=a_i\),</span>
      4. if \\(\ell=g_i\neq a_i\\), but \\(a_j=\ell\\) (i.e. \\(g\\) followed by \\(a\\) marks the i-th position of \\(g\\) yellow), then
        * either <span markdown="0">\(\forall b\in P\setminus\{a\}: b_i,b_j\neq\ell\),</span>
        * or <span markdown="0">\(\forall b\in P\setminus\{a\}: b_j=\ell\neq b_i\).</span>
  * Note that points 1.–3. above just imply that the sequence of guesses \\(g,a,b\\) is an admissible one: 
    1. If \\(a\\) has at least as many `A`s as \\(g\\), then all `A`s of \\(g\\) must have been marked green or yellow, hence their number cannot decrease anymore. 
    2. If \\(a\\) has fewer `A`s than \\(g\\), then there is at least one `A` in \\(g\\) that must have been marked grey and we must have been able to infer the correct number of `A`s with our guess \\(a\\) -- hence all subsequent guesses must have this exact number of `A`s.
    3. Clearly if the letters at the i-th position of the first two guesses coincide, then this determines the letter at the i-th position of the third guess. If, however, the letters at the i-th position of the first two guesses don't coincide, then this means that the letter at the i-th position of the first guess was not supposed to be there, hence the letter at the i-th position of the third guess must not coincide with that of the first guess.
    4. This is vacuously true if \\(g,a\\) (and hence, by 2., \\(g,b\\)) don't share any letters. If there exists such a letter \\(\ell\\), however, say `A` in the first position of \\(a\\), then 4. tells us that 
      * either all \\(b\\)s have an `A` in positions that are different from where \\(g\\) and \\(a\\) have `A`s,
      * or `A` will be marked green (in \\(a\\)), i.e. all \\(b\\)s must have an `A` where \\(a\\) has it.
  * Examples:
    * This covers our toy language consisting of words `AAAA?`: The first four letters are fixed, so we only need to consider the last letter, which for any three different guesses \\(g,a,b\\) is always different so that 4. is vacuously true.
    * For \\(g=\\)`ACK`, \\(a=\\)`INA` this is satisfied by \\(P=\\){`EAE`,`PAN`,`INA`} (the first subcase of 4.) and by \\(P=\\){`XXA`,`XYA`,`INA`} (the second subcase). 
    * It is not satisfied in our example above since `OTE` (and `EUE`) don't contain any `A`s, so `ACK`,`INA`,`OTE` is not an admissible sequence. 
    * A less trivial counterexample would be \\(g=\\)`ACK`, \\(a=\\)`INA`, \\(P=\\){`EAE`,`POA`,`INA`}: Now both `ACK`,`INA`,`POA` and `ACK`,`INA`,`EAE` would be admissible sequences of guesses, but the two sequences fix the `A` in different positions, so while `ACK`,`POA`,`EAE` (\\(g,b,b'\\) in our notation) and `ACK`,`EAE`,`POA` (\\(g,b',b\\)) are both admissible sequences of guesses, \\(g,a,b,b'\\) and \\(g,a,b',b\\) are not admissible.


# Ways of sampling
1. Loop through pairs of `initial_guess` and `solution`; fixing the solution determines the marking for each guess. For each such pair, loop through all possible next guesses, mark each one, then loop through all possible next guesses, etc.
2. Loop through `initial_guess`es; not fixing the solution, we have up to \\(3^5\\) possible markings for each guess and then have to choose from one of the compatible guesses. For each marking, we loop through all possible next guesses, etc.
  * This is basically like [Absurdle](https://qntm.org/files/absurdle/absurdle.html), which greedily selects the marking that results in the largest pool of future guesses (i.e. possible solutions).
  * Note that the above problem with greedy strategies seems to thwart rigorous results again: There is no guarantee that starting with all grey boxes actually is the "most adversarial way possible". Neither is it (necessarily) the one that leaves the largest pool of solutions/future guesses.
3. Loop through `initial_guess`es and `second_guess`es; this determines the marking of the initial guess; subsequent guesses can be found without ever marking guesses. Since an exhaustive search needs to loop through all possible markings anyway, this seems to be a strict improvement over the previous two methods. 


# Attempt #1 (Python)
Here is a brief implementation of the third way of sampling above. Note that this could be further optimised by incorporating the above observation about when we may be greedy.

{% highlight python %}
words = ["ABACK", "ABASE", ..., "ZONAL"]

# Computes the length of the longest sequence 
# of admissible guesses with initial guess `g0`.
def depth(g0: str, pool: list[str]) -> int:
    if pool == []:
        return 1
    else:
        return 1+max([depth(g1,new_pool(g0,g1,pool)) for g1 in pool])

# Given the set of admissible guesses after `g0`, 
# this function returns the set of admissibel guesses 
# after guessing `g0` followed by `g1`.
def new_pool(g0: str,g1: str,pool: list[str]) -> list[str]:
    return [w for w in pool if satisfies(g0,g1,w)]

# Returns true iff the sequence of guesses `g0`,`g1`,`w` is admissible.
def satisfies(g0: str,g1: str,w: str) -> bool:
    if w == g0 or w == g1:
        return False
    for i in range(5):
        if g0[i] == g1[i]:
            if w[i] != g1[i]:
                return False
        elif w[i] == g0[i]:
            return False
    
    for l in set(g0+g1): # the set of letters in g0 or g1
        if g0.count(l) > g1.count(l):
            if w.count(l) != g1.count(l):
                return False
        elif w.count(l) < g0.count(l):
            return False
    return True

print(depth(initial_guess,words))
{% endhighlight %}

## Attempt #1.1 (OCaml)
My first dive into [OCaml](https://ocaml.org/learn/description.html), and proper functional programming in general, proved a mixed bag (although ultimately I rather enjoyed it): 
* Compiled OCaml is _fast_ and doesn't crash like Python due to memory errors/max recursion limits.
* The latter seems hard to get around since [tail recursions](https://stackoverflow.com/questions/33923/what-is-tail-recursion) are not supported in Python ([although some dirty workarounds exist](https://stackoverflow.com/a/13592002/1122856)), while OCaml encourages everyone to make recursions tail recursive by providing the tail recursive `List.fold` method. Presumably my implementation still doesn't make the most of this and various other features of OCaml—please drop me an email if you have ideas how to improve it!
* OCaml feels pretty bare-bones-y compared to Python. Apparently there is no native implementation to get the maximum of a list of integers! This results in a fairly steep learning curve. But needing to think how all these convenient helpers are actually implemented also makes you realise how expensive your subroutines are and adapt accordingly, i.e. write efficient code.
* But OCaml is not only faster, I noticed that I never had to look for errors; in fact, whenever code is ready to compile, it ends up doing exactly what I want. I wish I could say the same about Python, but confusing conventions (e.g. concerning [lists](https://stackoverflow.com/questions/2612802/list-changes-unexpectedly-after-assignment-why-is-this-and-how-can-i-prevent-it)) regularly send me looking for errors.
* Getting OCaml code to run on/compile for Windows machines is too complicated. After failing to get [cross compilation via Dune](https://dune.readthedocs.io/en/stable/cross-compilation.html) and various other tutorials to work, I gave up, and continued to run my code on my (slow) laptop.

{% highlight OCaml %}
let words = ["ABACK"; "ABASE"; ...; "ZONAL"];;

(** Counts the number of occurrences of [c] in [str]. *)
let count c str = String.fold_left (fun acc c' -> if c' = c then acc+1 else acc) 0 str;;

(** Checks the following:
{ul 
{- If the first two guesses [a], [b] coincide at position [i], then [c] must take that same value at position [i] as well.}
{- If the first two guesses [a], [b] differ at position [i], then [c] must not take the value of [a] at position [i], as this was already ruled out by [b].}
}
Acts recursively by calling itself with [i+1] and returns [true] if [i=5].
*)
let rec check_green_grey i a b c = 
  if i = 5 then true else
    if a.[i] = b.[i] then
      if b.[i] = c.[i] then check_green_grey (i+1) a b c else false
    else 
      if a.[i] = c.[i] then false else check_green_grey (i+1) a b c;;

(**
Checks the following for all characters [c'] in either [a] or [b]:
{ul
{- If [c'] occurs more often in [a] than in [b], then it must appear in [c] exactly as often as in [b].}
{- If [c'] occurs at least as often in [b] as in [a], then it must appear in [c] at least as many times as in [a].}
}
*)
let check_yellow a b c = 
  String.fold_left (fun acc c' -> 
    acc && (if count c' a > count c' b then count c' b = count c' c else count c' a <= count c' c)) true (a ^ b);;

(**
Checks if all of the three hold:
{ol
{- [c] is different from both [a] and [b],}
{- [check_green_grey 0 a b c],}
{- [check_yellow a b c].}
}
*)
let satisfies a b c = 
  a <> c && b <> c && check_green_grey 0 a b c && check_yellow a b c;;

(**
Given a [pool] and two guesses [a], [b], this returns [new_pool], a list of elements [c] of [pool], such that [satisfies a b c].
*)
let rec new_pool a b pool =
  match pool with
  | [] -> []
  | h :: t -> if satisfies a b h then h :: new_pool a b t else new_pool a b t

(**
Returns an [int*string list] tuple that gives the length 
and the list of elements of the longest possible 
sequence of guesses, given a starting guess [a] and 
a pool of admissible guesses [pool].
*)
let rec longest_seq a pool = 
  match pool with
  | [] -> (1,[a])
  | _  -> 
    let t = List.fold_left 
      ( fun acc b -> 
        let tmp = longest_seq b ( new_pool a b pool )
        in 
        ( if fst acc > fst tmp then acc else tmp ) ) ( 0, [] ) pool
    in 
    ( 1 + fst t, a :: snd t );;

List.iter ( fun a -> print_endline a ) ( snd ( longest_seq "CRANE" words ) );;
{% endhighlight %}

This outputs the 14-word sequence `CRANE`, `TIGER`, `QUEER`, `ODDER`, `JOKER`, `HOVER`, `FOYER`, `BOXER`, `WOOER`, `SOWER`, `ROWER`, `POWER`, `MOWER`, `LOWER`.

Presumably this is still far from optimal: 
* `CRANE` was one of the first words I tried,
* it has fairly _high_ entropy, i.e. for a randomly chosen solution it reduces the pool of possible solutions by quite a bit (on average),
  * in particular it has higher entropy than `CIVIL`, which leaves a huge pool of possible solutions, but gives the shorter 13-word sequence 
  `CIVIL`, `ZEBRA`, `QUEER`, `ODDER`, `JOKER`, `HOMER`, `GONER`, `FOYER`, `WOOER`, `TOWER`, `SOWER`, `ROWER`, `POWER`,
  hence providing more evidence against choosing initial guesses greedily, 
* and running this algorithm with the initial guess `VIVID` takes _a lot_ longer.

(Keep in mind that ending in `..(OW)ER` might just be an artifact from the potential existence of many 14/13-word sequences of guesses starting with `CRANE`/`CIVIL`, respectively, and this algorithm's bias to traverse them in an alphabetical manner.)