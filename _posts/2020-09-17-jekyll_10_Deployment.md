---
layout: post
title: "Jekyll_튜토리얼(10)"
subtitle: "10. Deployment"
date: 2020-09-17 21:11:13 -0400
background: '/img/posts/06.jpg'
category: [Jekyll]
tag: [Jekyll]
---

## 10. Deployment

In this final step we’ll get the site ready for production.

## Gemfile[Permalink](https://jekyllrb.com/docs/step-by-step/10-deployment/#gemfile)

It’s good practice to have a [Gemfile](https://jekyllrb.com/docs/ruby-101/#gemfile) for your site. This ensures the version of Jekyll and other gems remains consistent across different environments.

Create `Gemfile` in the root with the following:

```
source 'https://rubygems.org'

gem 'jekyll'
```

Then run `bundle install` in your terminal. This installs the gems and creates `Gemfile.lock` which locks the current gem versions for a future `bundle install`. If you ever want to update your gem versions you can run `bundle update`.

When using a `Gemfile`, you’ll run commands like `jekyll serve` with `bundle exec` prefixed. So the full command is:

```
bundle exec jekyll serve
```

This restricts your Ruby environment to only use gems set in your `Gemfile`.

## Plugins[Permalink](https://jekyllrb.com/docs/step-by-step/10-deployment/#plugins)

Jekyll plugins allow you to create custom generated content specific to your site. There’s many [plugins](https://jekyllrb.com/docs/plugins/) available or you can even write your own.

There are three official plugins which are useful on almost any Jekyll site:

- [jekyll-sitemap](https://github.com/jekyll/jekyll-sitemap) - Creates a sitemap file to help search engines index content
- [jekyll-feed](https://github.com/jekyll/jekyll-feed) - Creates an RSS feed for your posts
- [jekyll-seo-tag](https://github.com/jekyll/jekyll-seo-tag) - Adds meta tags to help with SEO

To use these first you need to add them to your `Gemfile`. If you put them in a `jekyll_plugins` group they’ll automatically be required into Jekyll:

```
source 'https://rubygems.org'

gem 'jekyll'

group :jekyll_plugins do
  gem 'jekyll-sitemap'
  gem 'jekyll-feed'
  gem 'jekyll-seo-tag'
end
```

Then add these lines to your `_config.yml`:

```
plugins:
  - jekyll-feed
  - jekyll-sitemap
  - jekyll-seo-tag
```

Now install them by running a `bundle update`.

`jekyll-sitemap` doesn’t need any setup, it will create your sitemap on build.

For `jekyll-feed` and `jekyll-seo-tag` you need to add tags to `_layouts/default.html`:

```
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>{{ page.title }}</title>
    <link rel="stylesheet" href="/assets/css/styles.css">
    {% feed_meta %}
    {% seo %}
  </head>
  <body>
    { % include navigation.html % }
    {{ content }}
  </body>
</html>
```

Restart your Jekyll server and check these tags are added to the `<head>`.

## Environments[Permalink](https://jekyllrb.com/docs/step-by-step/10-deployment/#environments)

Sometimes you might want to output something in production but not in development. Analytics scripts are the most common example of this.

To do this you can use [environments](https://jekyllrb.com/docs/configuration/environments/). You can set the environment by using the `JEKYLL_ENV` environment variable when running a command. For example:

```
JEKYLL_ENV=production bundle exec jekyll build
```

By default `JEKYLL_ENV` is development. The `JEKYLL_ENV` is available to you in liquid using `jekyll.environment`. So to only output the analytics script on production you would do the following:

```
{% if jekyll.environment == "production" %}
  <script src="my-analytics-script.js"></script>
{% endif %}
```

## Deployment[Permalink](https://jekyllrb.com/docs/step-by-step/10-deployment/#deployment)

The final step is to get the site onto a production server. The most basic way to do this is to run a production build:

```
JEKYLL_ENV=production bundle exec jekyll build
```

And copy the contents of `_site` to your server.

A better way is to automate this process using a [CI](https://jekyllrb.com/docs/deployment/automated/) or [3rd party](https://jekyllrb.com/docs/deployment/third-party/).

## Wrap up[Permalink](https://jekyllrb.com/docs/step-by-step/10-deployment/#wrap-up)

That brings us to the end of this step-by-step tutorial and the beginning of your Jekyll journey!

- Come say hi to the [community forums](https://talk.jekyllrb.com/)
- Help us make Jekyll better by [contributing](https://jekyllrb.com/docs/contributing/)
- Keep building Jekyll sites!
