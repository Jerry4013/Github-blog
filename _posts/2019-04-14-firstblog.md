---
layout: post
title:  "Welcome to Jekyll!"
tags: Jekyll
---

<br/><br/>

# Welcome

**Hello world**, this is my first Jekyll blog post.

I hope you like it!

I decided to start writing technical blogs from today because of two reasons:

1. Recording what I learned everyday in case I need to check or review them later on.
2. Provide useful information to others and save their time.

Regard to Jekyll installation, I wasted a large amount of time on template usage. At first, I read the official
document of Jekyll and followed tutorial step by step. However, that tutorial is for someone who wants to 
built a site from scratch! If you do not want to be an expert of Jekyll, you actually do not need to read that.

## You only need to do the following:
1. Create a repository on github.
2. Go to `Settings`, under `GitHub Pages`, click `change theme`
3. Choose a theme you like, and then `Select theme`
4. Clone this repository to your local directory
5. Modify the file index.md as you like! 

This index page will be rendered automatically by github. Next, I will explain how to 
modify the index file and add your own blogs.

## index.md
You must understand some knowledge about markdown language.
This file includes many useful default templates, but you may not want them displayed in your own page.
*   Create a new file in your project and move all the code into it except `layout: default`
*   If you want to display all your blogs by links in your index page, the following code might be helpful:

```text
  {% for post in site.posts %}

* [{{ post.title }}  ]({{ site.baseurl }}{{ post.url }})
 
  {% endfor %}
```

*   Create a new folder named `_posts` in your project
*   Create blogs like the name "2019-04-14-yourtitle.md". Follow this naming convention.

### Now, you can start to write your blog in this file!