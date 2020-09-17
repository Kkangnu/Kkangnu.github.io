---
layout: post
title: "Jekyll_QuickStart"
subtitle: "지킬 시작"
date: 2020-09-17 21:00:13 -0400
background: '/img/posts/06.jpg'
category: [Jekyll]
tag: [Jekyll]
---

# Quickstart
### Jekyll is a static site generator. It takes text written in your favorite markup language and uses layouts to create a static website. You can tweak the site’s look and feel, URLs, the data displayed on the page, and more.

# PrerequisitesPermalink
## Jekyll requires the following:

+ Ruby version 2.5.0 or higher
+ RubyGems
+ GCC and Make
## See Requirements for guides and details.

# InstructionsPermalink
1. Install all prerequisites.
2. Install the jekyll and bundler gems.
---
gem install jekyll bundler
---
3. Create a new Jekyll site at ./myblog.
---
jekyll new myblog
---
4. Change into your new directory.
---
cd myblog
---
5. Build the site and make it available on a local server.
---
bundle exec jekyll serve
---
6. Browse to http://localhost:4000
