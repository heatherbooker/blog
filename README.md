# Heather B-Log

Ma blog using [Eleventy](https://github.com/11ty/eleventy) static site generator and [eleventy base blog](https://github.com/11ty/eleventy-base-blog) with leonids styling (from Jekyll).

## How to build

```sh
npm start
```

## New post

`./new '<words to title your new post here>'` to generate new post

Or, add `new` to the path (same for `thought`):
```sh
# Pick somewhere in your path, I used /usr/local/bin
echo $PATH

# Symlink
ln -s ~/dev/blog/new /usr/local/bin/new
```
Then just run `new <look ma im blogging>` or whatever :)

## TODO

- PR upstream to https://prismjs.com/#languages-list for `sh` to join "`bash`, `shell`"
- change 'new' script to not add date to filename??
- make post urls be better (remove dates? at least don't have dates have slashes, that implies ability to see all of 2020/, but 2020- is better) (try https://gist.github.com/BrianMitchL/f93622a46f4476b7514995ff502d8d17 for redirect from previous urls)
- redirect from adventures-with-mozilla-con't to url without apostrophe
- fix hacky solution for linking between posts
- add linter: check for dead links, ensure httpS not http
