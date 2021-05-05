---
layout: post
title: How to use Sails.js with Pug
tags:
- code
- how-to
made-at-rc: true
---
You may know by now that I like to make [tutorials for things with dismal search results](/blog/posts/2016-08-24-how-to-vue-directive-npm), and this is no different. The search results when I queried for the title of this post included the following:

![pugs on sailboats. not the sails or pugs I wanted.](/blog/img/use-sails-with-pug.png)

Yeah. Not exactly what I was looking for.<!--more--> So I started by following [a video tutorial](https://www.youtube.com/watch?v=ep6EQ5f82Ts&list=PLf8i4fc0zJBzLhOe6FwHpGhBDgqwInJWZ&index=3) (just kidding, actually I started by blindly trying to merge the basic sails app with my pre-existing code, which was painfully unsuccessful) to the T just to get an idea of how sails works, seeing as I’m still more than a little confused about routing and http and servers and databases. I shall now extrapolate the process of converting this project to use pug, to a tutorial on starting a sails app with pug.  

Normally you would be able to generate a project using a different templating engine than the default EJS, but it seems the name transition from Jade → Pug is not complete. The closest we can get (until [the Pug views generator](https://www.npmjs.com/package/sails-generate-views-pug) is published…) (**Update: it’s a thing!**) is to run
```bash
sails new puggity-project --template=jade
```
in the terminal to make a new sails project called ‘puggity-project’. Be sure that you have sails installed globally before trying to run this command — you can do that using the following command (also in the terminal) :
```bash
npm install sails -g
```
You may need to preface the command with ‘sudo’ if this command spits back a bunch of mumblebump about permissions.

Now we need to change all of the Jade config to Pug, including changing filename extensions in /view to .pug, installing pug, and changing the view engine.
```bash
cd puggity-project/views
for file in *.jade; do mv $file `basename $file .jade`.pug; done
npm i --save pug
```
Open config/views.js and change the line that says __engine: “jade”__ to say __engine: “pug”__.
You should now be able to run
```bash
sails lift
```
or
```bash
npm start
```
(whichever you prefer — “sails lift” is their cute start command but for consistency if you work with other npm projects, you might appreciate using “npm start”.)
From this point you can basically follow [this tutorial I linked above](https://www.npmjs.com/package/sails-generate-views-pug), being mindful that when the EJS is mentioned, you will need to take the appropriate action but using Pug syntax/files.

Some little notes:
- In new .pug files, you will need to start with
```pug
extends ../layout
block body
```
- For ep.3: watch the updated version (which is later in the playlist)
- In ep.3: the `sails generate user` command is going to need to be `sails generate api user`
- After running running `sails generate api user`, you may need to go into config/models.js and set uncomment the “migrate” line (I also changed it to `migrate: ‘safe’ `, because I like the sound of “safe”!) to avoid command line error msgs

In episode 3, you create a form with one input as follows:
```html
<input type=“hidden” name="_csrf" value=“<%= _csrf %>" >
```
However, the <% … %> syntax is specific to EJS templates; we need to puglify it! (Not uglify…pug syntax is definitely prettier to my eyes!) Our version will look like this:
```pug
input(type="hidden" name="_csrf" value= _csrf)
```
Voila! C’est beautiful.  

Then in episode 5, you get into making variables available to your views, which you can read more about in the locals documentation of Sails. They can then be accessed in the Pug-typical way (as seen above for the input “value” attribute), by appending an equals sign, a space, and the variable name to either an attribute or a tag.  

There you go! I hope this helps you get started whipping up a Sails.js + Pug app. If you need more inspiration or to see some code, check out my sails + pug project code or the app itself! Good luck and have fun! :)

*Note*: This post was migrated from its [original home on Medium](https://medium.com/@heatherbooker/how-to-use-sails-js-with-pug-d3ded437c895#.drv7vifry) on February 23, 2017.
