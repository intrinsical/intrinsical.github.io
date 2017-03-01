---
layout: post
title: Admiralty critical chance
category: sto-aso
comments: true
description: An examination of critical chance and how to calculate it. 
tags:
    - sto-aso
---

I have been asked several times about the relationship between critical rating and critical chance in Star Trek Online's admiralty system. I figure it's best to document this down once and for all. This way, the next time someone asks I can simply point to this post.

## Reverse Engineering the Admiralty System

When I first started working on ASO, the Admiralty system was still brand new and was only available for testing on Tribble test server. So I had to reverse engineer everything from scratch. 

I played with a number of assignments, plugging in different sets of ships. I recording down the critical chance and critical rate numbers in a comma separated file, and began analyzing it for patterns. 

## What is Critical Rating?

Critical Rating is primarily derived from any "overflow" or surplus stats. For example when you send a very strong ship on a simple admiralty assignment, you will get surplus. Suppose you have an assignment that requires 20 Eng, 20 Tac, 20 Sci. If you send ships that have a combined stat of 25 Eng, 25 Tac, 25 Sci, you have a total surplus stat of 15. This 15 surplus stat becomes your Basic Critical Rating value. Certain ships have special abilities that will further enhance this Critical Rating value. Also, some special events have an innate +Critical Rating bonus that will further increase your Critcal Rating for the assignment.

Your chance of getting a critical success on an assignment is dependent on the Critical Rating value. The more Critical Rating you have, the larger the Critical Chance. However, there is a built in exponential mechanic where you need an ever increasing amounts of Critical Rating to get a larger Critical Chance. The chart below shows the Critical Rating required for an example assignment (20 Eng, 20 Tac, 20 Sci).

![Critical Rating vs Critical Chance]({{ site.url }}/assets/admiralty-crit.png)

To get 10% Critical Chance, you just need 13 Critical Rating. To get 50% Critical Chance, you need 120 Critical Rating. However to get a 80% Critical Chance on the assignment, you need a whopping 480 Critical Rating. Beyond 80% Critical Chance, the Critical Rating requirements get larger at an ever increasing (exponential) rate. In fact, it is impossible to get 100% Critical Chance as you will need infinite Critical Rating.


## A word from Borticus

This is what a STO developer, Borticus, [had to say](https://www.reddit.com/r/sto/comments/3qhuoi/dont_send_stronger_ships_then_you_need_to_in_the/cwg3qyq/) about Admiralty critical rating and how to calculate it.

>
> $$1-\left(\frac{T}{(T+C)}\right)$$ 
>
> is the actual formula.
> How $$C$$ is calculated is as follows:
> 
> $$C = StatExtra*\left(\frac{StatRequired}{AllStatTotal}\right)$$
> 
> In other words, if an Assignment has requirements of 10/10/10 and you slot 10/10/20, you've exceeded one stat by 10. This value is then compared to the total requirement to give you a ratio of contribution, working out as 3.333(repeating).
> So, the total formula is:
>
> $$1 - \left( \frac{AllStatTotal}{\left(AllStatTotal + \left(StatExtra* \left(\frac{StatRequired}{AllStatTotal}\right)\right)\right)}\right)$$
> 
><footer><cite> - Borticus</cite></footer>
{: .blockquote cite="https://www.reddit.com/r/sto/comments/3qhuoi/dont_send_stronger_ships_then_you_need_to_in_the/cwg3qyq/" }


## My Take on Critical Chance

Of course, back then the Admiralty system was brand new and I had no information on any of the details. So I had to reverse engineer it. 
It turns out to be a pretty simple formula with two only parameters, which I call $$MinRequirement$$ and $$CritRate$$. $$MinRequirement$$ is simply the minimum points needed to get 100% success at an assignment, which is the sum of both the assignment AND event's requirements.

$$
\begin{align*}
MinRequirement & = AssignmentENG + \\
 & AssignmentTAC + \\
 & AssignmentSCI + \\
 & EventENG + \\
 & EventTAC + \\
 & EventSCI
\end{align*}
$$

Next, calculate $$CritRatio$$

$$CritRatio = \frac{CritRate}{MinRequirement}$$

Critical Chance is just the application of a simple Logistic Function.

$$CritChance = 1 + \left( \frac{-1}{\left(1 + \frac{CritRatio}{2}\right)} \right)$$

