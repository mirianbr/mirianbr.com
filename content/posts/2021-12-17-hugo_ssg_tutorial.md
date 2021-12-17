+++
title = "Hugo Static Site Generator (SSG): a tutorial"
description = "A walkthrough on how I put my mirianbr.com website + blog online, using Hugo SSG with the Hello Friend NG theme"
type = "post"
tags = [
    "web-dev",
    "ssg",
    "hugo"
    "tutorials"
]
date = "2021-12-17"
[ author ]
  name = "Mírian Bruckschen Motta Barros"
+++

I have been asked these days about [Static Site Generators](https://jamstack.org/generators/) (SSGs), and at the moment I had to confess I had very little practical experience with those.  I *did* put my [previous blog](https://github.com/mirianbr/old-mirianbr.github.io) online using [Jekyll](https://jekyllrb.com/) with [Github Pages](https://pages.github.com/)' straightforward integration, and updated it always online, directly at Github UI, but it's something else to create everything locally and have full control on my hosting (something I do now, here at [mirianbr.com](https://mirianbr.com)).

So, this time I decided to try out Hugo after checking [so](https://jekyllrb.com/) [many](https://gohugo.io/) [interesting](https://www.gatsbyjs.com/) [options](https://vuepress.vuejs.org/) [available](https://blog.getpelican.com/).  It turned out to be easier than I imagined to have something simple yet very beautiful.

In this post, I'm documenting all the steps I took for myself and for everybody that wants to do something similar effortlessly.  Continue reading to check it out!

![Ready?](https://media.giphy.com/media/CjmvTCZf2U3p09Cn0h/giphy.gif)

## Basic setup

1. Download the [Hugo installer](https://github.com/gohugoio/hugo/releases) for your platform; I'm under Windows, and used the extended version after trying the regular one, because of this ["TOCSS … this feature is not available in your current Hugo version" issue](https://gohugo.io/troubleshooting/faq/#i-get-this-feature-is-not-available-in-your-current-hugo-version)
2. Create your site by entering in the command line `hugo new site <name of your site>` (in my case, I used `home` as my site name)
3. Go to your newly created `themes/` folder
3. Choose and install [a theme](https://themes.gohugo.io/) there; for [mirianbr.com](https://mirianbr.com), I used the minimalistic [Hello Friend NG](https://github.com/rhazdon/hugo-theme-hello-friend-ng)
4. Configure your site theme, author, taxonomies, your social networks to appear in the home page and more, in the `config.toml` file, that is created at the root folder of your site (you can see [my config file](https://github.com/mirianbr/mirianbr.com/blob/main/config.toml), and [the example config file](https://github.com/rhazdon/hugo-theme-hello-friend-ng/blob/master/exampleSite/config.toml) for the theme I'm using)

![Sofa... so far, so good?](https://media.giphy.com/media/lJAdW92OYGZlkuMCTW/giphy.gif)

## Site structure

Now, think about how you want your website to look.  In my case, I wanted three basic pages, besides the home: About, Blog and Projects.  This is configured in the `config.toml` file.  Here's a snippet of mine, that has this structure created:

```
[menu]
  [[menu.main]]
    identifier = "about"
    name       = "About"
    url        = "/about"
  [[menu.main]]
    identifier = "blog"
    name       = "Blog"
    url        = "/posts"
  [[menu.main]]
    identifier = "projects"
    name       = "Projects"
    url        = "/projects"
```

## Enter your content!

You now have everything you need to start creating your site pages.  Your content will be all created under the `content/` folder.  All static files (your images, media or javascript, for instance) should be stored under `static/`.

In my case, I have created two Markdown files under `content/`: `about.md` and `projects.md`.  The `posts/` folder contains the blog posts, one per file.  You can check them all in my [Github repo](https://github.com/mirianbr/mirianbr.com).

Using Hugo, you can create your new pages using the command `hugo new <filename.md>`, but you can simply create a new file and add the header to it, so Hugo knows how to interpret that content.  It can be as simple as the below, in my `about.md`:

```
---
title: "About"
date: 2021-12-12T15:48:31-03:00
draft: false
---
```

Or it can be a little more complicated, as in this blog post:

```
+++
title = "Hugo Static Site Generator (SSG): a tutorial"
description = "A walkthrough on how I put my mirianbr.com website + blog online, using Hugo SSG with the Hello Friend NG theme"
type = "post"
tags = [
    "web-dev",
    "ssg",
    "hugo"
    "tutorials"
]
date = "2021-12-17"
[ author ]
  name = "Mírian Bruckschen Motta Barros"
+++
```

## Test your site locally

Once you have something already, it's time to test your site locally.  You can do that by entering the command below in the command line:

```
hugo server -D
```

The `-D` is for draft.  It will serve pages you have still labeled as draft as well.

You will see the logs, that end in this message:

```
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```

So go ahead and test it out in your browser!

## Generate your site and publish it

Are you satisfied with what you see?  You can then generate your site to publish it externally. Just run the command line below in the root folder of your site:

```
hugo
```

And... that's it!  Hugo will create a folder named `public/` that has all your site pages.  It will create the HTML files for your Markdown pages.  If you have a blog and use tags (or categories, another form of taxonomy), it will create listing pages for these tags.

You can now send the files to your hosting.  Remember to set the [755 (executable) permissions](https://stackoverflow.com/a/21344137) to your page folder and files so people can actually view your site.  And you're done!

![Easy peasy!](https://media.giphy.com/media/3oKHWAk16aLhunLALm/giphy.gif)

Do you like this guide?  Remember something else that could help more people on this tutorial?  Or just want to share your own Hugo site?

Drop me a note at hello@mirianbr.com! 👋🏼  I'll love to read your thoughts! 😊