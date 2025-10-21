---
layout: threat-intel-default
title:  "Molemaker"
date:   2025-10-20
description: "qwerty"
author: "GlowDuck"
tags: threat-intelligence
---

<h1> Overview </h1>

Many current phishing campaigns rely on Telegram bots to exfiltrate the victim's data to attacker-controlled Telegram channels. A number of native Telegram API endpoints are used to accomplish this: /getMe, /getUpdates, and /sendMessage among others. Due to the nature of these publicly-usable endpoints, we as researchers have the ability to both view information regarding the Telegram bot (username, privileges, user ID) and Telegram channel (number of members, restrictions, etc.), as well as recover the stolen data via forwarding messages from attacker-controlled chats to researcher-controlled chats. To faciliate this process, we have created the Molemaker command-line tool.