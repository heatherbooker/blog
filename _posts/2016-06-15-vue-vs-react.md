---
layout: post
title:  "Vue.js vs React.js"
categories: code
tags: code
---

Ooh, this is going to be a juicy one! I’m sure many people are fierce supporters of one of these technologies and zealous bashers of the other. Both Vue.js and React.js are both pretty new to the scene — let’s get some background information to begin. I’ll go in reverse alphabetical order to avoid offending anyone right out of the gates. ;)  <!--more-->

First of all, Vue and React are very similar. They are both libraries intended to be used to make composable “views”(Vue’s words) or “user interfaces”(React’s words), with reactive data binding. Composable, in this case, meaning based on the separation of view concerns into units described as “components”. In terms of data, both recommend a uni-directional flow of data.  

[Vue.js](https://vuejs.org) was created and maintained by Evan You and released publicly in early 2014. It positions itself as a simpler, more straightforward implementation of the component-based data-binding view concept.  

[React.js](https://facebook.github.io/react/) was released a year earlier, after being created by Jordan Walke for use at Facebook. Frankly, until I began researching for this article, I had no idea there was a single person behind it — Facebook definitely gets the acclaim for React.  

Vue is arguably the underdog at the moment. React absolutely took the javascript world by storm — it is currently the 6th most starred project ever on Github, and you would be hard pressed to be a front-end developer currently and not have heard anything about it. Vue even directly likens itself to React in its “[(re)introduction” article](http://blog.evanyou.me/2015/10/25/vuejs-re-introduction/):  

> When it comes to structuring complex interfaces, Vue takes an approach that is very similar to React: it’s components all the way down.  

The uni-directional data flow is an idea originally referred to by React as “Flux”. In this idea, data changes flow through a central container of state before they are sent back out to wherever the data may be needed. They each have their own personalized versions of Flux — the most popular for React is Redux*, written by Dan Abramov. Since its inception, Redux has been widely praised by the community as well as the React team at Facebook themselves. Redux is also compatible with Vue, though Vuex is an architecture similarly based on Flux while being more specifically tailored to Vue.  

Alright. Now that all that factual stuff is out of the way, lets talk about our feelings. My first foray into React was decidedly painful. I had never used such an architecture before, and many people agree with me on the following point: the documentation is not beginner friendly. In this excellent article [ReactJS for Stupid People](http://blog.andrewray.me/reactjs-for-stupid-people/), Andrew Ray points out React’s bewilderingly full and disorganized sidebar with multiple, competing entry points. React’s use of JSX, which looks like HTML tags written in the middle of a JS file, makes things hard enough, without additions such as — a mix of ES6 syntax and “safe” JS throughout React’s documentation and the hoardes of other tutorials found throughout the interwebs, superfluous yet pervasive use of webpack(which also has notoriously poor documentation), and the whole flux/redux/state/props mess(at least in my brain it was a mess). All in all, I think three things are the root of my lesser valuation of React:  
1. A ton of nearly inextricable tools (webpack, babel, jsx, redux/reflux/alt..)
2. Unnecessary complication, in my opinion, compared to Vue
3. Confusing documentation  

These three aforementioned items are surely related. However, I feel that Vue scores significantly higher than React on all three. I barely needed to venture away from the official Vue docs to get started using it; even when I did turn to Google, I frequently ended up back in the official docs to find my answer! To me, this is a major win — to have a technology clear enough that it can be sufficiently explained in one go, and to sufficiently and clearly do so. I expect that the Vue docs were written by the creator of Vue, which would afford the absolute clarity that accompanies such a position.  

I must end by saying that I enjoy using both — I appreciate React’s current far-reach, as well as Vue’s dedication to simplicity. But I can’t help but lean towards team Vue. In any case, long live component based UIs!  

*Redux is technically not flux. But it was heavily inspired by it.  

*Note*: This post was migrated from its [original home on Medium](https://medium.com/@heatherbooker/vue-js-vs-react-js-28caa8f9b033#.ojssrqrl3) on February 23, 2017.
