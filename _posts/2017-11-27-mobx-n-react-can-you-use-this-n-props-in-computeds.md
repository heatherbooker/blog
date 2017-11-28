---
layout: post
title: "MobX + React: Can you use this.props in @computeds?"
categories: code
tags: code
---

I am an `@computed` evangelist when it comes to `get`ters in the many MobX-React components my team writes for work. I prowl around PRs, hunting for unsuspecting plain ol' innocent getters like this:

```js
@observer class Potato extends React.Component {
  @observable frend = 'Ms. Beans';

  get grewInTheGround() { return props.earthlyInfo.source === 'the ground'; }

  render() {
    return <div> look, i'm a paytaytoe { this.grewInTheGround && 'from the ground' }</div>;
  }
}
```

...and POuNCinG on them: "What if we used `@computed` for that grewInTheGround getter? Check out [the docs](https://mobx.js.org/refguide/computed-decorator.html)!"

This was a gleeful pastime of mine until one day I was rudely\* interrupted by a co-worker not so easily convinced. <!--more--> They mentioned that for a getter function containing solely MobX observed properties, they would of course also advocate for the use of computed, but _this_ getter relied on only React props from a parent component. They doubted the effectiveness of the decorator in the case where props could influence the getter.

Huh, I thought. (Well actually, first I thought HMPH!, but then I thought better of it and realized ah ha, a learning opportunity!) I hadn't considered the fact that props might not be supervised in the same way other MobX observables are.

Some background:
- Decorators are like, JS functions with `@` in front that just magically do stuff to whatever they're applied to. 
- Mobx is a state management library that is based on observables.

So I went straight to the MobX docs and checked if they mentioned props. I checked about computed, and observables, and observer, and React, and optimization, and common pitfalls, and best practices. Result: no. (Sidenote, I wrote the notes for this blog post nearly a month ago now so I am fuzzy on some details, like exactly how much the answer to my question was not contained in the docs. -_-)

Next, to google with variations on 'Can you use this.props in computed mobx'. I found exactly 0 anythings that were helpful. Eventually I migrated my efforts to [mobxjs/mobx](https://github.com/mobxjs/mobx) on Github and searched just 'computed props', then opened every issue that looked remotely relevant.

(In retrospect, I should have been looking for whether props are _observable_ and not whether they belong in @computeds, but this did not occur to me because changes in props clearly impacted the render function. Upon realizing this, I decided I had thought that because obviously React was managing the render function. This is also not true because that would totally at least 50% defeat the purpose of using MobX to optimize(minimize) renderings. Whatever.)

I started on one issue, "Discrepancy between re-computation between render and computed", and then found the gold mine [in one comment](https://github.com/mobxjs/mobx/issues/1075#issuecomment-312801921):

> Since 4.0 props are an observable...

Dammnnnnnnn! Ok to [the changelog](https://github.com/mobxjs/mobx-react/blob/master/CHANGELOG.md#thisprops-and-thisstate-in-react-components-are-now-observables-as-well) we go. A sub-header of v4.0 is entitled "this.props and this.state in React components are now observables as well" - bingo!! Well, there you go. The answer is yes, we should use @computed for getters that access props values as well as internal class observed values.

But we can't stop now, we're just getting started. They threw a little teaser into the changelog description there:

> For more details see [#136](https://github.com/mobxjs/mobx-react/pull/136)

And whoowee, there's a lot of chattin' in that thread. Also the whole description is "see [#124](https://github.com/mobxjs/mobx-react/issues/124)" - #124 is an issue containing discussion leading up to the making of this.props being observed.

At this point I was jumping around between all of these tabs, trying to make sense of it all and answer questions like "what is this HoC they keep mentioning?"(spoiler, higher-order-component) and "wait, so how deeply or shallowly is props observed?".

If we look again at the first example code above, where the computed accesses `this.props.earthlyInfo.source`, with pre-4.0 MobX, the computed would run if `this.props.earthlyInfo.source` was modified. Now, the computed also re-runs if `this.props.earthlyInfo` itself is changed, like if the component was rendered with new data: `{% raw %}<Potato earthlyInfo={{source: 'kindly neighbour'}} />{% endraw %}`. Good to know! You can see an example of how this was implemented in [this PR](https://github.com/mobxjs/mobx-react/pull/136/files/0bcf70bee3068a1b3df51b1969c28805625b59e5); it pretty much cycles through props and observes each one.

Pleasingly, I am now able to sleep at night and continue my `@computed` calling during the day, in full knowledge that I am bettering the efficiency of our code ever ever so slightly. ;)

Life is beautiful and birds are singing, as they say.

And hey, a lovely unexpected side effect of this knowledge slurping extravangza was that it recharged my interest in what I was doing at work, at a time when I felt very dulled by menial tasks and repetitve going-ons. It reminds me that sometimes it's important to take some time out of the grind in order to refresh my perspective and remember that Stuff Is Interesting! I really respect that my manager can't resist digging into the internals of everything they touch and I hope to keep channeling that. Props to them and to you and to everyone who doesn't just skim the surface but gets their hands dirty!



SHORT ANSWER IF YOU SKIMMED IN THE HOPES OF JUST FINDING THE FRICK OUT BECAUSE the docs are pretending to be all fancy n whatever but u got sTUFF to do: YA PROPS ARE OBSERVED U can use them in ur @computed :+1: .


\* Note: No offense to the speaker of this statement intended; 'rudely' is merely employed as a figure of speech. :)
