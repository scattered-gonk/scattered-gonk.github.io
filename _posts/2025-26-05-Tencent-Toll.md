---
layout: default
title:  "Tencent-Toll"
date:   2025-05-26
description: "qwerty"
author: "ScatteredGonk"
tags: scam-analysis
---

<img src="/assets/nail_bite.ico" width="30" height="30"> **  ACTIVE SCAM  **<img src="/assets/nail_bite.ico" width="30" height="30">

*Sample: https://illinois.gov-jan.vip/pay*
* As of 5/27/2025, there are currently 69 variations of the https://illinois.gov-{three_letters}.vip/pay that are all live and operational. 
* The sites utilize a phishing kit identical to the one discussed in this post.
* The malicious domains remain hosted at Tencent Building, Kejizhongyi Avenue. 
* The phishing kit continues to rely on websockets and hardcoded private/public keys for collecting and receivinf data.

<img src="/assets/nail_bite.ico" width="30" height="30"> **  ACTIVE SCAM  **<img src="/assets/nail_bite.ico" width="30" height="30">
<hr />
<br>
<br>
<br>
In the past year, there has been a dramatic influx of scam messages arriving at people's phones regarding a supposed "toll notice." I received many myself, all with the following message, verbatum:

<span style="color:red">**Toll Violation Notice:**</span>

This is your <span style="color:red">**final notice**</span> regarding this unpaid toll balance on your account. You <span style="color:red">**must settle**</span> the balance within the <span style="color:red">**next 12 hours**</span> to avoid <span style="color:red">**severe penalties**</span>.

Unpaid: $2.17
Due Date: April 19, 2025

Failure to pay within this time frame will result in the following:

1. <span style="color:red">**Immediate addition of late payment fees**</span> to your balance
2. <span style="color:red">**Suspension of your vehicle registration**</span> by the Department of Motor Vehicles (DMV)
3. Collection actions, including a <span style="color:red">**negative report to your credit file**</span>

Please make your payment promptly to <span style="color:red">**avoid these severe consequences and protect your driving privileges**</span>.

Pay Now:
<ins>https://getipass.com-tdi.top/us</ins>

If the link fails, reply with 'Y', exit the SMS, and reopen it to activate the link, or copy and paste it directly into your browser to complete your payment.

<span style="color:red">**This is your last opportunity. Pay now to avoid irreversible consequences**</span>.

<br>
<br>
... if this was true, I had likely incurred enough debt to warrant leaving the country. Just in case, I sat down to look into the crime I had committed. 

Even with an initial look-over of the message, we see enough <span style="color:red">**red flags**</span> to ruin the strongest budding relationship: we cann see almost every telltale mark of phishing, from introducing a sense of extreme urgency to a <ins>suspicious link</ins>. 

As with other phishing sites of a similar nature, this site would only function when opened on an iPhone. So, moving onto analysis of the phishing site itself proved to require the same workaround as mentioned in previous blog posts: I would have to monitor web traffic and source files via connecting an iPhone to a MacBook, or other safari-enabled computer. Having done so, I was able to retrieve Javascript, CSS and HTML files used by the site.

In addition to this, it appeared that the site was using a web socket connection to send and receive data -- something which will play an extensive role in the site's capabilities. But that's for later.

<h2> What the Victim Sees </h2>

<h2> What the Scammer Sees </h2>

In order to correlate the victim's responses to that same victim, the site generates a unique token to be attributed to the victim. This token is retrieved by vising a URL of the following format:
    https://getipass.com-{four_letters}.xin/front/checkIp?token=123

The response would come in JSON format, as follows:

    {"msg":"success","code":200,"data":{"cardVerify":true,"encryption":true,"isTips":false,"token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpZCI6ODg4MX0._WNVDPP90r8RjVGPlGil5FJUD9kBJXFNji-FkqG9bss"},"success":true,"errorMsg":""}


<h2> URL Endpoints </h2>

/front  
&nbsp;&nbsp;&nbsp;&nbsp;/submit  
&nbsp;&nbsp;&nbsp;&nbsp;/checkIp?token=123  

<h2> Attribution </h2>

Although never an easy thing to do, we can look to several attributes of the phishing kit and sites to try and determine who created the scam, or at the very least, provided the infrastructure and code to facilitate the activities of cyber criminals.

For one, the wide majority of comments sprinkled throughout the code are written in Simplified Chinese. Not only that, but functions which logged errors onto the console featured error messages exclusively in Simplified Chinese. If we ignore the potential for this to be chicken-feed dropped in the code by the malicious actors to drive our attribution down the wrong path, we can infer that the designers of the kit originate from an area in which Simplified Chinese is primarily written, and/or Mandarin Chinese is spoken.

In addition to these elements of the script, the domains themselves are most often hosted by Shenzhen Tencent Computer Systems Company Limited, a Chinese multinational technology conglomerate and holding company headquartered in Shenzhen. 

Of course, we aren't the first to attribute these scams to hackers from China. Warnings from [tabloid news](https://nypost.com/2025/01/28/tech/fake-toll-road-texts-sweep-america-as-chinese-scammers-target-us-drivers/) and [industry professionals](https://krebsonsecurity.com/2025/01/chinese-innovations-spawn-wave-of-toll-phishing-via-sms/) alike cranked the siren to alert us of the sudden onslaught of a massive, almost-fine-tuned SMS phishing campaign. 