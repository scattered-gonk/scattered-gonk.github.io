---
layout: default
title:  "DifferentBank"
date:   2025-07-25
description: "qwerty"
author: "GlowDuck"
tags: scam-analysis
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

.cookie_images {
    width: 80%;
    margin: -30px;
    padding: 0px;
}

.homepageimage {
    width: 80%;
    margin: -30px;
    padding: 0px;
}

</style>

<img src="/assets/nail_bite.ico" width="30" height="30">  **ACTIVE CAMPAIGN**   <img src="/assets/nail_bite.ico" width="30" height="30">

Perusing PhishTank, we stumbled on a phishing site configured to appear as the homepage to a bank, complete with informational pages, a contact link, and (most importantly) an account creation link which prompts the user to enter their most personal information.

at the same time, we discovered a similar phishing site:

<img class="homepageimage" src="/assets/homepageimages/acmecredence.png">

and another,

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

<img class="homepageimage" src="/assets/homepageimages/hioutreach.png">
<img class="homepageimage" src="/assets/homepageimages/intersaversfin.png">
<img class="homepageimage" src="/assets/homepageimages/merchantdigital.png">
<img class="homepageimage" src="/assets/homepageimages/vsldemo.png">
<img class="homepageimage" src="/assets/homepageimages/wcbdigivest.png">
<img class="homepageimage" src="/assets/homepageimages/zealfinancial.png">

The bulk of these sites were discovered via a Google Dork using the phrase consistently found on each site's home page:

"We do banking differently. We believe that people come first, and that everyone deserves a great experience every step of the way – whether it’s face to face, over the phone, online or on our app."

for which we thought it appropriate to call the phishing kit DifferentBank :)