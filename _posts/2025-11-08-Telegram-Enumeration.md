---
layout: threat-intel-default
title:  "Telegram Enumeration"
date:   2025-08-11
description: "qwerty"
author: "RedDuck"
tags: threat-intelligence
---

<style>
    pre{
  counter-reset:line-numbering;
  background:#2c3e50;
  padding:20px 20px 20px 20px;
  width:700px;
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

<h2> Turning Loyal Bots into Moles </h2>

Many threat actors and eCriminals rely on the Telegram bot API in order to exfiltrate victim data. This process requires two core elements:
<ul>
    <li> A unique Telegram bot API token </li>
    <li> The unique Channel or Group ID to which the data will be sent </li> 
    
While certain entities keep these sensitive artifacts hidden, others neglect to do so, either because they are
<ul>
    <li> deploying half-baked phishing kits they've purchased from somebody called "__1337__phisher", or are</li>
    <li> inexperienced and looking to make a quick profit with minimal effort (and likely more than a pinch of vibe-coding) </li>
</ul>

Fortunately for defenders, this means that threat actors often leave both their Telegram bot API tokens and Channel/Group IDs publicly accessible, through
<ul>
    <li> hard-coding it into the site's page source, or </li>
    <li> exposing it via POST requests and responses </li>
</ul>

With this information, we can use the native Telegram bot API to enumerate the data collected by the attacker's operation. With this purpose in mind, we developed Molemaker.

<h2> Molemaker </h2>

The creator of most Telegram bots -- if not all -- is the all-powerful BotFather... so we decided to turn his loyal children into silent informants on their operators.

All content covered here has been uploaded to the Molemaker repository on our main GitHub page.

<h2> Storing The Data </h2>

All data enumerated by Molemaker, ranging from information about the bot's channel to the compromised data itself, is stored in a folder

<pre>
    def create_folder(self):
        import requests
        from datetime import datetime
        import os
        response = requests.get(f"{self.URL}getMe")
        formatted = response.json()
        bot_username = formatted["result"]["username"]
        folder = f"{bot_username}-{datetime.today().strftime('%m-%d-%y')}"
        exists = False
        try:
            os.mkdir(f"{folder}")
        except:
            print("Folder already exists")
            exists = True
        os.chdir(f"{folder}")
        return exists
</pre>

<h2> Interacting With Telegram API Endpoints </h2>

Although the data enumeration of Molemaker requires more precise interaction with Telegram API endpoints, more general queries (e.g., "getMe", "getUpdates", etc.) are passed through one function

<pre>
    def query_endpoint(self, endpoint):
        import requests

        for key in endpoint.keys():
            response = requests.get(self.URL+endpoint[key])

            print(f"Queried {key}, response was: {response.text}")

            if '{"ok":true' in response.text:
                with open(f"{key}.json","a") as a:
                    a.write(f"{response.text}\n") 
</pre>

If any queries are unsuccessful, they are not recorded as part of the reconnaissance process.

<h2> Making Bots Into Moles </h2>

The "auto" mode of Molemaker aims to enumerate stolen data in as hands-off a manner as possible, only asking the user for
<ul>
    <li> A user-owned Channel/Group ID (if any) <li>
    <li> The minimum of the ID range </li>
    <li> The maximum of the ID range </li>
</ul>

<pre>
    def auto(self):
        import time
        from datetime import datetime

        exists = self.create_folder()

        if not exists:
            self.query_endpoint(self.gen_endpoints)
            self.query_endpoint(self.channel_endpoints)
            
        BLUE_CHANNEL = ""

        BLUE_CHANNEL = input("ENTER YOUR GROUP ID: ")
        MIN = input("ENTER MINIMUM CHAT ID: ")
        MAX = input("ENTER MAXIMUM CHAT ID: ")
        with open("enumeration.log","a") as a:
            a.write(f"Began enumerating chat IDs between {MIN} - {MAX} at {datetime.now().strftime('%Y-%m-%d %H:%M:%S')}\n")

        if BLUE_CHANNEL == "":
            cntr = self.cycle_through_history("RED",int(MIN),int(MAX),BLUE_CHANNEL)
            print(f"COMPLETED DATA ENUMERATION. RETRIEVED {cntr} MESSAGES.")
        else:
            cntr = self.cycle_through_history("BLUE",int(MIN),int(MAX),BLUE_CHANNEL)
            print(f"COMPLETED DATA ENUMERATION. RETRIEVED {cntr} MESSAGES.")

            exit_channel = {
                "leaveChat":f"leaveChat?chat_id={BLUE_CHANNEL}",
                "close":f"close",
                "logOut":f"logOut"
            }

            self.query_endpoint(exit_channel)
</pre>

This mode of Molemaker automates the creation of the folder in which data will be stored, the enumeration of the data itself (given a specific range) and the process of closing your connection to the bot. If no Channel/Group ID is supplied by the user, Molemaker will use an alternative method of data enumeration to the classic forwarding to a user-controlled chat.

<h2> Isolated Informants </h2>

In certain instances, a bot is configured by the threat actor in such a way that it is unable to join Channels/Groups other than those managed by the malicious actor. For these cases, Molemaker will switch to the following flow of data enumeration:
<ul>
    <li> The bot will forward a discovered message to its own chat </li>
    <li> The action will generate a unique message_id for the new, forwarded message </li>
    <li> The bot records the forwarded message to a .json file (as with the usual flow) </li>
    <li> The bot deletes the newly created message according to its assigned message_id </li>

For each message found, the saved JSON response will allow you to see what information has been stolen by the attackers. On the threat actor's end, there is no noticeable change post-enumeration -- the messages appear as they had before; however, during the enumeration process it is possible that an attacker may see the momentary appearance and disappearance of the forwarded messages. This is a risk we are looking into mitigating.

This method is also suitable for defenders who do not have a Telegram account, or do not wish to create one.

<script>
$("pre").html(function (index, html) {
    return html.replace(/^(.*)$/mg, "<span 
class=\"line\">$1</span>")
});
</script>