---
layout: post
title: "Jekyll_튜토리얼(2)"
subtitle: "2. Liquid"
date: 2020-09-17 21:03:13 -0400
background: '/img/posts/06.jpg'
category: [Jekyll]
tag: [Jekyll]
---

# 2. Liquid
### Liquid is where Jekyll starts to get more interesting. Liquid is a templating language which has three main parts: objects, tags and filters.

## Objects
### Objects tell Liquid where to output content. They’re denoted by double curly braces: 
```
{{ and }}
```
. For example:
```
{{ page.title }}
```

### Outputs a variable called page.title on the page.

## Tags
### Tags create the logic and control flow for templates. They are denoted by curly braces and percent signs: 
```
{% and %}
```
. For example:
```
{% if page.show_sidebar %}

  <div class="sidebar">
    sidebar content
  </div>

{% endif %}
```

### Outputs the sidebar if page.show_sidebar is true. You can learn more about the tags available to Jekyll here.

## Filters
### Filters change the output of a Liquid object. They are used within an output and are separated by a |. For example:

```
{{ "hi" | capitalize }}
```

### Outputs Hi. You can learn more about the filters available to Jekyll here.

## Use Liquid
### Now it’s your turn, change the Hello World! on your page to output as lowercase:

```
...
<h1>{{ "Hello World!" | downcase }}</h1>
...
```

### To get our changes processed by Jekyll we need to add front matter to the top of the page:

```
---
# front matter tells Jekyll to process Liquid
---
```



### Our “Hello World!” will now be downcased on render.

### It may not seem like it now, but much of Jekyll’s power comes from combining Liquid with other features.

### In order to see the changes from downcase Liquid filter, we will need to add front matter.

### That’s next. Let’s keep going.
