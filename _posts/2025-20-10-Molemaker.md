---
layout: threat-intel-default
title:  "Molemaker"
date:   2025-10-20
description: "qwerty"
author: "GlowDuck"
tags: threat-intelligence
---
<h1> Overview </h1>
A significant number of malware and phishing campaigns rely on the Telegram bot API to exfiltrate victim data. A number of native Telegram API endpoints are used to accomplish this: /getMe, /getUpdates, and /sendMessage among others. Due to the nature of these publicly-usable endpoints, researchers have the ability to both view information regarding the Telegram bot (username, privileges, user ID) and Telegram channel (number of members, restrictions, etc.), as well as recover the stolen data. This is achieved via forwarding messages from attacker-controlled chats to researcher-controlled chats -- to faciliate this process, we have developed Molemaker.