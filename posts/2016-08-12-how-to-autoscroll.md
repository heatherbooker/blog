---
layout: post
title:  "How to auto-scroll to the bottom of a div"
tags: code how-to
---

This article is just going to be a little nublet of an article. Or a nugget. Whatever.  
<!--more-->
I found it ridiculously difficult to add what I thought was a common, simple feature to a web app: the ability for a div to automatically scroll to the bottom whenever new content is added. I didn’t want the page to scroll, and I didn’t want the div height to grow, or the content to expand outside the div.  

I tried using a library which is supposed to provide this functionality, but I was having no luck. It would sometimes jump around for the first couple of additions, but was terribly inconsistent, and I couldn’t understand the source code well enough to tweak and make it work.  

Next, I did it the hard way myself — by following a summary of how to accomplish it with some sample code using requestAnimationFrame. This was an excellent way to learn more about requestAnimationFrame, which was something I wanted to do anyway. It also eventually worked and solved my problem! Rad.  

But get this — it turns out there’s a better way. Introducing the under-appreciated Mutation Observer. Check it out:  
```js
// Get a reference to the div you want to auto-scroll.
var someElement = document.querySelector('.className');
// Create an observer and pass it a callback.
var observer = new MutationObserver(scrollToBottom);
// Tell it to look for new children that will change the height.
var config = {childList: true};
observer.observe(someElement, config);
```
MutationObservers are available in your browser. *In your browser!* They’re not some fancy shtuff. Well, they are fancy shtuff, but you don’t have to worry about how they got so fancy, which is what counts in my books.  

Now, that’s all well and good, but how do you do the actual scrolling?!  

Two options: jump or smooth scroll.  

Jump:
```js
function scrollToBottom() {
  someElement.scrollTop = someElement.scrollHeight;
}
```
Smooth Scroll:
```js
// First, define a helper function.
function animateScroll(duration) {
  var start = someElement.scrollTop;
  var end = someElement.scrollHeight;
  var change = end - start;
  var increment = 20;
  function easeInOut(currentTime, start, change, duration) {
    // by Robert Penner
    currentTime /= duration / 2;
    if (currentTime < 1) {
      return change / 2 * currentTime * currentTime + start;
    }
    currentTime -= 1;
    return -change / 2 * (currentTime * (currentTime - 2) - 1) + start;
  }
  function animate(elapsedTime) {
    elapsedTime += increment;
    var position = easeInOut(elapsedTime, start, change, duration);
    someElement.scrollTop = position;
    if (elapsedTime < duration) {
      setTimeout(function() {
        animate(elapsedTime);
      }, increment)
    }
  }
  animate(0);
}

// Here's our main callback function we passed to the observer
function scrollToBottom() {
  var duration = 300 // Or however many milliseconds you want to scroll to last
  animateScroll(duration);
}
```
I hope this helps you. If you’re using Vue.js, you can check out [vue-sticky-scroll](https://github.com/heatherbooker/vue-sticky-scroll) (alternate name: vue-glue?), a directive I published to simplify the lives of other people who also want what I wanted. I also wrote recently about the [making of Vue.js directives](/posts/2016-08-12-how-to-vue-directive
) in general. Recently being like 20 minutes ago. Woohoo for Vue.js!

*Note*: This post was migrated from its [original home on Medium](https://medium.com/@heatherbooker/how-to-auto-scroll-to-the-bottom-of-a-div-415e967e7a24#.etorqdgbe) on February 23, 2017.
