---
layout: post
title: How to control spotify from the command line
tags: code how-to shell
---
Holy buhjeebus the amount of steps. I just want to listen to business casual. Ya know... Let's keep it business. Let's keep it casual. Let's keep it...fun.

So! Here comes, how to listen to spotify from the terminal in fedora. YMMV (Your mileage may vary).<!--more-->

Copy this handy AF simple AF (you're gonna have to look that acronym up yourself...unless you're my mom, then don't worry about it mom) script from this amazing life saving person, that I only found after downloading 298374 other super complicated things that didn't do what I wanted, at all. Like mobify or mobidy or mondiby or something.

https://gist.github.com/wandernauta/6800547

It's just a gist! That's how simple it is! Amazing.

So, wherever you keep your code, maybe in `~/code`? Start yourself a nice new file called `control-spotify.sh` and paste the code from that gist into it. Save. Then we'll make a symlink to that script in somewhere that exists in your path, so you can execute this script easily.

Look at the output from `echo $PATH` - every path between colons (`:`) is where your computer will look for stuff when you type a command. My path includes `/usr/local/bin`, so I'm gonna put it there.

There's a couple more puzzle pieces around making the script easy to call. First, what will we type to invoke the script? I want to control spotify by typing `sp`, as inspired by the original gist title, because that's short and I'm lazy. You might choose `spotify` or `musica_pls_dj` or whatever.

So, to symlink the script:
```shell
ln -s ~/code/control-spotify.sh /usr/local/bin/sp
```

(PS, I never used symlinks before yesterday. Just in case you think "aw, symlinks, that is magic stuff I'll never understand", since that's what I thought 3 days ago, and every day before that. ;) )

Make sure you specify the whole path `~/code/...` even if you're already in `~/code` and think you can simply do `control-spotify.sh`, because the next time you run `sp`, our friend computer doesn't know that and can't find the script. Sad face. :(

Then we want to make the script executable (files ending in `.sh` are not by default executable! idk why. probably to save us from our dangerous selves):
`chmod u+x ~/code/control-spotify.sh`
I actually used `chmod +x` (no `u`), which modifies the permission for everyone; adding `u` restricts it to the current user, if I understand correctly. I don't really know which one you should use. You do you.

So, now you should be able to do
`sp`
and it should print out a happy little list of possible commands.

Great! Now, I've already taken the liberty of actually putting the script in a directory so I could version control it using git, because 
- I love git
- I make a lot of mistakes - I prefer to live fast and crash often :D
- I know I will want to make changes, because I can see a lot of comments in the gist

The first thing I did was try out each command by having a window open with the help output from running `sp`, and another window open to run each command. Oh, also, open spotify first. I installed it using `snap` as recommended for linux. Right, so here I am gleefully rejoicing when `sp play` and `sp pause` and `sp next` all work! But then `sp previous` doesn't work, it just stops the music, and it won't restart using `sp play`. Sad face. :( On further investigation, it turns out my brain translated `prev`, which is the actual command, into `previous`, which is not a recognized command and therefore does not have the intended outcome. Ha. So I made a wee change to the script, adding another alias under `sp-prev` for `sp-previous`. And updated the version from `0.1` to `0.2.0`, because

1. I like [semantic versioning](https://semver.org/) (x.y.z, rather than x.y), and
2. I added a feature, so it gets a "minor" bump (bug fixes get a "patch" bump and breaking changes get a "major" bump)

Next, I can keep trying out commands. Most of them work, which is frankly incredible to me considering so little code. After all the cruft I was wading through before, piling on packages and addons in an attempt to get anything functional. KISS Keep it simple stupid!! <3 Some like `sp url` don't work, which I don't need so I don't care about 8), and some like `sp display` need additional packages installed, but I don't need to look at album art, so I also don't care about that. :) Whee!!

The big trick though is `sp search`, which is really what I need - this is how you find songs to play. And spotify has apparently updated their API to require authorization so the original code does squat. D: Let's take the [changes @vorbeiei suggests](https://gist.github.com/wandernauta/6800547#gistcomment-2113314) and try those by replacing the `sp-search` function in the code. (There's a comment just after from someone who made `curl` silent for a nice clean output, which is great for when it's working, but I want all the details I can get while developing!)

Then we need our client ID and secret - follow the [spotify docs to register an app](https://developer.spotify.com/documentation/general/guides/app-settings/#register-your-app), kindly linked by github user @vorbeiei in their comment. Make your ID. Guess how to insert it into the script, and come back in a couple paragraphs for story time and instructions.

To test, I'm doing a nice easy search for "Moonshine" (by Caravan Palace) - it was on my spotify home page and it's a one word search term, which I assume can only help my cause in getting the search to be successful. It looks like the search syntax is either `sp search moonshine` or `sp moonshine`, but neither of them start moonshine playing. They just spit out curl progress, as if things were making good little requests. To debug, we need to drill down into the script a bit more. I took the first `curl` statement out and started fiddling with it in the command line directly.
`curl -H "Authorization: Basic client_id:secret" -d grant_type=client_credentials https://accounts.spotify.com/api/token`

Are you ready for the aforementioned storytime? First I will tell you how to correctly input your freshly generated credentials. Then I will tell you all the ways you could do the _wrong_ thing, all of which I did. Teehee.

Do do this:
- copy your client_id and secret from the spotify developer page for the app you registered
- stick your client_id and secret together, joined by a `:`
- then base64 encode that whole string, without any newlines (`base64 -w 0`)*
- jam the whole thing in the auth string for curl: "Authorization: Basic happily_encoded_stuff" 

Don't do this:
- plain old insert your client ID and secret as "Authorization: Basic my_client_id:secret"
- nor base64 encode each part seperately, then join them with a `:`
- nor base64 encode the whole thing, without removing newlines

I don't really know what base64 encoding is, so I just thought the credentials I copied from spotify were already encoded. That was my first problem. The second was trying to encode each part separately then join them with a `:`. Both of these were leading me to a `{"error":"invalid_client"}` error from curl. Anyway, it took some noodling as a bash noob, but my eventual process to encode all of this was:
```shell
CREDENTIALS=dxfjgbjbkjhgfbjhwhatevermyclientidcopieddirectlyfromspotifyis:jbjhkbkjhgbfsdjhgbkjhsfwhatevermysecretcopieddirectlyfromspotfyis
# make sure it worked
echo $CREDENTIALS
CREDS_ENCODED=$(echo $CREDENTIALS | base64 -w 0)
echo $CREDS_ENCODED
# you should have a single line of 39yijb7hi23h7i!! who doesn't love that stuff.
# now, in the curl command, make the auth string look like:
# "Authorization: Basic $CREDS_ENCODED"
```

Also, I highly recommend adding `--verbose` as the first argument to `curl`, so you can see exactly what gets sent once the `$CREDS_ENCODED` gets evaluated.

The next problem I ran into was a `stream was not closed cleanly: PROTOCOL ERROR`. I had some suspicion that this was due to the line break in the encoded credentials, but I thought that line break was part of the encoding, so what could I do about it?! Of course, [stack overflow to the rescue](https://superuser.com/a/1225139) - It turns out you can pass a flag to `base64` to "wrap". Sweet! Next problem! :sweat_smile:

So with that all sorted out, now we're back to `{"error":"invalid_client"}`, except now the object contains a new key too: `{"error_description":"Invalid client secret"}` -_-. Gah!

To solve this, since googling the error wasn't getting me anywhere, I searching in the github issues and started checking each one that seemed relevant. Eventually I got to one where somebody responded with [very clear instructions](https://github.com/spotify/web-api/issues/1372) to use this site for base64 encoding. Failing other leads, I tried it, and compared the result to my local `echo $CREDS_ENCODED` - unbelievably, somehow the final character differed, and when I used the web-encoded string, IT WORKED! I got my `access_token` in the response! No idea what is happening there. None. At all.

Anyway, onwards to moonshine! Add ` \>> token\` to the end of the curl call we were running, then run it again. PS, this is super hacky. You've been warned. ¯\\_(ツ)_/¯ Open the token file, delete everything except the token itself, even the quotes around it. Then in the terminal, run `TOKEN=$(cat token)`, and `echo $TOKEN` - you should see your token. We are starting to feel like [wizards](https://jvns.ca/blog/2017/12/01/new-zine--so-you-want-to-be-a-wizard/) at this point. ;D Now take the second curl statement in the sp script, stick it in the terminal, and replace the authorization `$ST2` with `$TOKEN`. 

By the way, I would totally just have pasted the token directly into the curl command if I could, but I just re-installed my OS and I haven't figured out how to copy from either the terminal or vi yet. Just saying. ;)

In the curl statement, we also have to replace the `"q=$Q"`, because we don't have a `$Q` defined. (It is defined a couple lines above in the script - we could just copy that and define Q in the terminal, but I like inlining it for clarity.) So instead, put `"q=$@"`. Haha, no wait. I tried it and it turns out the q is for query - if you run the curl with `$@`, you get a "400: no search query" response. That's kindof cool! Did you know that? Now you/I/we know. (Bill Nye reference because he's great.) Ok so back to reality, replace that q bit: `"q=moonshine"`. Oooh we're so close I can taste it! :joy: But why doesn't the script work?! If only I could put a debugger in...huh, the internet suggests `set -x` in the script to debug a shell script, so I put it as the first line in the `sp-search` function. Ooh, it sure spits out some info, but nothing particularly enlightening. Maybe I will settle for trying to extract each part to a variable, and hopefully that will mean more helpful output.

So, if I use `>> token_response` after the first curl, then `cat token_response | grep -E -o "access_token\":\"[a-zA-Z0-9_-]+\"" -m 1`, I do get the access token extracted, so that's cool! It looks like that gets assigned to `ST` and then `ST2` becomes characters 15 through 86 of that, so presumably the token itself. That leaves the final `grep` statement as the final possible culprit, so let's pull it out too and try it in the terminal. (PS, when making hot and sour soup, do cut the daylilies in half. Even short ones are too long. Otherwise you get the spaghetti effect of flinging sauce/liquid everywhere.) Huh, this grep statement seems to work nicely too, returning `spotify:track:ID`. Does the problem then lie with `sp open`? Ack! If I take that string, and put it on the end of `sp open` it works! As a sidenote, I think I am going to discover lots of new music with this - now I am listening to a random song called moonshine that I've never heard before. :joy:

Oh god, this whole time I thought the bash output produced when I set `set -x` was masking the credentials variable somehow, but in reality it was just ...nothing. So the first query fails, so everything else fails. Facepalm! Preliminary googling reveals [answers from Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-read-and-set-environmental-and-shell-variables-on-a-linux-vps)...

> We now have a shell variable. This variable is available in our current session, but will not be passed down to child processes.

D'oh! I need `export` to make `$CREDENTIALS` available to the `sp` script.
```shell
export CREDENTIALS
```

In the long term, that needs to be exported in my `/.zshrc`, with the actual base64 encoded string.
```shell
export CREDENTIALS=jhdfgkjsdbfkugabuyb34ouyfbosdufgyurWHEEEwhatever
```

Aaaand I'm feeling like a genius, now that it finally works! Thanks for tuning in, friends and countrymice. :) 

PS, here's the final version, nicely wrapped up in a repo with condensed instructions! Enjoy!

[https://github.com/heatherbooker/spotify-cli](https://github.com/heatherbooker/spotify-cli)

*Don't do `base64 -w 0`. I still don't know what it does weird, but...just keep reading.
