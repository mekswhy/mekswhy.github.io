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

<!-- more -->

### Getting Started
#### 1. Deploy on Github Pages.
To get started with my blog, I just create a new git repo with the name [mekswhy.github.io](http://mekswhy.github.io), clone the git repo [poole](https://github.com/poole/poole), and push it to my git repo. A couple minutes later (only for the first time), my blog is already public available!

```
git clone git@github.com:poole/poole.git
cd poole
git remote set-url origin git@github.com:mekswhy/mekswhy.github.io.git
git push origin master
```

#### 2. Directory Structure
This is the initial directory structure of poole:

```
$ tree poole
poole
├── 404.html
├── LICENSE.md
├── README.md
├── _config.yml
├── _includes
│   └── head.html
├── _layouts
│   ├── default.html
│   ├── page.html
│   └── post.html
├── _posts
│   ├── 2013-12-31-whats-jekyll.md
│   ├── 2014-01-01-example-content.md
│   └── 2014-01-02-introducing-poole.md
├── about.md
├── atom.xml
├── index.html
└── public
    ├── apple-touch-icon-precomposed.png
    ├── css
    │   ├── poole.css
    │   └── syntax.css
    └── favicon.ico

5 directories, 18 files
```

When you run jekyll, it creates a folder named `_site` which contains all the static web pages. Jekyll transfers all the files into `_site` unless the file name starts with an underscore. In this process, Markdown files will be converted to HTML files automatically and *liquid* will be handled properly. What about the folders or files start with an underscore like `_includes`? They are predefined by jekyll for special purpose like `_site`.

* `_includes` contains html snippets that can be reused among several pages.
* `_layouts` contains template files for layout.
* `_posts` contains your raw posts in Markdown format.
* `_config.yml` contains general configuration for the blog.

Customize your configurations first.

```
# Setup
title:            Mekswhy's Blog
tagline:          Be simple and stupid!
paginate:         5
baseurl:          /
author:
  name:           mekswhy
  email:          mekswhy@gmail.com
```

#### 3. YAML Front Matter
In each post file, a YAML front matter specifies several variables. These variables will then be available to you to access using *liquid* tags both further down in the file and also in any layouts or includes that the page or post in question relies on. The category and tags will be used next.

```
---
layout: post
title: Build a minimal blog with adequate features
category: web
tags: [blog, jekyll, poole, github pages]
---
```

### Improvements
#### 1. Navigation bar
Put several entry points in the `_layouts/default.html`.

``` html
<div class="masthead">
  <h3 class="masthead-title">
    <a href="/" title="Home">{{ site.title }}</a>
    <small>{{ site.tagline }}</small>
    <small><a href="/archive">Archive</a></small>
    <small><a href="/categories">Categories</a></small>
    <small><a href="/tags">Tags</a></small>
    <small><a href="/about">About</a></small>
    <small><a href="/atom.xml">Feed</a></small>
  </h3>
</div>
```

#### 2. Archive
Create a file named `archive.html` in root. This page shows a dynamic list of all posts through a *liquid* loop.

``` html
---
layout: page
title: Archive
---

{% raw %}
<ul>
{% for post in site.posts %}
    <li>
    <a href="{{ post.url }}"> {{ post.date | date_to_string }} &raquo; {{ post.title }}</a>
    </li>
{% endfor %}
</ul>
{% endraw %}
```

One question, how do I show this snippet finally without the *liquid* processing? Put `{{ "{% raw " }}%}` and `{{ "{% endraw " }}%}` around the snippet.

#### 3. Categories
Similar to `archive.html`, implement the `categories.html`. `site.categories` contains a list of [*category name*, *posts belong to this category*] pairs. Only categories with one level are supported here.

``` html
{% raw %}
{% for cat in site.categories %}
  <h3 id="{{ cat[0] | cgi_escape }}">{{ cat[0] }}</h3>
  <ul>
  {% for post in cat[1] %}
    <li>
    <a href="{{ post.url }}"> {{ post.date | date_to_string }} &raquo; {{ post.title }}</a>
    </li>
  {% endfor %}
  </ul>
{% endfor %}
{% endraw %}
```

#### 4. Tags
In the tags page, there should be all the tags in the front and then comes with posts list grouped by tags. I create a css class `tags` to specify a pretty style. 

``` css
.tags {
  padding: 4px;
  border: 1px dashed #ccc;
  border-radius: 4px;
  color: green;
  background-color: #eee;
}
```

`site.tags` contains a list of pairs similar to `site.categories`.

``` html
{% raw %}
<ul>
{% for tag in site.tags %}
  <a href="#{{ tag[0] | cgi_escape }}" class="tags">{{ tag[0] }}<sup>{{ tag[1].size }}</sup></a>
{% endfor %}
</ul>

{% for tag in site.tags %}
  <h3 id="{{ tag[0] | cgi_escape }}">{{ tag[0] }}</h3>
  <ul>
  {% for post in tag[1] %}
    <li>
    <a href="{{ post.url }}"> {{ post.date | date_to_string }} &raquo; {{ post.title }}</a>
    </li>
  {% endfor %}
  </ul>
{% endfor %}
{% endraw %}
```

After implementing categories and tags pages, don't forget to add the categories and tags links below each post title. So `_layouts/post.html` and `index.html` should be modified.

``` html
{% raw %}
<a href="/categories/#{{ page.category | cgi_escape }}">{{ page.category }}</a>
{% for tag in page.tags %}
  <a href="/tags/#{{ tag | cgi_escape }}" class="tags">{{ tag }}<sup>{{ site.tags[tag].size }}</sup></a>
{% endfor %}
{% endraw %}
```

#### 5. Index with preview
The origin index contains one post per page, and the content will be displayed entirely. I change the paginate to 5 in configuration and then add a filter to limit the post content. 

``` html
{% raw %}
{{ post.content | truncatewords: 200 }}
{% endraw %}
```

But this simple solution doesn't work well because the truncations may occur exactly between one html tag pairs thus produce broken pages. So add `<!-- more -->` mark in each post to determine the truncation point manually. And then modify the filter in `index.html` using `split` and `first`.

``` html
{% raw %}
{{ post.content | split:'<!-- more -->' | first }}
{% if post.content contains '<!-- more -->' %}
  <a href="{{ post.url }}">Read more</a>
{% endif %}
{% endraw %}
```

### Plugins
#### 1. Disqus
First you need sign up a [disqus](http://disqus.com) account and get an universal installation code. Save the code in a file named `comments.html` and put it in `_includes`. Only posts ought to receive comments, so include `comments.html` in `_layouts/post.html`. Once your posts page are load, this script will run in the browser and finally render a disqus comments widget on the bottom. All the comments will be saved in the diqus server, and they are load automatically.

``` html
{% raw %}
{% include comments.html %}
{% endraw %}
```

#### 2. Google Analytics
First you need enable the [Google Analytics](http://www.google.com/analytics/) and get the code provided by Google. Save the code in a file named `analytics.html` and put it in `_includes`. All the pages need analytics, so include `analytics.html` in `_layouts/default.html`.

``` html
{% raw %}
{% include analytics.html %}
{% endraw %}
```

#### 3. MathJax
We can display latex equations easily by courtesy of [MathJax](http://www.mathjax.org). Just add MathJax script in `_layouts/post.html`. 

``` html
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
```

In Markdown files, `\\( E=mc^2 \\)` will produce \\( E=mc^2 \\) and `\\[ E=mc^2 \\]` will produce \\[ E=mc^2 \\].
