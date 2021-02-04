# Heather B-Log

Ma blog using [Eleventy](https://github.com/11ty/eleventy) static site generator and [eleventy base blog](https://github.com/11ty/eleventy-base-blog) with leonids styling (from Jekyll).

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
- add [accessible footnotes](https://hugogiraudel.com/2020/11/24/accessible-footnotes-and-a-bit-of-react/)
- change 'new' script to not add date to filename??
- add archives page
- add pagination
- link from old urls to new ones
- fix cartoon of my face in sidebar
- make tags page prettier
- make tags show on post pages (beside date)
