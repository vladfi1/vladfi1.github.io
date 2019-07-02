---
layout: posts
title: Victim of your Own Success
---

## Introduction

Sometimes your own successes come back to hurt you. My dad likes to reference the titular phrase with regards to his project proposals -- if they succeed, he'll have a mountain of work ahead of him. Another example is [automation](https://medium.com/the-atlantic/the-coders-programming-themselves-out-of-a-job-c8dcdd58a025). Someone who puts in a lot of work and successfully automates their job might end up losing it.

In this post I'm going to talk about a simple game-theoretic scenario that illustrates this phenomenon.

First, let's introduce the concept of **Nash equilibrium**. We'll consider symmetric, zero-sum, 2-player games. In such a game, each player picks a move simultaneously. These are then revealed, and each player deterministically receives a payout as a function of the two moves. The game is *symmetric* when the two players have the same move set, and swapping moves swaps the payoffs. It is *zero-sum* if the payoffs add up to zero -- that is, however much I win, you must lose, and vice-versa. Such a game can be represented by an antisymmetric matrix of payoffs for each move combination.

The classic example of this class of game is Rock-Paper-Scissors (RPS), where wins are +1, losses -1, and ties 0. But actually quite a lot of games can be seen as falling under this category, if we relax the constraint on determinstic payoffs. Take, for example, Super Smash Bros., a popular series of fighting games from Nintendo. Smash players love to make tier lists ranking the different characters in terms of competitive viability, both on an absolute scale and on a matchup-by-matchup basis. For example, [this chart](https://docs.google.com/spreadsheets/d/1uekrvvZUDFGphbZ7LAyXpyspOuqvNX7I2HZvtwAoWSQ) shows that Fox beats Peach 60-40. What this means is that if two skilled players fight with Fox vs Peach, the Fox *should win* 60% of the time on average. So, we can view Smash Bros. as a symmetric, zero-sum, 2-player game with *stochastic* payoffs, where the "moves" are the possible characters that can be picked.

The obvious question to ask about any game is: what's the best way to win? But if we look at RPS, we'll have trouble answering that, as any move we make could get countered. Instead, we'll have to consider *mixed strategies* -- that is, probability distributions over moves. We'll consider a mixed strategy "optimal" if doesn't lose, in [expectation](https://en.wikipedia.org/wiki/Expected_value), to any other mixed strategy. In RPS, the optimal mixed strategy is to choose each move with equal probability.

When both players choose the optimal mixed strategy, the expected result is 0-0, and neither player will be able to do any better by switching to a different strategy. In general, such a scenario is known as a [Nash equilbrium](https://en.wikipedia.org/wiki/Nash_equilibrium) -- in an n-player game, it is a set of mixed strategies for each player such that no one would prefer to deviate to a different strategy than the one they already have.

For advanced readers: show that Nash equlibria for symmetric zero-sum 2-player games can be computed with linear programming.

## Games and Metagames

Another place Nash Equlibria show up is in collectible card games like Magic and Hearthstone. These games typically have a ladder system where players make a deck out of the cards they own, and then queue up to face some other random player of similar skill level. There certainly is a lot of strategy that goes into making decisions during the game itself, but a second *meta-level* game is being played before any card is dealt: the game of building your deck. Over time this *metagame* gets refined as players optimize decklists and figure out the matchup spread for each deck. Even after the best decks have generally been discovered, a lot of tweaking and fine tuning happens as players try to pick the deck that will do well against the set of decks that everyone else is currently playing. Like a market seeking [efficiency](https://en.wikipedia.org/wiki/Efficient-market_hypothesis), the efforts of each player to maximize their win-rate slowly shifts the distribution of decks played on ladder to an equlibirum -- a Nash equilibrium.

Let's consider how this plays out in practice. I'm playing Hearthstone, and I'm noticing that more often than not my opponents on the ladder are playing aggressive ("aggro") decks. Being knowledgeable of the metagame, I decide to switch to a control deck which has a favorable matchup against aggro. Other smart players do this too, and over time the proportion of control decks increases. The aggro players notice that they are being countered, and decide to switch to midrange, which counters the growing number of control decks. And it all comes full circle as the control players respond by switching to aggro to beat the midrangers. Given enough time, these fluctuations average out and the overall ratios of aggro, midrange, and control stabilize to a point where no one can gain any more by switching.

## A Puzzle: (Un-)Balancing the Meta

And now for the fun part. Let's consider a fictitious scenario where Blizzard is monitoring the Hearthstone meta, and for whatever reason, decides that a certain deck "A" should be played more. The current state of the meta has A, B, and C in a soft-RPS scenario: A beats B 60% of the time, B beats C 60% of the time and C beats A 60% of the time. By symmetry, the Nash equilibrium here has all three decks played equally. So, Blizzard gives A a new card that increases the winrate against B to 70%, but, to not be overpowered, doesn't affect its matchup against C (of course, in practice it would be impossible to achieve such a precise effect by introducing a whole new card). Before the change takes effect, try to predict how the meta would shift. Which decks would see more play, and which less?

(I originally heard this puzzle from my friend Jon Schneider in the context of SSBM: suppose Marth > Fox 60-40, Fox > Sheik 70-30, and Sheik > Marth 60-40.)

### Solution

This is just some filler text so that you don't immediately see the solution. Scroll up if you don't want to be spoiled! Even if you can't figure it out intuitively, you can try to solve for the solution with linear algebra and gain some insight that way.

----

Surprisingly, the playrate of A should actually *go down* to 25%, along with B. It is in fact C who benefits from this change, with its playrate going up to 50%. My intuitive "explanation" goes like this: first, the playrate of A shouldn't go below that of B (that would be crazy!), and the playrate of B should decrease. In response to the decreasing playrate of B, C should increase. The increase in C and decrease in B are both bad for A, so A might stay the same or decrease. This gives you the C > A >= B ordering.

A more rigorous explanation starts by noticing that, due to their anti-symmetric matchups vs C, in an equilibrium A must equal B. If A > B, then C will always perform better than average, and so should increase. And vice-versa, if A &lt; B, then C will do worse than average, and so should decrease. In either case the "equilibrium" is unstable and must shift to a new point. So, the equilibrium has A=B, and the question is whether they go down or up together. Either way, for the probabilities to sum to 1, A and B must move by a smaller amount (say, x) than C (-2x). If they go down, then A still breaks even because its strong 70-30 matchup vs B makes up for the more common but less skewed 40-60 matchup vs C, and vice-versa for B -- there's lots of favorable C to make up for the bad matchup vs A. But if they go up it'll look too good for A (lots of B and very little C) and too bad for B. So, A and B must go down and C must go up.

In the general case, if the A-B matchup is shifted in A's favor by 10x%, then C:A:B ratio will be (x+1):1:1.

And now for the punchline: it could be said that A was a victim of its own success.

