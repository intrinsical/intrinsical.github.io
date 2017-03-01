---
layout: post
title: Notes on critical rating and critical chance
category: sto-aso
comments: true
description: An examination of critical rating and critical chance in the Admiralty system. 
tags:
    - sto-aso
---

I have been asked several times about the relationship between critical rating and critical chance in Star Trek Online's admiralty system. I figure it's best to document this down once and for all. This way, the next time someone asks I can simply point to this post.

## Background

This is what Borticus (Cryptic developer) had to say about Admiralty critical rating and how to calculate it.

> 1-(T/(T+C)) is the actual formula.
> How "C" is calculated is as follows:
> =StatExtra*(StatRequired/AllStatTotal)

> In other words, if an Assignment has requirements of 10/10/10 and you slot 10/10/20, you've exceeded one stat by 10. This value is then compared to the total requirement to give you a ratio of contribution, working out as 3.333(repeating).
> So, the total formula is:
> 1-(AllStatTotal/(AllStatTotal+(StatExtra*(StatRequired/AllStatTotal))))
> <footer><cite> - Borticus</cite></footer>
{: .blockquote cite="https://www.reddit.com/r/sto/comments/3qhuoi/dont_send_stronger_ships_then_you_need_to_in_the/cwg3qyq/" }


## Calculating Critical Chance

It turns out to be a pretty simple formula with two only parameters, which I call MinRequirement and CritRate. MinRequirement is simply the minimum points needed to get 100% success at an assignment, so all you need to do is sum the assignment AND event's requirements.

MinRequirement = AssignmentENG + AssignmentTAC + AssignmentSCI + EventENG + EventTAC + EventSCI

Next, calculate CritRatio

$$CritRatio = CritRate / MinRequirement$$

Critical Chance is just the application of a simple Logistic Function.

CritChance = 1 + \left( \frac(-1, \left(1 + \left(\frac(CritRatio,2) \right) \right)

