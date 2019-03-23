---
layout: post
title:      "I am a robot"
date:       2019-03-23 01:33:58 -0400
permalink:  i_am_a_robot
---


I was very excited for my cli project, which would be scraping zillow.com for properties available for rent. Since I was in the process of searching for a property myself, I anticipated spending a lot of time on there anyway. I struggled a bit getting the url to work how I liked, so coped a page into a fixtures folder just for testing. All was going well and smoothly, I had a scraper, I could find the data, I needed, just the cli left to do and to use a live page. Switching out of the fixtures folder though, everything stopped working. My scraper was returning nothing. So instead of asking it to pull specific things out of each page it scraped, I asked it to pull eveything. I anticipated about 25 webpages worth of code, but maybe it will tell me something. What I got instead was 25 copies of:

```Please verify you're a human to contine.```


I was caught by a captcha. I vaguely knew that captchas served a purpose somewhere in the world, and unknowingly, I have become that purpose. In talking with my project advisor, we looked at something I've never heard of, the robots.txt file. Every webpage out there has one, you just add "/robots.txt" to the end of the url's main page.

```zillow.com/robots.txt

google.com/robots.txt

facebook.com/robots.txt```

Right there in that file, under user-agent: * (everyone), homedetail was disallowed, as well as pretty much everything, it seems. So, no dice for a robot finding a house for me this time. Redfin, Trulia, Rent.com, all the same. I suppose not having people's addresses, house values, blueprints, contact info, and so much more available for the inevitable CLI uprisings makes sense. But now we know! Robots are everywhere.
