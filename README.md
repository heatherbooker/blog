## Heather's B-Log.

Uses [Jekyll](https://jekyllrb.com) with [leonids](https://github.com/renyuanz/leonids) theme.

## Notes to self:

`bundle exec jekyll serve` to preview

`./new '<words to title your new post here>'` to generate new post

Or, add `new` to the path (same for `thought`):
```sh
# Pick somewhere in your path, I used /usr/local/bin
echo $PATH

# Symlink
ln -s ~/dev/blog/new /usr/local/bin/new
```
Then just run `new <look ma im blogging>` or whatever :)

__Troubleshooting bundle whatever__

bundle install and enter your password as many times as it wants it
sudo gem pristine ffi --version XX.XX
