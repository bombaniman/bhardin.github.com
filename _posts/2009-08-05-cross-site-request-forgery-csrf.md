---
layout: post
title: Cross Site Request Forgery (CSRF)
tags: [vulnerabilities, owasp]
series: the Owasp Top 10
image_url: http://farm4.static.flickr.com/3197/2689052349_07738f5902_b.jpg
image_credit: Joe Penniston
category: post
---
{% include series.html %}

## What is Cross-Site Request Forgery (CSRF)
Cross-Site request forgery is a client-side vulnerability that allows an attacker to make requests on the user's behalf. Although, most CSRF exploits require a user to be authenticated to the susceptible site to be successful, this is not always the case.

## An Example of Cross-Site Request Forgery
A user (victim) opens their browser and logs on to their online banking application located at http://bank.com

After checking their balance they browse away from the site (without logging off) and start reading web pages about olives from Madagascar. One of these olive sites is owned by an attacker. The attacker's website has the following `<img>` tag:

	<img src="http://bank.com/transfer.asp?to_acct=445544&amp;amount=1000">

When the victim's browser loads the malicious page that contains this `img` tag, the victims browser makes the transfer request `/transfer.asp?to_acct=445544&amount=1000` to bank.com using the **authenticated** cookie from the earlier session. Upon making this request, the bank then transfers $1,000 from the victim's account to account 445544. The attacker has now successfully executed a cross-site request forgery attack against a user of bank.com

## How Do You Prevent Cross-Site Request Forgery

Any **sensitive** request that is generated by the user should force the user to "re-authenticate." A simple example is that of change password functionality. You always want to verify the user knows the old password before changing their password, even if they are currently authenticated.

If you determine that "re-authentication" may be an inconvenience for the user or if all of your requests are considered sensitive then the application developers should include a random token that is unique to the user session. This token **should not** be present in the cookie, but rather as a hidden field in the HTML and then appended to the URL during any form submission.

When the attacker attempts to trick the users browser into making a request, the web application will look for this random token. The random token will not exist for the request, and the request will be denied. This prevents the CSRF attack from being successful.

**Note: Having SSL does not protect your application from CSRF vulnerabilities.**