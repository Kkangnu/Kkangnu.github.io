---
layout: post
title: "Jekyll_튜토리얼(8)"
subtitle: "8. Blogging"
date: 2020-09-17 21:09:13 -0400
background: '/img/posts/06.jpg'
category: [Jekyll]
tag: [Jekyll]
---

## 8. Blogging

You might be wondering how you can have a blog without a database. In true Jekyll style, blogging is powered by text files only.

## Posts[Permalink](https://jekyllrb.com/docs/step-by-step/08-blogging/#posts)

Blog posts live in a folder called `_posts`. The filename for posts have a special format: the publish date, then a title, followed by an extension.

Create your first post at `_posts/2018-08-20-bananas.md` with the following content:

```
---
layout: post
author: jill
---
A banana is an edible fruit – botanically a berry – produced by several kinds
of large herbaceous flowering plants in the genus Musa.

In some countries, bananas used for cooking may be called "plantains",
distinguishing them from dessert bananas. The fruit is variable in size, color,
and firmness, but is usually elongated and curved, with soft flesh rich in
starch covered with a rind, which may be green, yellow, red, purple, or brown
when ripe.
```

This is like the `about.md` you created before except it has an author and a different layout. `author` is a custom variable, it’s not required and could have been named something like `creator`.

## Layout[Permalink](https://jekyllrb.com/docs/step-by-step/08-blogging/#layout)

The `post` layout doesn’t exist so you’ll need to create it at `_layouts/post.html` with the following content:

```
---
layout: default
---
<h1>{{ page.title }}</h1>
<p>{{ page.date | date_to_string }} - {{ page.author }}</p>

{{ content }}
```

This is an example of layout inheritance. The post layout outputs the title, date, author and content body which is wrapped by the default layout.

Also note the `date_to_string` filter, this formats a date into a nicer format.

## List posts[Permalink](https://jekyllrb.com/docs/step-by-step/08-blogging/#list-posts)

There’s currently no way to navigate to the blog post. Typically a blog has a page which lists all the posts, let’s do that next.

Jekyll makes posts available at `site.posts`.

Create `blog.html` in your root (`/blog.html`) with the following content:

```
---
layout: default
title: Blog
---
<h1>Latest Posts</h1>

<ul>
  {% for post in site.posts %}
    <li>
      <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
      {{ post.excerpt }}
    </li>
  {% endfor %}
</ul>
```

There’s a few things to note with this code:

- `post.url` is automatically set by Jekyll to the output path of the post
- `post.title` is pulled from the post filename and can be overridden by setting `title` in front matter
- `post.excerpt` is the first paragraph of content by default

You also need a way to navigate to this page through the main navigation. Open `_data/navigation.yml` and add an entry for the blog page:

```
- name: Home
  link: /
- name: About
  link: /about.html
- name: Blog
  link: /blog.html
```

## More posts[Permalink](https://jekyllrb.com/docs/step-by-step/08-blogging/#more-posts)

A blog isn’t very exciting with a single post. Add a few more:

`_posts/2018-08-21-apples.md`:

```
---
layout: post
author: jill
---
An apple is a sweet, edible fruit produced by an apple tree.

Apple trees are cultivated worldwide, and are the most widely grown species in
the genus Malus. The tree originated in Central Asia, where its wild ancestor,
Malus sieversii, is still found today. Apples have been grown for thousands of
years in Asia and Europe, and were brought to North America by European
colonists.
```

`_posts/2018-08-22-kiwifruit.md`:

```
---
layout: post
author: ted
---
Kiwifruit (often abbreviated as kiwi), or Chinese gooseberry is the edible
berry of several species of woody vines in the genus Actinidia.

The most common cultivar group of kiwifruit is oval, about the size of a large
hen's egg (5–8 cm (2.0–3.1 in) in length and 4.5–5.5 cm (1.8–2.2 in) in
diameter). It has a fibrous, dull greenish-brown skin and bright green or
golden flesh with rows of tiny, black, edible seeds. The fruit has a soft
texture, with a sweet and unique flavor.
```

Open [http://localhost:4000](http://localhost:4000/) and have a look through your blog posts.

Next we’ll focus on creating a page for each post author.