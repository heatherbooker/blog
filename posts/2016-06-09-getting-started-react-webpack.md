---
layout: post
title:  "Getting started with React and Webpack"
tags:
- code
- how-to
---

I [recently wrote about](/blog/posts/2016-05-28-if-gulp-were-a-person) the wonders of gulp. Ironically, as soon as it was published, I ditched gulp for webpack. Actually that’s not quite true — I first tried to incorporate browserify into gulp. I knew I needed some sort of packaging system in order to use JSX and React, and of these two most popular, browserify and webpack, webpack was touted to be a real mind-bender to set up. Everywhere I looked suggested that people who weren’t terribly experienced with other module-requiring systems like CommonJS should probably use browserify. So I got to work setting it up, only to realize that webpack would pay off in the long run.
  <!--more-->

Scratch browserify. Move on to webpack. The internet was right — it was a pain to set up. Let’s dig right in and see if we can’t set up our own project right now using React and Webpack. (If you can’t be bothered to follow along, feel free to git clone my [Webpack-React starter](https://github.com/heatherbooker/webpack-react-starter) — a complete environment to use React and Webpack, as well as SCSS.)  

We will be creating a project ‘anyProject’ in a directory ‘dev’. Feel free to adapt these to your needs.  

```bash
mkdir dev/anyProject
cd dev/anyProject
```

npm is a dandy package manager we will be using to set up webpack and react. Make sure you have npm installed by typing `npm -v` — check [here](https://docs.npmjs.com/getting-started/installing-node) or [here](http://blog.npmjs.org/post/85484771375/how-to-install-npm) for details.

Run `npm init -y` to get started — this will initialize your project with a package.json file. The -y option tells npm to use default settings; if you forget to use it, just press enter until you have gone through all of the questions. Running npm init -f accomplishes the same, but it also warns you:  

![npm being a know-it-all](/blog/img/npm-warn.png)

Alarming! Better stick to -y which gets the job done without sassing you. (not the css type…)  

Next, we install webpack:

```bash
npm i webpack -S
```

This should have initialized a node\_modules folder in your current directory (dev/anyProject, in this example). If not, webpack may have been downloaded to a node_modules folder found higher up in your directory tree. Webpack should now also be listed in the ‘dependencies’ section of your package.json. Let’s create a configuration file for webpack now, to tell it how to handle our files and what we want it do.

```bash
touch webpack.config.js
```  

Open webpack.config.js, and type the following:

```js
module.exports = {
  entry: “./src/index.jsx”,
  output: {
    path: ‘build’,
    filename: “index.js”
  }
};
```

This sets the first file webpack looks at as ‘index.jsx’ located in directory ‘src’, and writes the output to a file ‘index.js’ located in dir ‘build’. But how does our JSX become plain ol JS? Webpack uses ‘loaders’ to essentially translate code; we need to add loaders for whatever functionality we desire. First npm install some loaders: Babel will help the transition from JSX to js:

```bash
npm i babel-loader babel-preset-es2015 babel-preset-react -S
```  

Then go back to the webpack.config.js, and at the same level as ‘entry’ and ‘output’, add a key ‘module’ which will contain configuration information for our loaders, starting with babel:

```js
  ...
  , module: {
    loaders: [{
      test: /\.jsx$/,
      exclude: /node_modules/,
      loader: ‘babel’,
      query: {
        presets: [‘es2015’, ‘react’]
      }
    }]
  }
```

But where are our heads?! (not the git type…) We don’t even have React in our project yet! Better fix that. Run:  

```bash
npm i react react-dom -S
```

Let’s check that everything we’ve done so far is working. We will need directories ‘src’ and ‘build’, an index.jsx file and an index.html file.

```bash
mkdir src build
touch src/index.jsx
touch src/index.html
```

Hopefully you are using an editor like Sublime which will auto-generate a basic HTML doc by typing ‘html’ and pressing tab. Then throw in a quick <div> with an id=“app”, and a script tag so the index.html looks like this:

```html
<!DOCTYPE html>
<html>
<head>
 <title></title>
</head>
<body>
 <div id=”app”>
 </div>
<script type="text/javascript" src="index.js"></script>
</body>
</html>
```

Next we’ll create a quick component in index.jsx using React. Start by requiring in react and react-dom, then create a class and render a component:

```jsx
var React = require(‘react’);
var ReactDOM = require(‘react-dom’);
var AComponent = React.createClass ({
  render: function() {
    return <div>HELLO WORLD!</div>
  }
});
ReactDOM.render(
  <AComponent />,
  document.getElementById(‘app’)
);
```

(PS — you may want to install a plugin for your text editor that you can set to parse JSX files (ex, Babel).)
For now, we will need to manually copy our index.html into the build directory:

```bash
cp src/index.html build
```

Now all we have to do is run webpack! We will put a command into our package.json in the ‘scripts’, so that npm can help us run webpack (the ‘test’ script should have been auto-generated with the package.json file):

```js
"scripts": {
 "test": "echo \"Error: no test specified\" && exit 1",
 "build": "webpack"
 },
 ```

Now in your terminal/command line just type:

```bash
npm run build
```

And open dev/anyProject/build/index.html in your browser and you should see this:  

!['Hello world' in top left corner of your page](/blog/img/hello-world.png)


Awesome! It works! You have successfully set up a project with webpack and react! But you might be thinking : we copied that index.html file manually, that was stupid. You’re right! Let’s prepare this project a little more for the real world. We want to handle html files, and we might as well also use webpack for one of its great skills — requiring in all sorts of tidbits, such as images. I like using SVGs as they are highly editable, and resize well.  

Start by installing html-webpack-plugin and file-loader:  

```bash
npm i html-webpack-plugin file-loader -D
```

As their names suggest, the former is a plugin while the latter is a loader. To use the plugin, we require it at the top of the webpack.config.js:  

```js
var HtmlWebpackPlugin = require(‘html-webpack-plugin’);
```

Then after the loaders, we add a new section called plugins to the module.exports, and reference the HtmlWebpackPlugin var we just created. I like to instruct it to use a template, so that in the template I can have the one <div id=“app”> for React to inject content into:

```js
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: ‘./src/index.html’
    })
  ]
};
```

Scroll down to see how to set up the file loader (along with a SCSS loader).  

Now two last things: I like to be able to use SCSS , so we will set up a loader for that, and we can also set up React HMR for to serve our webpage and update it every time we make a change to a file. This is an excellent time-saver for development!  

```bash
npm i babel-preset-react-hmre sass-loader node-sass css-loader style-loader -D
```

To set up the file loader for images and the SCSS loader, add the following to the ‘loaders’ array in the webpack.config.js:

```js
}, {
    test: /\.scss$/,
    loaders: [“style”, “css”, “sass”]
   }, {
    test: /\.svg$/,
    loader: ‘file’
   }
}]
```

And finally, setting up hot module reloading is very easy. Simply add ‘react-hmre’ to the presets array in the babel loader:

```js
presets: [‘es2015’, ‘react’, ‘react-hmre’]
```

Then in package.json, add a new script which can be run with ‘npm start’:

```js
"start": "webpack-dev-server --progress --inline --hot"
```

Once you run ‘npm start’, you can open localhost:8080 to see your app change as you develop it. Voila, that’s it! Now you can write SCSS and JSX and use React to create awesome apps. Get to it!  

*Note*: This post was migrated from its [original home on Medium](https://medium.com/@heatherbooker/vue-js-vs-react-js-28caa8f9b033#.ojssrqrl3) on February 23, 2017.
