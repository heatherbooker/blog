---
layout: post
title: Learning SSH etc
tags: code
---

I am job searching! Fun times. I also have a weirdly high proportion of friends simultaneously job searching, which is cool because we can discuss interview stuff. This is exactly what led me down the road labelled "learn SSH" - my friend and co-job-searcher behind [Hey Heather, it's me again](https://www.heyheatheritsmeagain.com/) mentioned that an interviewer asked "If you needed to get some files onto a remote server, how would you do that?", and my response was externally a cool, blank stare, and internally a spiral-of-doom-panic. :sweat_smile:

<!--more-->

But my friend knew! They answered "scp", and then the interviewer added "sure! or rsync.". And then I asked this question to a different friend, who answered "ssh". So now I had 3 things to learn about! I had heard of all of these, and I know my team at my previous job used `rsync` for our deployments.

So, I got together with my friend who went through the interview, and we started slowly chewing through the many topics we had on the menu. The main objective was: "Use each other's computer as a remote server, and try to get some files onto it!"

We started by listing what we already knew (or _thought_ we knew):
- FTP: An antiquated way to transfer files (this "knowledge" came from me using it several years ago to make a website for someone.).
- SSH: How to use another computer's command line remotely.
- SCP: SSH, but just for copying files.
- rsync: Another way to copy files???

Unfortunately, (or perhaps fortunately? the more learning, the better?), the explanations of what these things are always references other unknowns, and so on and so forth until you have unraveled a giant tangle of things-you-don't-know, and suddenly it's not looking like a one-day project anymore. It reminds me of this type of dictionary definition which I cursed just recently:

>__heretical__:
Of or relating to heresy or heretics.
  Characterized by, revealing, or approaching departure from established beliefs or standards.
  Containing heresy; of the nature of, or characterized by, heresy.

Two thirds of those definitions just mean you need to learn MOAR DEFINITIONS. (As defined when [searched via DuckDuckGo](https://duckduckgo.com/?t=ffab&q=heretical&ia=definition).)

And so it is that we learned many things that day. Mostly that everything internet-protocol-y (IP-y!)[^IP] is badly named:

- SCP's original implementation was flawed, so now, under the hood, calling `scp` just gives you a secure version of FTP: SFTP, which is FTP over SSH.[^FTPETC]

- TLS/SSL has both in the name because SSL was essentially under copyright, but SSL was the well-known name for the protocol, so TLS was just a fancy new name given to the new version of SSL. Except people kept jamming the names together because nobody knew what TLS was, so it needed SSL to hold its hand while it got famous too, and now the pair of names has stuck. [Read about the meeting that decided this](https://tim.dierks.org/2014/05/security-standards-and-name-changes-in.html) (and the actual details, because "copyright" is the wrong word).

I also ended up reading some of Julia Evan's zines, specifically the [HTTP](https://wizardzines.com/zines/http/) and [DNS](https://wizardzines.com/zines/dns/) ones. Reading the HTTP zine made me think: I really oughtta memorize HTTP response codes. Because while I definitely laugh a socially appropriate amount at any HTTP status code jokes, I don't actually remember which one is which, and that's a bit strange for a web developer. No problem though, because [HTTP dogs](https://httpstatusdogs.com/)[^dogs] are here to help me (and you!) learn while LOOKING AT CUTE DOGGIES :heart_eyes:.

Here they are, grouped by the 100:

- 200: OK
- 300: Redirect
- 400: _You_, the user, did a booboo.
- 500: _They_, the server, did a booboo.

And for a bonus level, we can get into specific codes:

- 403: "permission denied"
- 404: a crowd favourite—"not found"

While looking at this graphic of the parts of a URL (from https://wizardzines.com/comics/how-urls-work/):

!["parts of a url" from wizard zines](/img/url-wizard-zine.png)

, I discovered the "port" is a part of the URL that we don't usually see when browsing the web, but you can sneak around and try putting it in the URL to see what happens -`:80` for `http://` sites and `:443` for `https://` sites. I found that some sites (stackoverflow over https) loaded just the same, but others (I forget what, over http) did not appreciate having the port specified. XD Or perhaps it was that we specified `:80`, the port designated for HTTP, while having typed `https://` at the start of the URL? I'll admit I forget. Please do try it out and write me with your results.

Once we figured out how to get each other's IP addresses -

Oops, time for a sub-lesson!

I used `ip address`, which spits out a lot of output, and the _first IP that isn't localhost (127.0.0.1)_ you see is the useful one. (I don't know what the others are. MOAR THINGS TO LEARN! :))

It seems that I get a new IP depending on which router I am connected to, which makes sense, because I only exist on the internet when I am connected to it, and I have to connect through the router, and how else would other internet devices find me, if not by starting at my router? I just found it cool to see how the IP of each device connected to that router shared almost the entire IP address, and only one portion was incremented, ie (these are all fake IPs):

172.666.1.9 - my friend's computer!
172.666.2.9 - random other wifi-connected device, say, a smart thermostat
172.666.4.9 - my computer!

I admittedly couldn't remember whether the second-last digit should be incremented, or the very last digit; while trying to find the answer, I unearthed one hundred billion more questions, like do I have a public _and_ a private IP?! (No, I have a private IP that my router knows, and my router has a public IP that the rest of the internet knows.), and how do [Signal](https://www.signal.org/) messages get to me if I don't have a public IP?! (The internet can never initiate sending things to me; I must always ask for it. Messages can get to me by the app leaving a TCP connection open, or by sending requests for updates frequently, by example.) There's also a thing called NAT traversal, or Network Address Translation traversal, which is a way of cutting out any middle parties, to decrease the overall bandwidth used. NAT traversal is often used for video calls among other things. Sorry for mentioning it and basically teaching you nothing specific about it; have fun looking it up yourself! ;D

It also seems that the router reserves some number for devices which are intermittently connected. I say this because we used `nmap` to see which devices were on the network, and it showed that no devices had the number 3, but that the router gave me an IP address with the digit 4 when I connected to the network. `nmap` is a magical and spooky tool that can tell you which other devices are connected to the same network. It can tell you their IP, their MAC address; it can guess which OS they are running, and it can tell you the device's name. This is just one terrifying part of what we will later find out amounts to a significant argument for never working in a café again.[^nmap] To return to the IP addressing: we couldn't identify my friend's phone as any of the devices that nmap found on the network; it turned out that the phone disconnected from the wifi while it wasn't in use, and once a browser tab was opened, the phone now showed on the nmap output, with the skipped IP address! I'd like to know how it manages this; surely it can't keep addresses reserved for offline devices forever, or else I would have gotten a much higher number than 4 (it was actually 6, I thought I could get away with simplifying things!).

Now that I've typed "router" 20 times, I'm feeling self-conscious about the difference between a router and a modem. Should I have been saying "modem" this whole time?! I can't look it up because firefox keeps crashing today. Too bad! :joy:

So, as I was saying, once we figured out how to get each other's IP addresses, I started a tiny server on my computer using the npm module http-server, in a directory with just a dog.txt file containing this adorable ascii dog ([source](https://ascii.co.uk/art/dog)):

```txt
         ,--._______,-. 
       ,','  ,    .  ,_`-. 
      / /  ,' , _` ``. |  )       `-.. 
     (,';'""`/ '"`-._ ` \/ ______    \\ 
       : ,o.-`- ,o.  )\` -'      `---.)) 
       : , d8b ^-.   '|   `.      `    `. 
       |/ __:_     `. |  ,  `       `    \ 
       | ( ,-.`-.    ;'  ;   `       :    ; 
       | |  ,   `.      /     ;      :    \ 
       ;-'`:::._,`.__),'             :     ; 
      / ,  `-   `--                  ;     | 
     /  \                   `       ,      | 
    (    `     :              :    ,\      | 
     \   `.    :     :        :  ,'  \    : 
      \    `|-- `     \ ,'    ,-'     :-.-'; 
      :     |`--.______;     |        :    : 
       :    /           |    |         |   \ 
       |    ;           ;    ;        /     ; 
     _/--' |   -hrr-   :`-- /         \_:_:_| 
   ,',','  |           |___ \ 
   `^._,--'           / , , .) 
                      `-._,-' 
```

And my friend typed my IP address into the URL bar on their computer, followed by `:8080`, and saw a link to that dog.txt, and clicked it, and saw the dog! And voilà, just like that, we have succeeded at transferring files between computers - an excellent first step to our interview mission. We then followed up with me running my development server for my blog which is configured to port `:8080`, and my friend could see that too!

Now this suddenly seemed very terrifying, because when you do an nmap scan, it also gives you a list of ports in use on each device, meaning that a stranger in a café could see that a computer with X IP was serving something on Y port, and then they could access it just like my friend just did - without my realizing, without my meaning to.

Naturally, we tested this out in the other direction too: with me trying to type 172.666.my-friend-IP:friend-port in my browser's URL bar, but I couldn't see anything! I forget which kind of "access denied" page it was; I think it just says something like "unable to connect". Unfair!! Why can't I spy on a website about me?! :joy:

More investigation reveals that my friend's blog uses Jekyll, my blog uses 11ty, and the dog art server I just made uses http-server. They have different default ports too - at first I read something that indicated `:8080` is for public use, and since my blog was using that port, I suspected this was the explanation. However, using all sorts of different ports on the dog art server did not prevent my beautiful dog art from being accessed right out from under my watchful eyes. Yet more investigation reveals that setting the default host address to `0.0.0.0` makes a server accessible from other IPs, while `127.0.0.1` keeps it private - for `localhost` only. The help page for http-server indicates how to change this, but I have yet to figure it out for Eleventy which is the framework my blog uses. It's wild to me that something that seems so significant could be so...undocumented? Un-obvious? I mean, admittedly http-server lists it as an option in the help dialogue, but it's not in the readme and I had a hard time searching for it. I admit I have a fear of man-pages and help-dialogues...they just have...

SO MANY WORDS.
SO LITTLE TIME.
MUST CONTROL-FIND.
:joy:

Side note, now that I'm reading about these IPs, I'm just garnering more and more questions, like how can a host have multiple IPs? That's gonna have to be more questions for another day.

So that was a significant aside, but we eventually got back to our main squeeze, putting a file on a server. (Servers don't tend to have web browsers or care about ascii art, but maybe they should...?)

`rsync` is purportedly also a handy tool for getting files onto a server - my friend read the wikipedia intro paragraph and proclaimed "It's like git, but for transferring files". This comparison is because they both use delta encoding[^delta]. Delta encoding means that instead of copying the whole file every time, it checks which parts changed, and sends (in the case of rsync) or saves (in the case of git) only those parts. In a massive app like the one I used to work on, rsync could mean significant bandwidth savings. But if I have the updated files on my computer, and the server has an old set of files, how does rsync know what the diff is, without first sending one of the versions to the other? I suppose it could use a sort of hash? This is conjecture, I need to do a part 2.

Moving on for now to `ssh` and `scp`. It seems to me that `ssh` only helps insofar as you can use a `scp` command from the server, rather than using the same `scp` command but with arguments (file_from, file_to) reversed but from the computer that you're sshing from. So:

```
computer has dog.txt
server wants dog.txt

start in computer shell:

computer$ ssh into_server
server$   scp remote_file to_local

or

start in computer shell:

computer$ scp local_file to_remote
```
Again, total conjecture. I used `ssh` a few times at my old job but was always terrified of accidentally deleting the entire database[^database]. All tutorials on the internet indicated that `scp`ing onto a server should be as easy as
```
scp dog.txt 192.666.their.ip:/home/directory
```
Alas, a solitary computer is not a server, or at the very least it's not a server that wants to be `ssh`ed into or `scp`ed onto, because I was rudely "disconnected" several times. It seems that you must set things up, share keys perhaps, and all that is going to need to be a lesson for another time, because we were hungry or maybe hangry and that was the end of our lesson.

But progress has been made. Many things were learned, explored, examined, and insulted that day, yet our eagerness to answer interview questions more deeply than was ever expected remains strong. Thanks for sticking along for this wild ride of computer learnings, and I hope you're out there having your own fun and asking your own turtles-all-the-way-down questions.



[^IP]: did you know that IP just stands for _internet protocol_?! Well, I didn't. :joy: I've been watching the [Crash Course Computer Science](https://www.youtube.com/watch?v=tpIctyqH29Q&list=PL8dPuuaLjXtNlUrzyH5r6jN9ulIgZBpdo) videos, and they are just amazing. They explain stuff so simply and are just a joy to watch. That's where I learned what IP and MAC address mean.

[^FTPETC]: SFTP or FTP over SSH is not to be confused with FTPS - FTP over SSL, or the other SFTP - Simple FTP; see [Wikipedia FTP derivatives](https://en.wikipedia.org/wiki/File_Transfer_Protocol#Derivatives)).

[^dogs]: Oh, the website says HTTP cats came first. Well, my friend knew about the dogs one, and I was too busy admiring those lil cuties to read the fine print. :joy:

[^nmap]: I am certain that I have seen nmap used before, and that in fact someone probably demonstrated exactly how nosy it can be, when I was at the Recurse Center several years ago. Alas, the message didn't stick, and I continue to be obtusely unconvinced to trade convenience for security.

[^database]: Which definitely never happened and wasn't a story that got passed around to spread fear into the hearts of innocent developers like me. Nope.

[^delta]: Reading more about delta encoding led me to the idea of [using it for websites](https://en.wikipedia.org/wiki/Delta_encoding#Delta_encoding_in_HTTP), which is quite cool. I'm thinking it would be a cool project to build a...what could you build to simulate this? A website? A package that could be used on any website? Probably not a browser extension.
