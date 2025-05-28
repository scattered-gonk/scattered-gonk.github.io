---
layout: default
title:  "Tencent-Toll"
date:   2025-05-26
description: "qwerty"
author: "ScatteredGonk"
tags: scam-analysis
---

<img src="/assets/nail_bite.ico" width="30" height="30">  **ACTIVE SCAM**   <img src="/assets/nail_bite.ico" width="30" height="30">

*Sample: https://illinois.gov-jan.vip/pay*
* As of 5/27/2025, there are currently 69 variations of the https://illinois.gov-{three_letters}.vip/pay that are all live and operational. 
* The sites utilize a phishing kit identical to the one discussed in this post.
* The malicious domains remain hosted at Tencent Building, Kejizhongyi Avenue. 
* The phishing kit continues to rely on websockets and hardcoded private/public keys for collecting and receivinf data.

<hr />
<br>
<br>
<br>

<h1> You've probably heard about this </h1>

In the past year, there's been a dramatic influx of scam text messages regarding a supposed "toll notice." I received many myself, all with the following content, verbatum:

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
... if this was true, I had likely incurred enough debt to warrant leaving the country. When I realized the cost of checking in my bags would far exceed the toll charge, I sat down to look into my crime. 

Even with a cursory look-over of the message, we see almost a dozen <span style="color:red">**red flags**</span>, telltale mark of phishing, from introducing a sense of extreme urgency to a <ins>suspicious link</ins>. 

As with other phishing sites of a similar nature, this site would only function when opened on an iPhone. So, moving onto analysis of the phishing site itself proved to require the same workaround as mentioned in previous blog posts: I would have to monitor web traffic and source files via connecting an iPhone to a MacBook, or other safari-enabled computer. Having done so, I was able to retrieve Javascript, CSS and HTML files used by the site.

In addition to this, it appeared that the site was using a web socket connection to send and receive data -- something which will play an extensive role in the site's capabilities. But that's for later.

<h2> What the Victim Sees </h2>

<h2> What the Scammer Sees </h2>

In order to correlate the victim's responses to that same victim, the site generates a unique token to be attributed to the victim. This token is retrieved by vising a URL of the following format:
    https://getipass.com-{four_letters}.xin/front/checkIp?token=123

The response would come in JSON format, as follows:

    {"msg":"success","code":200,"data":{"cardVerify":true,"encryption":true,"isTips":false,"token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpZCI6ODg4MX0._WNVDPP90r8RjVGPlGil5FJUD9kBJXFNji-FkqG9bss"},"success":true,"errorMsg":""}


<h2> Picking It Apart <h2>

To start looking into the site's functionality, we have to deobfuscate the file which handles the bulk of the sending and receiving. We use lelintinh's [de4js](https://lelinhtinh.github.io/de4js/) to reliably accomplish this every time we have to deobfuscate Javascript (we are not associated with lelintinh, just big fans :3). 

Although the Javascript is marginally more readable, all the variables and functions remain largely difficult to read. Instead of normal variable names, we see things like this:

```JS
_0x5ae
```

and after getting through fits of repulsion, we realize that we're in luck. The site has a built-in method for making sense of this jumble.

It does so throughout the script via a common method of client-side deobfuscation implemented by scam sites: a shuffled array of a words.
*The following is all pasted from our Faux IRS post, as we realized we won't be able to describe the process equally as good again, or any better. Enjoy the fruits of our laziness.*

The process depends of three functions:
1. The first returns the entirety of the array
2. The second retrieves an element from the array based on a passed index
3. The third unshuffles the array, so that it can be used by the rest of the scripts 

### get_word_array()
The first function is straightforward. It contains the array, and returns it. For the sake of readability, I'll refer to it as get_word_array()

### get_word_from_array(index)
I will refer to the second function as get_word_from_array(*index*). Before we can dissect how the array is unshuffled, we have to learn about how get_word_from_array(*index*) is used. For instance, take this line:

```js
_0xdeadbeef = "https://"[get_word_from_array(340)]("toll")+get_word_from_array(560)+"/"+get_word_from_array(220)
```

the function get_word_from_array(*index*) retrieves an element from the array, given a specific index. Oftentimes, the function will take the passed index and subtract a hard-coded offset to calculate the element's *true* index, as follows:

```js
function get_word_from_array(index)
{
    var word_array = get_word_array();
    const offset = SOME_NUMBER;
    return word_array[index-offset];
}
```
With this, we can begin to deobfuscate our jumbled code. Using get_word_from_array(*index*), we plug in the arguments seen in the code and get the following:
* get_word_from_array(340) => ".concat"
* get_word_from_array(560) => "scam.com"

So, now we have
```js
_0xdeadbeef = "https://"[".concat"]("toll")+"scam.com"+"/"+"sendCode"
```
By the sacred, ridiculous rules of Javascript we can change ".concat" into the .concat() function due to the brackets enveloping the String. Simplifying the code more, we get:
```js
_0xdeadbeef = "https://".concat("toll")+"scam.com"+"/sendCode"
```
Put it all together, and we get
```js
_0xdeadbeef = "https://tollscam.com/sendCode"
```
likely an endpoint to either send or receive data!

### Unshuffling the Array
We can now approach dissecting the obfuscated array unshuffler. Here's an example of one such unshuffler (I've taken the liberty of deobfuscating it and converting it to Python for readbility and general mental health):


```python
while True:
    try:
        calculated_number = -parse_int(get_word_from_array(372)) / 1 + -parse_int(get_word_from_array(370)) / 2 + parse_int(get_word_from_array(362)) / 3 + -parse_int(get_word_from_array(359)) / 4 * (-parse_int(get_word_from_array(361)) / 5) + -parse_int(get_word_from_array(369)) / 6 + -parse_int(get_word_from_array(368)) / 7 * (parse_int(get_word_from_array(367)) / 8) + -parse_int(get_word_from_array(355)) / 9 * (-parse_int(get_word_from_array(357)) / 10)
        if(calculcated_number == 346304):
            break
        else:
            word_array.append(word_array.pop(0))
    except Exception as e:
        word_array.append(word_array.pop(0))
```

Yowza.

It looks like a lot, but the logic is very simple. We'll make it more readable to make this clear:

```python
while True:
    try:
        num_1 = parse_int(get_word_from_array(372))
        num_2 = parse_int(get_word_from_array(370))
        num_3 = parse_int(get_word_from_array(362))
        num_4 = parse_int(get_word_from_array(359))
        num_5 = parse_int(get_word_from_array(361))
        num_6 = parse_int(get_word_from_array(369))
        num_7 = parse_int(get_word_from_array(368))
        num_8 = parse_int(get_word_from_array(367))
        num_9 = parse_int(get_word_from_array(355))
        num_10 = parse_int(get_word_from_array(357))

        calculated_number = -num_1 / 1 + -num_2 / 2 + num_3 / 3 + -num_4 / 4 * (-num_5 / 5) + -num_6 / 6 + -num_7 / 7 * (num_8 / 8) + -num_9 / 9 * (-num_10 / 10)

        if(calculated_number == 346304):
            break
        else:
            word_array.append(word_array.pop(0))

    except Exception as e:
        word_array.append(word_array.pop(0))
```

The unshuffler takes an existing, "shuffled" array of elements, and begins to run a loop. 

get_word_from_array(*index*) is used to retrieves elements from hardcoded indices, extracting the Integer content from the String (if any). 

If, when plugged into the equation, the result of all these Integer values does not equal a <mark style="background-color: #B0DCEB">Specific Value</mark> (in this instance 346304), the loop continues. The first element of the array is pushed to the back. In the event no Integer values can be extracted, an error occurs, and the same pushing of the first element is carried out.

This cycle continues until the calculcated value is equal to the <mark style="background-color:  #B0DCEB">Specific Value</mark> -- when this occurs, the loop breaks. The array has been "unshuffled"! It can now be used throughout the rest of the script to retrieve the correct elements from the array.

The act of calling get_word_from_array(*index*) must be repeated for almost every line of obfuscated code to determine its true purpose -- this is a process perfect for medidation or hobbyist dissociation.

<h2> URL Endpoints </h2>

/front  
&nbsp;&nbsp;&nbsp;&nbsp;/submit  
&nbsp;&nbsp;&nbsp;&nbsp;/checkIp?token=123  

<h2> Attribution </h2>

Although never an easy thing to do, we can look to several attributes of the phishing kit and sites to try and determine who created the scam, or at the very least, provided the infrastructure and code to facilitate the activities of cyber criminals.

For one, the wide majority of comments sprinkled throughout the code are written in Simplified Chinese. Not only that, but functions which logged errors onto the console featured error messages exclusively in Simplified Chinese. If we ignore the potential for this to be chicken-feed dropped in the code by the malicious actors to drive our attribution down the wrong path, we can infer that the designers of the kit originate from an area in which Simplified Chinese is primarily written, and/or Mandarin Chinese is spoken.

In addition to these elements of the script, the domains themselves are most often hosted by Shenzhen Tencent Computer Systems Company Limited, a Chinese multinational technology conglomerate and holding company headquartered in Shenzhen. 

Of course, we aren't the first to attribute these scams to hackers from China. Warnings from [tabloid news](https://nypost.com/2025/01/28/tech/fake-toll-road-texts-sweep-america-as-chinese-scammers-target-us-drivers/) and [industry professionals](https://krebsonsecurity.com/2025/01/chinese-innovations-spawn-wave-of-toll-phishing-via-sms/) alike cranked the siren to alert us of the sudden onslaught of a massive, almost-fine-tuned SMS phishing campaign. 