---
layout: post
title: How to publish an Vue.js directive to npm
tags: code how-to
---

Wow, so I’m always amazed when something I want to know how to do doesn’t stare right back at me in the top 5 Google results. I’m sure you can guess why I am writing this article today (hint: check the title), but be assured that I have previously been unsuccessful in my mission to publish my Vue.js directive to npm. Please join me for a thrilling recount of my journey to enlightenment.  

<!--more-->

So the original version of [my directive](https://github.com/heatherbooker/vue-sticky-scroll) just had
```js
Vue.directive('name-of-directive', {
  // Definition
});
```
Stupidly simple. Perfectamundo. Works great when you just drag the file in, either by cloning/downloading or using a [CDN](https://rawgit.com).  

But it isn’t enough to make this work as an npm package. (I know because I tried it.) For that, we will need to sleuth a little more — I did this by investigating a bunch of [other people’s directives](https://www.npmjs.com/search?q=vue+directive) that were already available through npm. I think I must have sleuthed *too* hard the first time, because it turns out that making a Vue directive work as an npm package is also pretty stupidly simple.  

It can be done by essentially the same principle as making anything else available both through npm and a non-node environment (such as a CDN or direct use of the file), which is as follows:
```js
// Before you create your Vue.directive:
try {
  var Vue = require('vue');
} catch (e) {
  // It's all good, Vue should already be available globally.
}
// Then create your Vue.directive as a variable:
var myDirective = Vue.directive('', {});
// Finally, check if you should export it:
try {
  module.exports = myDirective;
} catch (e) {
  // No problem, it should be registered as is.
}
```
The try/catch clauses could also be `if` statements, where you check the environment before doing what needs to be done:
```js
if (typeof module !== 'undefined' && this.module !== module && module.exports) {
  var Vue = require('vue');
}
// Continue as above, replacing final 'try/catch' with above 'if'
```
But there is some discourse on the best way to do this — whether checking the first and last clauses is sufficient ([Underscore](http://underscorejs.org/docs/underscore.html#section-11) does it this way) or the first and second clauses are a preferable pair. Let me know if you have thoughts on the `try/catch` vs `if (module)` dilemma.  

Of course, everything should be wrapped in an anonymous, self-invoking function to prevent bloating of the global namespace if users are not using node.  
```js
(function() {
  // Do all the things;
})();
```
In any case, either of the above should be sufficient to make your directive available through both platforms. Now all you have to do is npm publish!  

*Note*: This post was migrated from its [original home on Medium](https://medium.com/@heatherbooker/how-to-publish-an-vue-js-directive-to-npm-e98600fb5d2f#.ngsetlhae) on February 23, 2017.
