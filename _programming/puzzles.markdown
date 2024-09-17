---
title: "Puzzles"
about: "Spoilers! Wherever strategies have to be devised, I simulate a few different ones."
updated: 2022-03-08
draft: true
---
# Wise Men
> A sultan has captured 50 wise men. He has a glass currently standing bottom down. Every minute he calls one of the wise men who can choose either to tum it over (set it upside down or bottom down) or to do nothing. The wise men will be called randomly, possibly for an infinite number of times. When someone called to the sultan correctly states that all wise men have already been called to the sultan at least once, everyone goes free. But if his statement is wrong, the sultan puts everyone to death. The wise men are allowed to communicate only once before they get imprisoned into separate rooms (one per room). Design a strategy that lets the wise men go free. 

#### Solution from [A Practical Guide To Quantitative Finance Interviews](https://usermanual.wiki/Document/Practical20Guide20To20Quantitative20Finance20Interview.604244935.pdf)

> For the strategy to work, one wise man, let's call him the spokesman, will state that every one has been called. What does that tell us? 1. All the other 49 wise men are equivalent (symmetric). 2. The spokesman is different from the other 49 men. So naturally those 49 equivalent wise men should act in the same way and the spokesman should act differently. Here is one of such strategies: Every one of the 49 (equivalent) wise men should flip the glass upside down the first time that he sees the glass bottom down. He does nothing if the glass is already upside down or he has flipped the glass once. The spokesman should flip the glass bottom down each time he sees the glass upside down and he should do nothing if the glass is already bottom down. After he does the 49th flip, which means all the other 49 wise men have been called, he can declare that all the wise men have been called.

Note that the expected time before they can leave is 

#### My solution
I tried to think of a solution that would generalise to other puzzles more easily. While this might or might not be true for the following solution, I wondered which one would allow the wise men to leave earlier.

> Number the wise men from 1 to 50. Assume each wise man keeps track of time, so if he is called in the the 1396th minute, he knows that 1395 men (potentially including himself) were called before him. At times \\(T=50k+t,k\in\mathbb N,t\in\{0,1,\dots,49\}\\) only the \\(t\\)th wise man turns the glass upside down, all others turn it bottom down. This way the wise man chosen at time \\(t+1\\) may learn whether the \\(t\\)th wise man was chosen before him. Eventually one of the wise men will have been chosen directly after each of the other 49 men.

