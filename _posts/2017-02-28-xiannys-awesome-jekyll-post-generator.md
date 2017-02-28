---
layout: post
title: "Xianny's awesome Jekyll post generator"
categories: code 
tags: code shell
---

Oops, note to self - need to strip special characters such as apostrophes from file names. (Guess how I came up with that one..[1]).

Having got all this Jekyll stuff finally up and running, I was dying from manually formatting all of my posts and their titles. Seriously, typing out the whole date for every post's file name?! Terrible. Also, srsly, what kind of programmer am I if I keep doing something manually. What is even the point.

Unfortunately, shell scripting is _not_ my forte.<!--more--> It is so confusing. I remember one day at [RC](https://recurse.com), AJ (teehee, two two-letter acronyms, I am Heather-two-two over here) showed me a couple quick things about it and I was amazed. Then I read [this blog](https://strugee.net/blog/2017/02/rc-week-7) post where he talks about 

1) how he and Aditya solved a Code Dojo with a bash script (!!!), and how  
2) Aditya asked about type-casting in bash.  

AJ thought that was funny, because bash doesn't have a type system. What?! That explains this one problem I couldn't solve a while back, and why the internet wouldn't tell me how to coerce a string to a number in my shell script... -_-

Anyway, you can see this snippet from my (appallingly large collection of) desktop notes that _desperately_ desires this Jekyll-generating script in my life:  

![it has a lot of exclamation points. because i wanted it.]({{ site.url }}/assets/img/post-generator-wanted.png)

And then, in checking out the Blaggregator feed (that I finally added myself to! \*cue emoji with monkey covering its eyes\*), I came upon Xianny's blog post where she wrote the PERFECT SCRIPT! You should definitely [read it](http://journal.xianny.com/2017/02/21/autogen-jekyll-new-post.html)! Go. Right now.

This was beyond perfect. I am so happy right now. 

I made a couple of [tweaks](https://gist.github.com/heatherbooker/50672cef429e667270b39c0d19f44fe3), namely:

- if `-d` is passed as a command line option, add `draft: true` to the front matter[2] of the post instead of adding it to a _drafts dir.
- make post titles all lowercase in filenames.  

This second undertaking involved a very tiny addition, surprisingly.

```
# Before:
# (The - before the $ is not fancy bash shtuff...it's just a dash.)
TITLE+="-$word" 
 
# After:
TITLE+="-${word,,}"
```

Not bad hunh?! And now all I did to write this post was type `./new "Xianny's awesome Jekyll post generator"`! 

(Also I had to write the rest of the post and screenshot my note, but...ya know, that was CHILD'S PLAY.)

Thank you Xianny!!


PS - If I forget to fill in the `categories` field in the front matter, this happens:   
![nonsense categories on the post]({{ site.url }}/assets/img/fill-me-in.png)  
because I set it to do that, so I won't forget. It's sorta functional to that end I guess? I am snort-laughing because I tried to help myself but it turns out I am un-helpable.

PPS - If you don't use Jekyll, you may not know why I want to strip apostrophes, or what front matter is.

[1] I want to strip non-alphanumeric chars from titles when they become filenames. I have a feeling they don't belong there.  
[2] Front matter is this configuration crap that goes at the top of every post. Stuff like the layout, title, categories, etc.

PPPS - Help, I think I am addicted to writing blog posts.
