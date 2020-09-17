---
layout: post
title: "Jekyll_튜토리얼(3)"
subtitle: "3. Front Matter"
date: 2020-09-17 21:05:13 -0400
background: '/img/posts/06.jpg'
category: [Jekyll]
tag: [Jekyll]
---

# 3.  Front Matter
### Front matter is a snippet of YAML which sits between two triple-dashed lines at the top of a file. Front matter is used to set variables for the page, for example:

```
---
my_number: 5
---
```

### Front matter variables are available in Liquid under the page variable. For example to output the variable above you would use:
```
{{ page.my_number }}
```

## Use front matterPermalink
### Let’s change the <title> on your site to populate using front matter:


```
---
title: Home
---
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>{{ page.title }}</title>
  </head>
  <body>
    <h1>{{ "Hello World!" | downcase }}</h1>
  </body>
</html>
```

### Note that in order for Jekyll to process any liquid tags on your page, you must include front matter on it. The most minimal snippet of front matter you can include is:

```
---
---
```

### You may still be wondering why you’d output it this way as it takes more source code than raw HTML. In this next step, you’ll see why we’ve been doing this.
