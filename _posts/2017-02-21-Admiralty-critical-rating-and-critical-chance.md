---
layout: post
title: Admiralty System - A look at critical rating and critical chance
category: blog
comments: true
description: An examination of STO Admiralty system's critical rating and critical chance. 
tags:
    - blog
	- sto-aso
---

I have been asked several times about the relationship between critical rating and critical chance in Star Trek Online's admiralty system. I figure it's best to document this down once and for all. This way, the next time someone asks I can simply point to this post.

## Calculating Critical Chance

It turns out to be a pretty simple formula with two only parameters, which I call $$MinRequirement$$ and $$CritRate$$. MinRequirement is simply the minimum points needed to get 100% success at an assignment, so all you need to do is sum the assignment AND event's requirements.

$$MinRequirement = AssignmentENG + AssignmentTAC + AssignmentSCI + EventENG + EventTAC + EventSCI$$

Next, calculate $$CritRatio$$

$$CritRatio = CritRate / MinRequirement$$

Critical Chance is just the application of a simple Logistic Function.

CritChance = 1 + \left( \frac(-1, \left(1 + \left(\frac(CritRatio,2) \right) \right)

