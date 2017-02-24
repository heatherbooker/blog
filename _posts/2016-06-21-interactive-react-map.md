---
layout: post
title:  "How to make an interactive map in React"
categories: code
tags: code how-to
---

I am going to share with you a great and magnificent secret: the stages of creating a clickable svg map in a React project. Are you ready? You might want to take notes, because it is super mind-blowing.  

Wait, first here’s the map, being clicked on (ooh, ahh):  

![clickable map being clicked]({{ site.url }}/assets/img/map-being-clicked.gif)  
<!--more-->

(And if you just want to skip straight to the good stuff, click [here to get my clickable SVG-based React component world map](https://www.npmjs.com/package/react-world-map) on npm!)  

If you are ready, let us begin:  

Stages of creating a clickable SVG map in a React project  

1. Hatred
2. Suffering
3. Defeat
4. Suffering
5. Hatred
6. Defeat
7. Hatred
8. Hated
9. Utter hatred
10. SUCCESS!
11. Just kidding. Have you seen this? Because this. (Edit: that link was broken, [try this](https://s33.postimg.org/niqd2zgwf/code.jpg), or failing that, image search “how to do coding”.)  

Hahaha..haha..ha…*lying on floor*  

No, but actually. So it was hard. For me. A majority of that, I reckon, is attributable to my lack of experience with various tasks and technologies, such as setting event listeners and using React. However, in the end, I don’t think I would actually classify it as too difficult a project for the average front-end developer to take on.  

If you are genuinely interested in creating any sort of clickable SVG-based components for your React application, I have compiled the basic steps I ended up taking to complete my map project below. Good luck and have fun!  

When I started out, I had webpack [file-loader](https://www.npmjs.com/package/file-loader) set up to handle svgs:

```js
...
const myImage = require(‘./aFile.svg’)
class clickableSvg extends React.Component {
 constructor() {
  super();
 }
 render() {
  return (
   <div>
    <img src={myImage}>
   <\/div>
  )
 }
}
```

(If you’re not familiar with ‘class’ or ‘const’ in js, they are new to ES6.)  

However, embedding the svg file in an `<img>` tag like this means that any links within the svg code are not valid. [Wait, did I just say svg code? Aren’t svgs just images? Oh yeah! You can edit svg code — it’s a markup language like HTML, made of `<tags>`. Cool.] Sources suggested other options — using an `<object>` or `<iframe>` tag instead, or directly embedding the svg code in the document. I wasn’t keen on the last option, as I felt the svg code for an entire world map was terribly bulky and would make my code messy. I set upon using the iframe tag, in which I could add links around paths/objects, using `<a xlink:href=“#”>`.  

But I could go on forever about things that didn’t work. Let’s summarize some of the key points to making your clickable svg component work with React:  

- the svg code must be directly embedded in your React component
- React will not accept any colons (:) or dashes (-) in tag properties — you can convert to camelCase to circumvent this
- links must use the property xlinkHref, not simply href
you can use event listeners to communicate that a component has been clicked   

(sidenote: I’m not entirely certain that using event listeners isn’t a slightly hacky way to accomplish communication between components, but it worked for my purposes of alerting users of the map as an npm module )
- to change css when a component is clicked, assign it a variable for a className, and then use an onClick to manipulate the className  

for me, this looked as follows:
```svg
<g id=”AF” className={this.mapState.af} onClick={this.onMapClick.bind(this, ‘af’)} >
 <path id="path4307" d="M345.902 112.802c-.17.07 … " />
</g>
```
AF represents Africa, which contained two paths. I grouped the paths in Inkscape (where I could easily view which should go together), then in Sublime I assigned a className. Then I added an onClick to call a function which is defined in the class definition, at the same level as the constructor and render functions.  

The classNames are originally set either in the constructor using this.mapState = {<see code below>} if you are using ES6 classes, or the componentDidMount function if using React.createClass:  

```js
componentDidMount: function() {
 this.mapState = {
  na: "map-unselected",
  sa: "map-unselected",
  af: "map-unselected",
  //etc through all continents
 }
},
```

To make my code as clean as possible, after grouping elements into the areas that will need to be individually manipulated (ex continents), I used an svg optimization tool. I thought I would need path IDs so I instructed the tool not to remove them, but it turns out I haven’t needed them so they could have been removed. One option you might need to uncheck though, is for removal of unnecessary groupings — the tool may see the groupings we just worked hard to create as unnecessary, and that is definitely not what we want!  

Now, getting into the nitty-gritty of it: The map component has the following features in the source code:
- React state declared in constructor(ES6) or getInitialState(ES5) and used to track “clicked” state:

```js
getInitialState: function() {
 return {clicked: ‘none’};
},
```
- Fire an event when clicked, denoting current clicked state:

```js
componentDidUpdate: function() {
  this.emitEvent();
 },
 emitEvent: function() {
  const clickedEvent = new CustomEvent(
   ‘WorldMapClicked’,
   {detail: {clickedState: this.state.clicked}}
  );
  window.dispatchEvent(clickedEvent);
 },
```
The event is dispatched on componentDidUpdate, which guarantees that it is receiving the new state. Firing an event allows users of the npm module to easily track what is happening (again, I think this might be hacky).  

There are two functions which handle the state and classNames — the first determines what to set `this.state.clicked` to by using some if/else statements and comparing the area clicked to the previous state, and the second manages classNames for css purposes by resetting all to the default, unselected, then if necessary, changing only the selected area to have a className reflecting that.  

It is important to note that the states should be suitable class names, such as “map-selected” and “map-unselected”, as they are assigned as className attributes to the groups in the svg! These classNames can then be referenced through CSS to change the appearance.  

Tune in next week for some choice words on making a React component available as an npm module and through a CDN.  

And that’s it! That’s all there is to using SVG to its full clickable, CSS-able potential. Don’t forget to [check out the project on github](https://github.com/heatherbooker/clickable-svg-map) if you need any clarification, or would like to use it yourself! Happy map-making!

*Note*: This post was migrated from its [original home on Medium](https://medium.com/@heatherbooker/how-to-make-an-interactive-map-in-react-f4e6e074b500#.entvl9iu5) on February 23, 2017.
