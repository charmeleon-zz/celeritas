celeritas
=========

A personal blog, powered by [Hugo](https://gohugo.io/).

Running
=========

To run, install `hugo` then clone this repo and generate the content with `hugo serve`

Configuration
=====
Since I forget how to configure this repo every time I move to a new machine.
After checking out this repo:
* `git submodule add git@github.com:charmeleon/charmeleon.github.io.git public`
* `cd public`
* `git pull`
* `cd ..`
* `hugo --buildFuture`
* `cd public && git add . && git commit -m "..." && git push`