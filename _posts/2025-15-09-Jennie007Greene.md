---
layout: scam-analysis-default
title:  "Jennie007Greene"
date:   2025-09-15
description: "qwerty"
author: "GlowDuck"
tags: scam-analysis
---

As of 9/14/205, several distinct Wordpress domains submitted to the OpenPhish.com database, urlscan.io, and other artifact collection platforms have been infected by threat actors with the Jennie007Greene.php web shell. For the purpose of protecting these domains from further exploitation and defacement, no specific domains will be discussed; however, general Indicators of Compromise are available below.

Multiple compromised domains allow the threat actor wide swathes of access to victim emails, usernames, and passwords. In addition to recording phishing campaign credentials in .txt files, the attackers used the Telegram bot API to exfiltrate sensitive victim data.

Files frequently found alongside Jennie007Greene.php included a configuration file that specified the format in which data would be sent via Telegram bot; a section of this formatted message was consistently observed to include the string "@SMARTMACBOOKBOT*TFCU". Inspecting the data enumerated from attacker-controlled chats, we determined that messages containing this string began appearing on August 21st of 2025, though was preceeded by messages without the string. We may therefore mark 08/21/25 aa a general point at which this iteration of the threat actor's phishing campaigns started.

Indicators of Compromise:
<ul>
    <li>Domains that are part of the phishing campaign are structured as follows:
        <ul>
            <li>hxxps[://]domain[.]com/backup/Jennie007Greene[.]php </li>
            <li>hxxps[://]domain[.]com/backup/.../{random word}-access/{page name}[.]html</li>
        </ul>
    </li>
    <li> Endpoints commonly seen accompanying Jennie007Greene.php:
        <ul>
            <li>hxxps[://]domain[.]com/.../ss[.]html</li>
            <li>hxxps[://]domain[.]com/.../details[.]html</li>
            <li>hxxps[://]domain[.]com/.../otp[.]html</li>
            <li>hxxps[://]domain[.]com/.../personal[.]html</li>
            <li>hxxps[://]domain[.]com/.../otp2[.]html</li>
            <li>hxxps[://]domain[.]com/.../otp3[.]html</li>
        </ul>
    <li> Hashes of Jennie007Greene.php:
        <ul>
            <li>3bb4dd755a1495466d09c9a3767b13f6</li>
            <li>758f8a46738f0167b10610558e2c675ada0543c7</li>
            <li>66070940681148e6f80640dcd8d194cd4b54cbf614355130765d561423cb337d</li>
            <li>9c7f37eabc54c3ffe84cc74377e6870294f95e83cab91e7f17479e74eba7ead3189905dcd917f6141dfeb7a1873acab0afd9f4da37ea961c6e012d0d7d2e5285</li>
        </ul>
    </li>
    <li> Common hash IOCs between domains (as per urlscan.io):
        <ul>
            <li>06dee56fb4e2677948bc2f6ce7e20e9900e3c7431843ae3d9c9d975ff03889a7</li>
            <li>4e2bcd3f2e35e841bece706f0426cc746ac77cfd2148ea365ce05eebd5124a45</li>
            <li>75d3513cd72651dab00071d36b00c1546142fa141167f7fc770af9bce061028e</li>
            <li>97895393404cac42e8bcf1a936689ae21d265cbf820816b18d0ab6f490feca6a</li>
            <li>a0ed31680bc7f4621bdc1189abb0930e6e3213c00920003c0c7bd766e1d06e09</li>
            <li>a44ee19cced78f183c40a52367159e6e5284ee2367f5ea06f72bfd64feaff848</li>
            <li>b732cce907baa80850c8857d858358284883f9ea9b7239fc6a5ed6e7eaa9b5b1</li>
            <li>bccb23d41c2cc69cf0c7d22c4314ca8181a513c6999b73e45307792830f4e482</li>
            <li>e691f5f15c1d8e90005c96d016c3106f111d44b42bd210491af925dc68e4e979</li>
            <li>e80a28e260de3fa02ff629d2ae4a84c50a5e159f40807ca8c61b108cb2899880</li>
            <li>f555d3efaa4e368224cc19b0b261b00da4183e8a5247d3858e8ce7e2aa764558</li>
            <li>f5b383a6ca50aad2cd676d209b104cff2cb429d08f46ed5021d83e67e6597139</li>
            <li>f6734f8177112c0839b961f96d813fcb189d81b60e96c33278c1983b6f419615</li>
            <li>f6fca06e2aac270b488f73bcf0a10d249e2722a015135e60dbb49360c5335a72</li>
            <li>fd76f1e358e7d8c0f93f6c683060682e3423c5d7c967ce38724ccd7bb60fa0c1</li>
        </ul>
    </li>

In addition, all observed instances of config.php files present alongside Jennie007Greene.php contain identical Telegram bot API tokens and channel IDs. This indicates that a single entity -- individual or not -- may be compromising sites and deploying Jennie007Greene.php, rather than it being part of a publicly sold phishing kit.

An in-depth analysis of Jennie007Greene.php will be available shortly.
