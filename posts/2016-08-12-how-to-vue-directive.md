---
layout: post
title: How to make a reusable “directive” in Vue.js
tags:
- code
- how-to
---

I like Vue.js. A lot. (See my [comparison with React.js here](/posts/2016-06-15-vue-vs-react).) It is a beautifully simple, straightforward, and superbly functional view-layer framework. I’ve gone over most of the docs multiple times, and I am absolutely enamoured with the clarity they exude. They don’t use stupidly obscure words or send you around in circles trying to find what you want. They make you and Vue Just Work.

However, I had a weak spot — the “Custom Directives” page. <!--more-->I avoided it for the longest time. Not being familiar with Angular (which everyone else seemed to be comparing these “directives” to those “directives”), I did not understand what a “directive” was or why I might want to use it. I couldn’t find a page explicitly labelled “Normal Directives” so I decided these custom “directives” were probably super insanely advanced.  

Anyway, as it turns out, smack dab on the [Vue.js overview](https://vuejs.org/guide/overview.html) page is a succinct introduction to “directives”:  

> Directives are prefixed with v- to indicate that they are special attributes provided by Vue.js, and as you may have guessed, they apply special reactive behavior to the rendered DOM.  

Yup, that is right out of the docs. Apparently it was *too* succinct for me. If any of you are like me, let me see if I can break down what a “directive” is even more.  

“Directives” in Vue.js are handy little tools or services that *do something* to a component. Simply by adding “v-your-directive-name” as an attribute (like “class” or “v-bind:whatever” or “style” are all attributes) in your markup as follows —
```html
<div v-some-directive>BLAh blaH BLAH or whatever</div>
```
— you will find that this div now has whatever behaviour the directive specifies.
But what is the difference between:
- a directive and a component?
See, a component *is* a thing. A directive *does* a thing.
Clear as mud? Good. (We’ll get to an example soon.)
What about the difference between:
-a directive and a plugin?
Wait, what *is* the difference between a directive and a plugin?  

Well, on this distinction, I admit I really *am* seeing it clear as mud. It seems that a directive can be, or become, a plugin. Or is it that plugins can be encapsulated in directives? Perhaps if I end up needing to make a plugin, then my comprehension will be strengthened. Please do let me know if you are wiser than I on this topic.  

Anyway, now that I’ve bored you to death with the blabbing, let’s dig right in and see a useful example of a custom directive. [Check out the code on Github](https://github.com/heatherbooker/vue-sticky-scroll).  

I created a directive (as all great things are created) out of necessity: I wanted a div of a fixed height to always auto-scroll down to the bottom if new content is added. A common problem you would think, no? Various attempts at solutions took hours upon hours — using libraries, requestAnimationFrame, home-hacked fixes, until I was satisfied. Then someone pointed out the excellent browser feature, Mutation Observers. This was an infinitely simpler implementation of the behaviour I desired.  

(If what you are really after is how to solve this same problem, I wrote about the exact implementation [here]().)  

My original directive was not a directive. It was just a function that took the class or id of the div in question as an argument, and used that to attach the behaviour to the element. An Angular.js veteran recommended that I extract this functionality into a directive, so that it would automatically attach to the desired element. What witchcraft is this?! That was my reaction to this push that I needed to finally explore directives.  

Oops, that was definitely not digging right in. That was blabbing.  

Javascript:  
```js
Vue.directive('sticky-scroll', {
  bind: function() {
    var observer = new MutationObserver(scrollToBottom);
    var config = {childList: true};
    observer.observe(this.el, config);
    function scrollToBottom() {
      // whatever it takes to scroll to the bottom
    }
  }
});
```
Html: (template in your Vue component)  
```html
<div v-sticky-scroll>
  <!-- new elements will be added here, that's why we want to scroll!-->
</div>
```
Let’s talk about that.  

The name of the directive is ‘sticky-scroll’, which must be prefaced with ‘v-’ in your template html. The ‘bind’ method found in the directive is one of three possible: bind, update, and unbind, called respectively when the directive is attached, modified, and detached from an element.  

This.el is one of several properties available through the directive-element interface, and is a direct reference to the element. Other properties can specify additional arguments and parameters to modify the directives activity.  

You can now clearly see the distinction between the mysterious “directive” and a component: in the JS file for the directive, there is no html markup. Only behaviour. In contrast, our component has markup, to which the directive is applied.  

Finally, I want to talk about making your directive reusable and available to the whole of the Vue.js community. Sharing is caring, as they say, and Vue.js is currently maintained by essentially one person. This incredible feat merits some serious love, and contributing useful features and stuff you create is a great way to spread the love. Some tips for doing this in the most awesome way possible:  

- For maximum simplicity, use a CDN like [RawGit](https://rawgit.com) — you can just upload your single directive file to Github and make it available through a CDN
- Make sure your code is clean and readable — users might have problems or want to modify your source code. // Comments can’t hurt.
- Throw in a readme.md with a description and usage instructions, and an example preferably
- Then, submit a PR with your shiny new directive to [Awesome Vue](https://github.com/vuejs/awesome-vue) for the world to see!

*Note*: This post was migrated from its [original home on Medium](https://medium.com/@heatherbooker/how-to-make-a-reusable-directive-in-vue-js-b28e1dfd76a3#.bt5ya37q2) on February 23, 2017.
