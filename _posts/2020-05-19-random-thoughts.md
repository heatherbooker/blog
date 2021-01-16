---
layout: post
title: "Random thoughts"
tags: thoughts code
---

There is a super cool thing a friend of mine did, which I wish to emulate: have an adorable [page of random thoughts](https://glit.sh/~wesleyac/thoughts/). It seems super low pressure, fun as a reader to get a glimpse into someone else's head, and fun as a creator to just collect a mish-mash of glorious tidbits.

The [original implementation](https://github.com/marenbeam/thoughts) doesn't involve any manual git-anythings, and makes a nice separate page for these random thoughts. However, I (apparently) am never going to get around to doing that, so a blog post that I update with the thoughts seems good enough. It will be weird, but I am fine with that. :D

<!--more-->

19 May, 2020

First thought is this hilarious ad on a website for a local music store that I thought was very cute and saved a few weeks ago, for this exact purpose:

![steves music store - wash those hands!]({{ '/assets/img/steves.png' | relative_url }})

20 May, 2020

I think this format will not do; it is going to make me censor myself more, because this feels like a conversational blog post rather than a collection of random thoughts. But we must try anyway.

20 May, 2020

I am a parasite to humanity.

21 May, 2020

[I did it!]({{ '/thoughts' | relative_url }}) Bye!

23 May, 2020

So! How did I do it? Mostly in [this commit](https://github.com/heatherbooker/blog/commit/60a3504cbddbf12fc16e290ea313b8119a7b4f08) (where I have omitted less relevant parts from this post):

```git
{% raw %}
commit 60a3504cbddbf12fc16e290ea313b8119a7b4f08
Author: hboo <nope>
Date:   Thu May 21 22:51:00 2020 -0400

    Add proper thoughts page!
{% endraw %}
```

So exciting! Look at that exclamation mark in the commit message! I had to specially write it in vim because I use zsh instead of bash and it gets confused about the exclamation mark when I use it with `git commit -m`. (Maybe bash gets confused by `!`s in quotes too. I don't know.) Anyway! Are you ready for the actual good stuff to begin? It's mostly just jekyll stuff (using [collections](https://jekyllrb.com/docs/collections/)):

```git
{% raw %}
diff --git a/_config.yml b/_config.yml
index 58693b8..8235bb6 100644
--- a/_config.yml
+++ b/_config.yml
@@ -34,6 +34,8 @@ exclude:
   - Gemfile
   - Gemfile.lock

+collections:
+  - thoughts

 owner:
   name: Heather Booker

diff --git a/thoughts.html b/thoughts.html
new file mode 100644
index 0000000..45572c7
--- /dev/null
+++ b/thoughts.html
@@ -0,0 +1,18 @@
+---
+layout: page
+title:  Thoughts
+permalink: /thoughts/
+---
+
+<div class="thoughts">
+  <div>
+    {% for thought in site.thoughts reversed %}
+    <span class="post-meta">
+      <span class="post-date">
+        {{ thought.date | date: "%-d %b %Y" | upcase }}
+      </span>
+    </span>
+    <div>{{ thought.content }}</div>
+    {% endfor %}
+  </div>
+</div>
{% endraw %}
```

What's next?

```git
{% raw %}
diff --git a/_posts/2020-05-19-random-thoughts.md b/_posts/2020-05-19-random-thoughts.md
index a175619..153c9a4 100644
--- a/_posts/2020-05-19-random-thoughts.md
+++ b/_posts/2020-05-19-random-thoughts.md
@@ -14,7 +14,7 @@ The [original implementation](https://github.com/marenbeam/thoughts) doesn't inv

 First thought is this hilarious ad on a website for a local music store that I thought was very cute and saved a few weeks ago, for this exact purpose:

-![steves music store - wash those hands!](/assets/img/steves.png)
+![steves music store - wash those hands!]({{ '/assets/img/steves.png' | relative_url }})

 20 May, 2020
{% endraw %}
```
Ack! That path fix shouldn't be in this commit. -_- Darnit, that's what I get for mixing up doing a million things at once >< The paths are a bit more complicated than I was imagining, because the blog thinks it is at `/` but actually it is at `/blog/`, so I can't just throw absolute paths around and call it a day. Puh.

Tell the world!:

```git
{% raw %}
@@ -23,3 +23,7 @@ I think this format will not do; it is going to make me censor myself more, beca
 20 May, 2020

 I am a parasite to humanity.
+
+21 May, 2020
+
+[I did it!]({{ '/thoughts' | relative_url }}) Bye!
{% endraw %}
```

Nice.

Next: The original implementation that inspired me strips all newlines, but I want my newlines. However, I don't want the paragraphs to be spaced as far apart as in regular posts - it makes it harder to scan the page. So I take away the margin from every paragraph that _follows_ a paragraph by using the `element + element` CSS selector. Because the default styling on the blog for paragraphs is to have `1.2em` both above and below each one, simply setting the `margin-top` isn't enough - the bottom margin of the paragraph above it is still pushing it away. (Those margins were previously being [collapsed](https://css-tricks.com/what-you-should-know-about-collapsing-margins/).) So it needs a negative margin to overcome that.

```git
{% raw %}
diff --git a/_sass/pages/_thoughts.scss b/_sass/pages/_thoughts.scss
new file mode 100644
index 0000000..f2fd639
--- /dev/null
+++ b/_sass/pages/_thoughts.scss
@@ -0,0 +1,5 @@
+.thoughts {
+  p + p {
+    margin-top: -1.2em;
+  }
+}
{% endraw %}
```

And import that style file:

```git
{% raw %}
diff --git a/_sass/leonids.scss b/_sass/leonids.scss
index 225ff25..f338ed7 100644
--- a/_sass/leonids.scss
+++ b/_sass/leonids.scss
@@ -21,3 +21,4 @@
 @import "pages/tags";
 @import "pages/archive";
 @import "pages/post";
+@import "pages/thoughts";
{% endraw %}
```

I guess there's no scoping of css to certain pages automatically by jekyll; it has to be done manually. So I actually first released the blog with all the paragraphs being squished together, because I was SO EAGER and didn't double check the other pages. Just the new one. :joy: It was only afterwards that I addded the `.thoughts` scope to the `p + p`. Regression check, my friends!

Last but certainly not least is the glorious thought generator. I largely copied the existing new post generator that I use ([by Xianny]({{ site.url }}{% post_url 2017-02-28-xiannys-awesome-jekyll-post-generator %}) (who I met!! it was so cool! people are just the best.[^1])), but simpler because thoughts don't have titles, or tags.

```git
{% raw %}
diff --git a/thought b/thought
new file mode 100755
index 0000000..ac77be4
--- /dev/null
+++ b/thought
@@ -0,0 +1,19 @@
+#!/bin/bash
+# This script creates a new thought in jekyll with pre-filled front matter.
+# Run `./thought` to generate a thought.
+
+FILEDIR="_thoughts"
+FILENAME="$FILEDIR/$(date +%F-%H-%M-%S).md"
+
+echo "---" > $FILENAME
+echo "date: $(date '+%F %T')" >> $FILENAME
+echo "---" >> $FILENAME
+
+if command -v nvim
+then
+    nvim $FILENAME
+else
+    vi $FILENAME
+fi
+
+echo $FILENAME
{% endraw %}
```

Also, the filenames are based on date-time down to the second. I started this paragraph to say that it's not terribly robust, if you generate two thoughts within the same minute the second one will overwrite the first, but then I remembered it is _seconds_ not _minutes_, and it would be pretty hard to write two thoughts in the same second. So nevermind.

Anyway, so far the whole thing is pretty sweet! You are actually looking at a lot of manipulated git history - I am quite liberal with my interactive rebases and force pushes, which allows me to pretend I wrote everything right the first time, which I definitely did not. ;D

Next up, I made some improvements to this script and the new post generator script.

Every time I would finish editing a post, when I would close it, the path to my editor would be printed in the terminal, because of this line:

```shell
if command -v nvim
```

That `command -v` thing checks if `nvim` is available. But then if it is, it prints the path to nvim, `/usr/bin/nvim`, which is slightly confusing every time I see it. So the magical world of internet told me I could try appending `>/dev/null 2>&1` to the end of the check. I don't know what each part of it does, something about not mashing standard error into standard out, but it sends the output into oblivion aka `/dev/null`[^2] which is good enough for me! So that line became

```shell
if command -v nvim >/dev/null 2>&1
```

I don't really know what `/dev/null` is, except that it is oblivion. It is where perfectly useless information goes to die, get swept away and cleaned up, and not be accessed or accessible.

The next improvement brought the thought generator closer to that from which is was inspired. Since the original script goes through all of git add + commit + push, I wanted a little taste of that, but with the safety that comes with not pushing things to production automatically every time...since that is a little beyond my comfort level, as a mini control freak. :P

For the thought generator:

```shell
git add $FILENAME
git commit -m "Think a thought"
echo "You thinked a thought! Run 'git push' to tell the world! Or just to get it off your chest."
```

Everything except pushing the thought. Instead a cute reminder to push it.

Also `git commit` spits some output to the terminal, so you get to see the filename and commit, as a reminder that it really happened. ^^

Whereas for the new post generator, I kept it a little simpler:

```shell
git add -N $FILENAME
echo "New post \"$FILENAME\" has been added to git tracked files"
```

I struggled with the right wording for the second half of the message.

- "is now being tracked by git"
- "has been added to the list of tracked files in git"
- ?????

But what I chose seems as good as any. ¯\\\_(ツ)\_/¯

What I wanted for a new post was just for the _file_ to be tracked, but not for the contents to be staged. So I used `git add -N`, so that I can use `git add --patch` to manually review the contents that I am about to commit. I also wanted the filename to be printed, so I could remember which one I just wrote.

Anyway, I am pretty thrilled with how things came out! I think writing these little shell scripts is really fun, and I had never done it much before. The thoughts page itself is exactly as heckin adorable as I thought it would be. On the other hand, I am concerned that it is slightly addictive. I find myself thinking of cute one-liners all day long that could go there, and I have to resist the urge to put every thought I ever think on that page. Thanks for joining me on this trip! Make sure to think lots of thoughts today and always. :) (Sorry, this ending feels very sudden and uncoordinated. I hate reading articles like that. Um...bye!)

[^1]: "people are just the best." Help I sound like a dog !! :joy:

[^2]: True story, once while I was at [the Recurse Center](https://www.recurse.com/) (or maybe somewhere else, I don't know now that I think about it :joy:), I was showing someone something, or doing a presentation, and I had some code snippets that I was just playing around with in a folder called `~/dev/null`, because `~/dev` seemed like a reasonable location for all my code, and `null` seemed like a reasonable location for nonsense code. I had never heard of `/dev/null` and had no idea I was mimicking it, but whoever enlightened me certainly thought it was funny. :joy:
