---
layout: post
title: "Customize Category Names in Jekyll So-Simple Theme"
modified:
categories: blog
excerpt: "Let's start simple in the first blog post by discussing an interesting step in customizing the Jekyll so-simple theme for this website."
tags: [Jekyll, so-simple-theme]
image:
  feature:
date: 2015-09-06T14:06:00-5:00
---

It took me some time searching for my favoriate Jekyll theme for this website. I finally decided to use the so-simple-theme. I like its minimal embellishments and the capability of building a full-fledged personal website. Well, you may argue that it is not difficult to modify any existing themes to include everything you need, such as a blog, a portfolio, a personal bio and so on. But the so-simple-theme can be *very easily* adapted to serve all these purposes.

# Goal

The goal is to build a website, which contains a blog, a project list, and a publication list.

# What do we have initially?

After clone the repo from so-simple-theme, there will be a blog and a list of articles (plus many other things that we will not explore in this post).

# How to customize the category names?

The category names are shown in the navigation bar, the url, the title of the index pages. They are also used as the names for the directories that store the index page, and the posts for each category. Finally, category names are used to label each post in the corresponding post markdown file (`.md`). To add or modify a category name, we need to make sure all the places mentioned above are updated correctly.

# What exactly did I do?

In the first step, we start with the simplest task. Let's rename the *articles* in the original theme to *projects*. To do this, we need to:

* Rename the directory for the index page from "/articles" to "/projects";

* Edit the index.md file in the `/projects` directory (originally the `/articles` directory). This file defines how the index page appears for the category "projects". We need to (1) modify the title, which will be shown in the center of the index page, and (2) the code that enumerates the posts in the category.

  * To change the title, we need to locate the `title: Articles` in the index.md file and then replace it with `title: Projects` or any other title of your choice.

  * To enumerate the posts in the new category name, we need to modify a `for` loop code in index.md. In the original file, there is a line like this:

{% raw %}
{% for post in site.categories.articles %}
{% endraw %}

It loops through all posts in the articles category and show their titles and post dates in the articles index page. Now we need to change it to

{% raw %}
{% for post in site.categories.projects %}
{% endraw %}

so that the for loop will list projects instead of articles.

* Rename the directory `_post/articles` to `_post/projects`. This directory contains all the posts for the article/project category;

* Edit the posts (i.e. the .md files). Change the "categories" from "articles" to "projects";

* Edit navigation bar as defined in _data/navigation.yml. Find the following lines

{% raw %}
title: Articles
url: /articles/
{% endraw %}

and replace articles with projects as follows

{% raw %}
title: Projects
url: /projects/
{% endraw %}

OK. We can re-build the website now. We will see "PROJECTS" instead of "ARTICLES" in the navigation bar. Clicking it will direct us to a list of projects with title "Projects" shown in the center.

Now, let's try adding a new category.

The steps are almost the same as modifying the existing categories. Let"'"s say we want to add a new category called publications. The necessary steps are listed in the following.

* We first create a directory called "publications" and copy an index.md file from one of the existing categories (for example, `blog/index.md`). Then we change the title and the `for` loop just as we did when modifying the category names.

* Create a directory `_post/publications` to store the posts in the publication category. We may then create some `.md` files or just copy some posts from the other categories and then modify their content. Make sure the `category` in the `.md` files are set to the new category name `publications`.

* Finally, add an entry in the navigation bar by modiying the file `_data/navigation.yml`:

{% raw %}
title: Publications
url: /publications/
{% endraw %}

After these three major steps, we will see a working publication list. :D

There is one last thing I want to mention. There is no sub-category for the posts. Each post is characterized by a number of tags. A list of posts with each tag can be found in the directory `/tags`. You can add a link to this directory in the navigation bar. In this way, we will have a page allowing visitors to easily browse all the tags and the associated posts.

