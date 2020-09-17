---
layout: post
title: "Jekyll_튜토리얼(6)"
subtitle: "6. Data Files"
date: 2020-09-17 21:08:13 -0400
background: '/img/posts/06.jpg'
category: [Jekyll]
tag: [Jekyll]
---

## 6. Data Files

Jekyll supports loading data from YAML, JSON, and CSV files located in a `_data` directory. Data files are a great way to separate content from source code to make the site easier to maintain.

In this step you’ll store the contents of the navigation in a data file and then iterate over it in the navigation include.

## Data file usage[Permalink](https://jekyllrb.com/docs/step-by-step/06-data-files/#data-file-usage)

[YAML](http://yaml.org/) is a format that’s common in the Ruby ecosystem. You’ll use it to store an array of navigation items each with a name and link.

Create a data file for the navigation at `_data/navigation.yml` with the following:

```
- name: Home
  link: /
- name: About
  link: /about.html
```

Jekyll makes this data file available to you at `site.data.navigation`. Instead of outputting each link in `_includes/navigation.html`, now you can iterate over the data file instead:

```
<nav>
  {% for item in site.data.navigation %}
    <a href="{{ item.link }}" {% if page.url == item.link %}style="color: red;"{% endif %}>
      {{ item.name }}
    </a>
  {% endfor %}
</nav>
```

The output will be exactly the same. The difference is you’ve made it easier to add new navigation items and change the HTML structure.

What good is a site without CSS, JS and images? Let’s look at how to handle assets in Jekyll.