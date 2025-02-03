---
layout: post
title: "Getting infinite money inside the platform Through Race Condition"
description: "Breaking account activation logic and earning rewards in an uncontrolled way in bug bounty."
date:   2025-02-03 09:01:35 +0300
image:  '/images/991.jpeg'
tags:   [web, bug bounty]
---

Hello Hackers, how are you? Today I come to present a vulnerability that I recently found during a bug bounty hunt, it was a vulnerability with a very cool impact involving money... Let's go!!

#### Introduction
About two weeks ago, I received a private invitation to a program that had launched in 2024 but had only a handful of accepted reports—fewer than ten. Intrigued, I decided to take a closer look at the application.

The platform in question was a gaming service where users had to top up their accounts with the platform’s currency to make purchases. So far, so good… However, things started to get interesting when I came across a rule stating that I would receive 200 in-platform currency upon confirming my account via email.

- Think about it: what if I try to activate my account several times at the same time?

This was a valuable insight, because until now, no Hunter had thought of it and done it... So, can we exploit this functionality?

Let's do it!!

![](https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExbW5oOHY4ZGV4NXJibXJlZHYzeTM4NWk5dWF0bWk3YjBtenBjcXp0ZiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/78XCFBGOlS6keY1Bil/giphy.gif)


### Proof of Concept
To start, let's create our test account...

![]({{site.baseurl}}/images/992.png)

Account created, we will ask you to resend the account activation email (The application apparently had some problem sending us the email...).

![]({{site.baseurl}}/images/993.png)

In the activation email, we received a confirmation link. We will copy it, intercept the request to prevent accidental activation, and then send it to Turbo Intruder.
![]({{site.baseurl}}/images/994.png)

![]({{site.baseurl}}/images/995.png)

Let's now configure the turbo intruder by injecting a custom header into the request.

![]({{site.baseurl}}/images/996.png)

Now, let's start the attack. If everything goes as expected, we will be able to activate our account multiple times in the same space of time, preventing the application from performing any validation checks related to the fact that our account is already active. With this, we will be able to repeatedly trigger the functionality that adds 200 coins to the account on the platform.

![]({{site.baseurl}}/images/997.png)

Apparently some requests gave code 200, I think it worked... Let's check our account...

![]({{site.baseurl}}/images/998.png)

Yes... WE DID IT!

We were able to exploit the flaw in the account verification logic activated via Race Condition by activating our account multiple times simultaneously. This prevents the application from having time to properly validate whether the account has already been activated.

The vulnerability is triaged and the vulnerability has been fixed

![]({{site.baseurl}}/images/bounty.png)

See you next time, don't forget to connect with me on LinkedIn:
https://www.linkedin.com/in/eduardo-maragno/

![](https://media0.giphy.com/media/v1.Y2lkPTc5MGI3NjExajlrdGNxdmQ5azVobm91bjY4NmM5YXY4eTVhYWY4MHplYmh0cHN5NyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/o0vwzuFwCGAFO/giphy.gif)