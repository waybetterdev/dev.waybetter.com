---
title: How did you node that?
tags: [hubot, npm module, node, teresa-nededog]
categories: [technology]    
name: [Teresa Nededog]
author: [teresa-nededog]
snippet: [I've been given the privilege of taking charge of our company's software robot, known as Hubot. Since expressing to my superiors that I aim to be a DevOps engineer when I grow up, this has been the perfect opportunity to dive into script writing!]
---

I've been given the privilege of taking charge of our company's software robot, known as [Hubot](https://hubot.github.com/). Since expressing to my superiors that I wanted to dive into DevOps engineering, using Hubot made this the perfect opportunity to venture deeper into script writing! Not only was I tasked with customizing features relative to our company's chatroom needs, I was assigned to create my first node module!  

I had absolutely NO idea how what a "hubot" was. Luckily, there are libraries of documentation. I first had to get hubot integrated and running 24/7 within our WayBetter Slack channel. After hours of studying documentation and video tutorials from hubot experts, I was comfortable enough to touch the code within our own hubot. 

After installation, a custom name is expected to be declared for your hubot. Our company decided to affectionately name our hubot "Richard Simmons." 
<br>
<img class="img-med" src="/images/posts/richard_simmons.gif">
<br>

Initial installation was a bit of a challenge. It's one thing to clone a project but the README wasn't very clear and to get Hubot up and running took me more time than I would've liked. However, I'm a firm believer that anyone can become a dev if they aren't afraid to dive in, make mistakes and break things. I spent days suffering in silence, writing brittle code and decifering confusing error messages until I found a doc written in Japanese that provided the [solutions](http://qiita.com/tbpgr/items/eb876eb2847ef55e218e) I needed to get Richard Simmons running in our chatroom 24/7. 

Celebrating that victory was short lived by the flurry of assigned features. Features included New Relic, Jira and Pager Duty Integration, Karma points and User's Away Status. I was able to integrate Jira, Karma points successfully.  The User's Away Status feature became the greater challenge. So much of a challenge that it took a week to figure out. Big boss thought this would be best implemented as a node module.  I never thought I'd have so much stressful fun.

Creating a node module seemed something only experienced developers were capable of.  Come to find out, it wasn't as hard as I thought. NPM offers a step-by-step tutorial on how to [register your own module.](https://docs.npmjs.com/getting-started/creating-node-modules) The breakdown is as follows: 

Create the package.json:

    $ npm init

In this case with hubot, we use default index.coffee instead of index.js. Add the author field: 

    $ Your Name <email@example.com> (http://example.com)

Hit enter for each prompt. You can change these default settings later.

Setup a new user account on the npm registry: 

    $ npm adduser

If an account already exists, store the credentials: 

    $ npm login 

Publish your new package:

    $ npm publish

Double check if your package has been successfully published:

    http://npmjs.com/package/your-package-name

The npm module I published on behalf of [WayBetter](http://waybetter.com/) is an extension of [hubot-rememberto](https://github.com/wdalmut/hubot-rememberto) called [hubot-whereyouat](https://www.npmjs.com/package/hubot-whereyouat). Still undergoing major changes but feel free to pull it into your own working Hubot:

    $ npm install hubot-whereyouat

Then add:

    "hubot-whereyouat"

To your external-scripts.json.

Whether you're working locally or integrating it into your company's chatroom, the commands not case-sensitive and are as follows:

Set away message and timestamp. Pings user after time is up:

    hubot (away message) for (numeric value)(s|m|h|d) 

Post user's away message:

    hubot where is (user)

Clear away message:

    hubot back




