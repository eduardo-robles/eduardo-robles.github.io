---
layout: post
title: Fixing RSS on Velox Theme
date: '2014-07-14'
---

In the last [post](http://eduardo-robles.github.io/ghost-and-github-pages/index.html) I got Ghost and Github pages setup. It is working great but I quickly stumbled upon a tiny problem. There was no direct way to view my RSS feed.

So I dug around a bit and learned [how RSS works](http://www.ghostforbeginners.com/how-to-publish-a-rss-feed-like-feedburner-for-your-ghost-blog/) on Ghost blogs. Basically there is a some code on the default theme (casper) for Ghost that control the RSS functions. Here is what I found when I dug in the code

*on default.hbs, casper theme

~~~ HTML

<a class="subscribe icon-feed" href="{{@blog.url}}/rss/"><span class="tooltip">Subscribe!</span><div class="inner">

~~~

The important part in this code is...

~~~ HTML

<a class="subscribe icon-feed" href="{{@blog.url}}/rss/">

~~~

Pay close attention to the part `<strong>href="{{@blog.url}}/rss/</strong>`, this bit of code is what produces your blog's RSS feed. The hack is simple just add this code to your theme's default.hbs with any of your modifications.

My final code looked this..

~~~HTML

<h5 class="text book small"><a href="{{@blog.url}}/rss/" target="_blank" class="text bold">Subscribe!</a></h5>

~~~

I moved things around to match the Velox theme footer which is where I placed my RSS link (Subscribe!).

![RSS on Ghost Theme]({{site.url}}/assests/RSSonGhost.png)

Well I hope you found this tutorial helpful because I know RSS is essential for some users.
