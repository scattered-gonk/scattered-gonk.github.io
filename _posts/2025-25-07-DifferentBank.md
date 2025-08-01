---
layout: phishing-campaign-default
title:  "DifferentBank"
date:   2025-07-25
description: "qwerty"
author: "GlowDuck"
tags: phishing-campaigns
---


<style>

.overlay {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background: rgba(0, 0, 0, 0.7);
  transition: opacity 500ms;
  visibility: hidden;
  opacity: 0;
}
.overlay:target {
  visibility: visible;
  opacity: 1;
}

.popup {
  margin: 70px auto;
  padding: 20px;
  background: #fff;
  border-radius: 5px;
  width: 30%;
  position: relative;
  transition: all 5s ease-in-out;
}

.popup h2 {
  margin-top: 0;
  color: #333;
  font-family: Tahoma, Arial, sans-serif;
}
.popup .close {
  position: absolute;
  top: 20px;
  right: 30px;
  transition: all 200ms;
  font-size: 30px;
  font-weight: bold;
  text-decoration: none;
  color: #333;
}
.popup .close:hover {
  color: #06D85F;
}
.popup .content {
  overflow-x:hidden;
  overflow-y:auto;
  width: 90%;
  min-height:200px;
  max-height:250px;
}

@media screen and (max-width: 700px){
  .box{
    width: 70%;
  }
  .popup{
    width: 70%;
  }
}

.homepageimage {
    width: 80%;
    padding: 0px;
}

.container {
    width: 80%;
    padding: 0px;
}

</style>

<img src="/assets/nail_bite.ico" width="30" height="30">  **ACTIVE CAMPAIGN**   <img src="/assets/nail_bite.ico" width="30" height="30">

Researchers at Squeaky Flotilla have come across a spattering of nearly-identical phishing sites configured to appear as the homepage to a bank, complete with
* an informational pages, 
* a contact link, 
and (most importantly) 
* an account creation link which prompts the user to enter their most personal information

Initial links were found on PhishTank.org, a Cisco Talos-operated public database of potential and active phishing links that is regularly updated. After encountering the first such site

<img class="homepageimage" src="/assets/homepageimages/acmecredence.png">

we came across another,

<img class="homepageimage" src="/assets/homepageimages/alderstone.png">

and another,

<img class="homepageimage" src="/assets/homepageimages/acetrust.png">

and a couple more

<img class="homepageimage" src="/assets/homepageimages/arvestlitebank.png">
<img class="homepageimage" src="/assets/homepageimages/crescentunion.png">
<img class="homepageimage" src="/assets/homepageimages/cti.png">
<img class="homepageimage" src="/assets/homepageimages/gotrustcredit.png">
<img class="homepageimage" src="/assets/homepageimages/guardiantrust.png">

... and another few :(

<img class="homepageimage" src="/assets/homepageimages/intersaversfin.png">
<img class="homepageimage" src="/assets/homepageimages/merchantdigital.png">
<img class="homepageimage" src="/assets/homepageimages/vsldemo.png">
<img class="homepageimage" src="/assets/homepageimages/wcbdigivest.png">
<img class="homepageimage" src="/assets/homepageimages/zealfinancial.png">

Following the initial PhishTank-sourced sites, we noticed that the sites consistently used the phrase

<blockquote>
<p>"We do banking differently. We believe that people come first, and that everyone deserves a great experience every step of the way – whether it’s face to face, over the phone, online or on our app."
</blockquote>
on their home page; as such, a Google Dork of this phrase revealed the majority of these malicious sites to us. Their insistence of not being like the other banks earned the phishing kit its name: DifferentBank :)