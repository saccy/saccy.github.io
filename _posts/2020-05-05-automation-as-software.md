---
layout: post
title: Automation as software
description: Automation as software
summary: Automation code is software that often lacks attention to detail
comments: true
tags: [cicd, automation]
---

The words automation and programming are synonymous, but when we say automation today in the context of software we're talking about things like DevOps, pipelines, config management and API integrations to name but a few. Automation code often lacks the attention to detail that traditional application development receives.

When we think about why automation is on trend and what it can do for business, we're thinking about reducing time and effort and increasing profitability. From my experience, is this thought of effort reduction then associated with the *act* of, for example, developing a pipeline to build, test, and deploy your application? Yes. Ironically, automation code is oftentimes hardcoded, untested and duplicated throughout repos. *'Do as I say, not as I do'*.

This lack of attention to detail manifests itself in basic programming principles that are quite rare to see in pipelines such as classes, functions, error handling and shared libraries. This doesn’t necessarily mean that the pipeline will be trash and in fact it will probably fulfil its purpose well, however whenever we need to change the code or re-use it somewhere else we’re limiting our ability to do that easily which can lead to duplication of code and a codebase that is harder to maintain.

So how do we go about giving this overlooked area the love it deserves? We need to recognise that writing automation code should follow the same good practice we apply elsewhere, and likewise to give it the *time* required to nail it down. This is important as project time is often prioritised for other components. If you're working for someone who doesn’t see the value in *quality* then you need to *quantify* the benefits of what you're doing, i.e. how much time and money will be saved (both in the short and long run) when you do something the right way.

Hopefully the next time your project manager is on your case about completing some devops component, you can take some of the language used here and learn em gud. 
