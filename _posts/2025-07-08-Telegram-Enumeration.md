---
layout: threat-intel-default
title:  "Telegram Enumeration"
date:   2025-08-07
description: "qwerty"
author: "RedDuck"
tags: threat-intel
---

Many phishing sites rely on the Telegram bot API to exfiltrate victim data. Oftentimes, eCriminals are either
<ul>
    <li> deploying half-baked phishing kits they have purchased from somebody with a username akin to "__1337__phisher", or </li>
    <li> inexperienced and looking to make a quick profit with minimal effort (and likely more than a pinch of vibe-coding) </li>
</ul>

Fortunately for us, this means they often leave their Telegram bot API tokens publicly accessible, either through
<ul>
    <li> hard-coding it into the site's page source </li>
    <li> exposing it in POST requests and responses </li>
</ul>

With this token (and oftentimes, the accompanying channel ID to which the information is sent), we can use the native Telegram bot API to enumerate data pertaining to the attacker's operation, and what data they have stolen so far.

THE FOLLOWING WORKFLOWS SHOULD NOT BE USED AGAINST NON-ATTACKER CHANNELS. WE ONLY ANALYZE SITES SUBMITTED TO PHISHTANK.ORG, AND ARE CONFIMRED TO BE VALID PHISHES.

<script src="https://cdn.jsdelivr.net/npm/prismjs@1.9.0/prism.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/prismjs@1.9.0/components/prism-markdown.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/prismjs@1.9.0/components/prism-markup.js"></script>

<pre><code class="language-python">
from datetime import datetime
import requests
import time
import os

USING = "BOT_TOKEN1"

BOT_TOKENS = {"BOT_TOKEN1": "{BOT-TOKEN}"}

CHANNEL_IDS = {"BOT_TOKEN1": "{BOT-CHANNEL}"}

MY_CHANNEL_IDS = {"BOT_TOKEN1": "{YOUR-CHANNEL}"}

URL = f"https://api.telegram.org/{BOT_TOKENS[USING]}/" 
</code></pre>

<!-- ```python
from datetime import datetime
import requests
import time
import os

USING = "BOT_TOKEN1"

BOT_TOKENS = {"BOT_TOKEN1": "{BOT-TOKEN}"}

CHANNEL_IDS = {"BOT_TOKEN1": "{BOT-CHANNEL}"}

MY_CHANNEL_IDS = {"BOT_TOKEN1": "{YOUR-CHANNEL}"}

URL = f"https://api.telegram.org/{BOT_TOKENS[USING]}/" 

# Workflow that will gather data specific to the channel that the attackers are using to exfiltrate data. If the channel ID is not known, you can use 
# determine this value by interacting with the attacker's bot; the response you receive will likely contain the ID of the channel used to exfiltrate victim data
channel_endpoints = {
    "getChatAdministrators":f"getChatAdministrators?chat_id={CHANNEL_IDS[USING]}",
    "getChat":f"getChat?chat_id={CHANNEL_IDS[USING]}",
    "getChatMembersCount":f"getChatMembersCount?chat_id={CHANNEL_IDS[USING]}",
    "createChatInviteLink":f"createChatInviteLink?chat_id={CHANNEL_IDS[USING]}",
}

# Workflow that will gather data on the bot, such as 
# - Name
# - Recent updates
# - Description
# and other general information that can be used later.
gen_endpoints = {
    "getUpdates":"getUpdates",
    "getWebhookInfo":"getWebhookInfo",
    "getMe":"getMe",
    "getMyCommands":"getMyCommands",
    "getMyName":"getMyName",
    "getMyDescription":"getMyDescription",
    "getAvailableGifts":"getAvailableGifts",
    "getWebhookInfo":"getWebhookInfo",
}

# Workflow that will remove evidence of your interaction with the attacker's bot. These endpoints will
# - Leave the chat you created to enumerate stolen data
# - Close the bot
# - Log the bot out
clear_tracks = {
    "leaveChat":f"leaveChat?chat_id={MY_CHANNEL_IDS[USING]}",
    "close":f"close",
    "logOut":f"logOut"
}

# Workflow that will send data specified by you to the attacker's channel. By default, this workflow sends empty messages, and disables notifications, so as to minimize the likelihood that the attacker will immediately detect any anomolous behavior. You may use this workflow to pollute the attacker's
# dataset, enumerate channel ID(s), etc.
sendMessage = {
    "sendMessage":f"sendMessage?text=''&chat_id={MY_CHANNEL_IDS[USING]}&disable_notification=True"
}

# Workflow to enumerate what data the attackers have stolen. 
forwardMessage = {
    "forwardMessage": f"forwardMessage?chat_id={MY_CHANNEL_IDS[USING]}&from_chat_id={CHANNEL_IDS[USING]}&message_id="
}

folder = f"{USING}-{datetime.today().strftime('%m-%d-%y')}"
try:
    os.mkdir(f"{folder}")
except:
    print("Folder already exists")
os.chdir(f"{folder}")

def enumerate():
    for inc in range(X,Y): 
        for key in forwardMessage.keys():
            looking_at = key
            response = requests.get(URL+forwardMessage[looking_at]+str(inc))
            print(response.text)
            if "Too Many Requests: retry after" not in response.text and "Bad Request: message to forward not found" not in response.text:
                with open(f"{looking_at}.json","a") as a:
                    a.write(f"{response.text}\n")
    
def query_endpoint(endpoint):
    for key in endpoint.keys():
        looking_at = key
        response = requests.get(URL+endpoint[looking_at])
        print(response.text)
        if '{"ok":true' in response.text:
            with open(f"{looking_at}.json","a") as a:
                a.write(f"{response.text}\n")

def initial_recon():
    query_endpoint(gen_endpoints)
    query_endpoint(channel_endpoints)

STAGE = "3"

match STAGE:
    case "1":
        initial_recon()
    case "2":
        enumerate()
    case "3":
        query_endpoint(clear_tracks)

os.chdir("..")
``` -->