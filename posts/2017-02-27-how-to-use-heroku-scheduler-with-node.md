---
layout: post
title:  "How to use Heroku Scheduler with Node.js"
tags:
- code
- how-to
---

While there are [multiple](http://stackoverflow.com/a/13956024) [tutorials](http://www.spacjer.com/blog/2014/02/10/defining-node-dot-js-task-for-heroku-scheduler/) out there on using the Heroku Scheduling add-on with Node.js, they all seem to suggest the same strange series of steps.<!--more--> I can't understand why they all recommend
- putting your scheduled task in a file without an extension...
- in a directory called 'bin', and...
- executing it without specifying `node` in the command, but instead adding `#!/path/to/node` to the top of the task file.

I originally didn't look at any of these tutorials - I guessed from the examples using other languages in the [Heroku docs]() that I could just write a normal `javascriptfile.js` and execute it by running `heroku run node javascriptfile.js`. 

However, Heroku also asserts that you should be able to test your task by running `heroku run my task` locally, but every time I tried this, it would fail with the following error:
```bash

```
Wait, hold on just one cott'n pickin' secon'. I just went to run it so I could dutifully copy the _exact_ error message for y'all, and it ran WITHOUT GLITCHES. wAT?!?? 

Well. My only proposed explanation is that all the times I ran this in the past, I either  

1) hadn't added, committed, and pushed the file to Heroku, or  
2) hadn't waited long enough after adding, committing, and pushing to Heroku for it to be available.  

Idk if that last one makes any sense. But I swear it has to be something like that, because after reading a tutorial that (said I should be able to do it the way I [was doing it](http://www.modeo.co/blog/2015/1/8/heroku-scheduler-with-nodejs-tutorial), plus) specified adding and committing and pushing the file before trying to `heroku run` it, I definitely made sure I had done that.  

Anyway, where I _was_ going with this was to say that it kept claiming it couldn't find the `javascriptfile.js` that I was telling it to run. Because my task wouldn't necessarily output anything every time it ran, I simply added an extra line  so it would email me every time the file was executed. I also added a similar line to the sibling file that I had copied into the /bin dir and removed the .js from and added the path/to/node to, plus a comment telling me which was which.

My Heroku scheduled tasks now looked like this:  

!['node task.js' and 'task'](/img/heroku-scheduler.png)

I gratefully soon received an email from my task! It was from the file I had originally included - the one that made structural sense to me. Phew. Maybe I do understand things after all.

Now I wait to see if the other file will also send me an email! I hope the suspense doesn't kill me. If it doesn't, I'll just delete it. If it does, I'll delete it anyway.* Teehee - it's all for __science__!


\* I'll delete it anyway because I don't need two files for one task, and I wanna keep the one I came up with all on my own! ![heart eyes face](/img/heart-emoji.png)

