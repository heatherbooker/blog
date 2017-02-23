---
layout: post
title:  "Day Whatever @ RC"
tags: RC code thoughts
---

## What am I doing?
I'm waiting for git to finish cloning Mozilla's Bedrock repo.
### Why?
So I can set up a dev environment to contribute to Mozilla.
### Why?
So I can get my first bug fix in and give back to FOSS (Free Open Source Software)!!
### Why?
So I can work with Mozilla in the Outreachy project! <3

## Thoughts
Following along with their docs on setting up the dev environment is amazing! But of course I have just one suggestion...they say
> If you are on Linux, you will need at least the following packages or their equivalent for your distro:
>   python-dev libxslt-dev
Now, in particular because one of them starts with 'python', I tried to `pip install` both of them. Eventually I realized they needed to be `apt-get install`ed instead. Whoops.
But maybe people who can't realize that by themselves will struggle even more to actually contribute to the code base...so maybe it's an early test. I passed!

GAH
The tests for some reason don't all pass! So I ran them one by one until I found a culprit - bedrock/newsletter. I don't know what that means or if it'll be helpful, but I feel like I made progress so that's cool.
