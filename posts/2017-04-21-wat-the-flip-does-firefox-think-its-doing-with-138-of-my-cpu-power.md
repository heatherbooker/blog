---
layout: post
title: "Wat the FLIP does Firefox think it's doing with 138% of my CPU power?!"
tags:
- code
- plz-help
---

Oh my bananas, I am having a computer crisis. It all started when I was nOT tOUCHING mY cOMPUTER at all and the only application running (to my knowledge) was Firefox, which had like 4 tabs open. "So what?" you think. "Who cares?" you ask. Well, my fan was going as hard as it could for several iterations of several minutes. I was starting to worry for my baby's health.<!--more--> Also, lately my battery has been dying way too fast for a laptop in its first year of life. So I was concerned.

Let me walk you through my process over the last hour and a half.

I notice the fan going and going and GOING AND GOING AND GOING AND --

I vaguely remember some fabulous new profiling tool I discovered in the last couple days that showed me CPU usage. What was it? Something short...hblot...`top`!

I forget why I first wanted to check `top` out. Oh yeah, I was indexing a bunch of stuff in Elasticsearch and my computer was srsly struggling. It was using around 110 - 135% CPU which at first was utterly confusing, because you can't have more than a hundred per cent of a thing, cuz _per cent_ means _OUT OF A HUNDRED_ and THAT'S NOT HOW THAT WORKS. (I've been reading this blog lately with a lot of YELLING...I'm sorry :,D)

Anyway, turns out CPU usage is expressed (at least by `top`, I don't know whether other tools follow the same principle) in usage of one processor. 

Right, so Elasticsearch was doing all this (presumably) computationally expensive stuff, dealing with all the data I was giving it with 0 mappings or structure (types? what's a type?), using >100% CPU. The fan was going crazy then too, so the parallels between these situations are not looking good.

I `ctrl-alt-T` and `top` and `enter` and FIREFOX WAS USING > 100% CPU! WAT THA FROCK! I close it, kill the process (why is that necessary? why didn't closing it like a normal person kill it? so many questions.), reopen it, and great, now 20-30% at rest and 60-70% while using. 

I mean, small blessings? But to me, that still seems like an awful lot when I want to have Firefox and Sublime and Opera and Vim and Rhythmbox[^1] and a server or two running alongside it. There's not gonna be enough %s to go around, and somebody is going to be left in the dust.

I do some googling, which suggests:

- My browser is outdated (no.)
- I should enable hardware acceleration (no.)
- My addons/plugins are being greedy - okay, maybe!

I disable [Lightbeam](https://www.mozilla.org/en-US/lightbeam/) as my first action, which brings the CPU usage from 60% to 30% while using and 15-20% if I'm not touching it, although all I have to do to cause a 5-10% spike is mouse over it, so that's weird. Maybe it's a coincidence. Sidenote, Lightbeam is a Firefox browser extension made by Mozilla which tracks websites tracking you basically, and all the requests that happen in your browser. I was originally checking it out as a potential Outreachy project. But if it turns out it takes 30% of my CPU, take it away!

Woohoo! Ok, so now we're making progress. Next I disable session manager which saves all my tabs and reopens them next time I open the browser, which Opera automatically does for me and I adore because I am 100% a tab hoarder. Then I try some browsing while keeping an eye on `top` and NOPE, back at 50-70%. Shucks, turns out Lightbeam wasn't actually doing any insane-o CPU hogging after all.

So I just straight up switch to Safe Mode, which disables all extensions, to see if any other addons or maybe the whole lot of them combined are being sneaky little buggers. I go to Youtube and before I can even click on anything my CPU usage is back at 138%!!!! I wish I could write numbers in capitals. ONE HUNDRED AND THIRTY EIGHT PERCENT!! (No wait, my teacher always told me, if you say "and", it means hundredths. ONE HUNDRED THIRTY EIGHT PERCENT!!! That's better.) 

Anyway I google more stuff and checkout Chrome, which has MULTIPLE PROCESSES RUNNING Chrome wtf u think ur doing?! Pff. This makes it really hard to compare with Firefox's one number because I need to add like 4 or 5 numbers and that is toO many numbers friends. TOo many. Also I'd like to point out that I am semi-gleeful at discovering this, because I've heard some negative things about Chrome eating way more resources than is reasonable and I think maybe I have cracked the secret. Next I try my trusty friend Opera which also has multiple processes! Opera! How could you betray me like this!?? I'll just say each process of Opera and Chrome is generally around 20%. Sometimes they'll go up to 45% or 70% or more.

Now I am googling different stuff: "Why does Chrome use multiple processes". I think this will explain all of Chrome's bad behaviour. Ever. What I find is [this comic](https://www.google.com/googlebooks/chrome/), elaborating some interesting thing on the design of Google Chrome (sorry, I just realized I've been saying "Chrome" as if Google Chrome owns that term. How did they get away with using the universal name for a major part of the browser for _their_ browser? Hmph. Making it confusing out here.), including:

- Each process is a tab!
- This means that if one webpage has something silly like an infinite loop, only that tab needs to be killed as opposed to the whole browser.
- This system is based on how operating systems open threads for each process.
- This also ensures greater security, as each tab is isolated. (I am definitely simplifying here.)

This comic is 39 pages long and I am on page 29. I'll get there eventually. Maybe. I do recommend checking it out though if you're interested.

Bonus - remember how I said I don't need to enable hardware acceleration? I decided this because in my settings, the checkbox for "Enable hardware acceleration" is ticked. Seems pretty clear right? Nah, jk. Checking some more advanced settings page tucked away somewhere (as these things always are) exposed that actually, apparently my OS was blocking this from happening. From what I understand, I don't want to force enable this because bad things could happen, ie, something about webby acceleration scripts exploiting OS vulnerabilities, or something. So I'm just gonna not touch that.

To conclude, I learned some stuff about Google Chrome's design, and I solved nothing so my computer is still a minion[^2] to the browsers siphoning as much power as their grubby paws can. Do let me know if you think you are more smarticle than I in this matter and wish to assist. :)

UPDATE OH MY DOG Opera has calmed down while I've been writing and each process is now using under 10% it's a miracle??!!?


[^1]: "Yo, why the heck you need two browsers and two text editors?!" Whatever, don't judge me! Sometimes one sucks and you gotta switch over but then that one sucks too so...well...NYEH. *sticks tongue out*

[^2]: This line used to read "...my computer is still a slave". I am trying to remove uses of common tech terms that perpetuate white supremacy from 1) my vocabulary and 2) my bubble, starting with blacklist/whitelist and master/slave.
