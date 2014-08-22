---
layout: post
title: Build a minimal blog with adequate features
category: web
tags: [blog, jekyll, poole, github pages]
---

I build this simple blog using **jekyll** and **poole**, and deploy it on **github pages**. This blog values simplicity but it contains adequate features like categories, tags and comments. I'd like share experiences with you on how to build it.

### Introduction
* [Jekyll](http://jekyllrb.com) is a lightweight static site generator, it parses your **Markdown**, **Liquid**(a html template language) and converts them into static HTML files. **Static** doesn't mean it can't be expressive or interactive, you need make good use of javascripts and CSS. Without the complicated server-side affairs, you can concentrate more on the **content**. Some useful dynamic features like comments and analytics can be equiped with the help of third-part plugins. Jekyll is blog-aware, so posts, pages, and custom layouts are all ready to use.
* [Poole](http://getpoole.com) is the butler for jekyll, it provides a clean and concise foundational setup for jekyll site. It's a good start point to learn jekyll. The pretty theme that comes with poole also attracts me a lot because the CSS affairs are not easy for me.
* [Github Pages](https://pages.github.com) supports jekyll. Hosting blog directly from github repository provides built in version control. It's so nice to edit posts via git workflows. Moreover, it's all free.

### Getting Started
To get started with my blog, I just create a new git repo with the name [mekswhy.github.io](http://mekswhy.github.io), clone the git repo [poole](https://github.com/poole/poole), and push it to my git repo. A couple minutes later (only for the first time), my blog is already public available!

```
git clone git@github.com:poole/poole.git
cd poole
git remote set-url origin git@github.com:mekswhy/mekswhy.github.io.git
git push origin master
```

