---
layout: post
title:  "Getting Started with Hexo"
tags: [code, static site, blogging platform]
categories: [code]
share: discuss
---

[Hexo](http://www.hexo.io) is a cool new static site generator that seems to be solving some of the pain points I've faced with [Jekyll](http://jekyllrb.com/). However, I noticed the documentation is unclear (at least for me) on how to work with an existing Hexo site on GitHub. Here is what worked for me.

<span style='display: none;'><!--more--></span>

## How to get started

In Hexo, the source code can live in a seperate repo on GitHub from where the static site lives (if you're using GitHub Pages). To work on an existing Hexo site online, simply clone the source code repo:

```
  $ git clone --recursive https://github.com/anirudh-eka/my_hexo_site_source.git
  $ cd my_hexo_site_source
  $ npm install
```

We use the `--recursive` option here to pull down the 'themes' that the site uses, which are stored as Git submodules in the source repo. (More about this below.)

Now, run the server, to check it out locally!

```
  $ hexo serve
```

## Deploy

Assuming the [deployment configuration](https://hexo.io/docs/deployment.html) has already been set and you have push privelages to the live site, deploying is simple:

```
  $ hexo deploy
```

## Theme

Most of the styling of a Hexo site exists in the `themes` folder. This is a nice separation of data from styling, which allows you to swap themes pretty cleanly. 

### Add a theme

Themes in a Hexo site are [git submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules), which are basically git repos inside of a git repo. If there is a Hexo theme already in a remote repo, then all you have to do is:

```
	$ git submodule add [git repo url] themes/[name of theme]
```

If you want to create a new theme, then you must make a seperate theme project. [Check the Hexo docs](https://hexo.io/docs/themes.html) for more info on how to make a theme. After you push your new theme up to GitHub, add the theme by using the command line command above with the `git clone` url for your new theme on Github.

### Work on a theme

Often it's easiest to work on a custom theme when you are working within a Hexo site. Because of git submodules, this is possible! You can edit, add, do whatever in your theme and see what your site looks like as you make changes. Once you are ready to commit the changes and push. `cd` into the root of the theme, which would look like this if you were at the root of your site:

```
	$ cd themes/[name of theme]
``` 

Now, if you do a `git status` in this directory you will only see changes concerning the theme because you're in it's repo. You can:

```
	$ git add
	$ git commit -m "the changes I made in my theme are earth shattering"
```

 If you have privelages to push this repo:

```
 	$ git push
```
