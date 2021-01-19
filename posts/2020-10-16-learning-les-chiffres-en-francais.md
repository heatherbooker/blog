---
layout: post
title: "Learning les chiffres en français"
tags:
- code
- french
---

>Oh, Miss hboo:
>
>Many years I have waited  
For a gift like yours to appear  
Why, I predict this project  
Could make you a first rate! francophone  
My dear, my dear!  
I'll write at once to blog readers  
Tell them of it in advance  
With a tool like this, dear there is  
A definish chance  
If you work as you should  
You'll be making ...goooooooooood :heart_eyes:
>
>Did that really just happen?  
Have I actually understood?  
This problem I've tried  
To ignore or hide  
Is a talent! that can be learned...to  
Help me use French numbers!  
If I work good.  
So I'll work good...

:joy:

I definitely ripped that off big time from Wicked the musical. And my changes aren't particularly clever, which is fine, because the goal here is:

LEARN YOU SOME NUMBERS IN FRENCH FOR GREAT GOOD.

And not: "Be good at writing song lyrics." We can work on that another day maybe. :joy:

<!--more-->

Anyway, so learning numbers in French is impossibly difficult, at least if you ask me. Which reading my blog is basically asking me, I guess. ¯\\_(ツ)_/¯ It's especially difficult because 50 is 50, which is cool, and 60 is 60, which is also cool, but then suddenly 70 is 60-10, and then 80 is: well, what could it be? 60-20? 60-10-10? No, better: 4-20. Ah! Beautiful. Now 90 can follow the 70 pattern, and add 10: 4-20-10. And god forbid you want, for example, 97: 4-20-10-7. -_\_- Unbelieveable. I know English is impossible too but come on, numbers!! They're so hard!! And so important!! Who cares if you say "I goed" instead of "I went". But we will care if you give us $4.20 when we actually wanted $80, thank you very much!!

Puh. Rant over. So what I really wanted for practicing numbers was just something that would shoot numbers at me again and again, and tell me if I understand them correctly. I found [a cute lil video series](https://www.youtube.com/watch?v=3rd_haB5V0c) which was fine for my purposes, except it's only like 20 numbers, and I need to practice like infinity numbers. Eventually I gave up wanting and wishing, and made something myself!! Which I began working on a week or so ago, and then suddenly received a kick in the butt to finish, in the form of a voicemail message that I had to listen to 15 times to get the phone number I needed to call back. I was getting sweaty just sitting there.

So [here's what we've got](https://github.com/heatherbooker/pratiquer-chiffres)! A cute little python script that uses the [Google text-to-speech API](https://cloud.google.com/text-to-speech) to say numbers aloud in French. The normal (online) mode picks a random number, sends it to the Google API, writes the response to disk, plays the newly-saved file, asks you what number it was, and then JUDGES YOU. The current judgement format of "right vs wrong" is great for me, because I'm slow but usually correct, as long as the number:

- doesn't start with 8000, and
- isn't longer than 4 digits

Don't ask, idk, 8000 always confuses me, I always write 1000 instead.

It saves the audio to disk because it seems wasteful to ask Google over and over for 44 (or 709, or 0, or..). That also opened up the ability to add a second mode: offline mode! For when my Google free cloud limit is reached, or the internet is being a potato, or whatever. So for offline mode, it just picks a random audio file from the numbers that have been done previously, and hits me with that. Some other little perks of this offline method include being slightly faster, and also not making my fan spin for 0.5 seconds at a time.

Other interesting things I learned or decided while creating this lil creature:

- the french equivalent of "uh oh" is "houlà"
- python has the best interface for getting simple user input
- i miss javascripts sneaky conversion between strings and integers, i got bit by expecting that a few times, plus:
- i miss typescript telling me that i'm stupidly trying to put a string where an int should be, and vice versa
- i used `subprocess.Popen` instead of `subprocess.run` for playing the audio, so that for big numbers, i can start typing the answer before it has been fully said :joy:
- you have to specify you are writing bytes to a file (as opposed to writing a string)

On the sad side of things, I did not succeed at taking a screen recording of me playing, so I don't get to show you that. :disappointed:

On the to-do side of things, I really would like to be able to give the program an answer and then tell it to quit instead of it starting another number. I'm thinking something like

```shell
what numba?
> 42 done
correct and goodbye!
```

Because right now it's like

```shell
what numba?
> 42
what numba?
> done
correct and goodbye!
```

And this stresses me out because I feel like I'm leaving it halfway unfinished[^1].

Anyway, [here's the code](https://github.com/heatherbooker/pratiquer-chiffres), have fun, submit issues/improvements, troll, learn, etc. :) À la prochaine !

-----

PS. I installed a plugin to convert `:+1:` emoji shorthands into actual visible emojis, which is rad. However, I predict it will make the previous post where I explain that I am using shorthands very confusing, since now they will just be actual emojis. You win some you lose some. :joy:

-----

[^1]: "Halfway unfinished" is a pretty weird thing to say, but I'm kindof into it.
