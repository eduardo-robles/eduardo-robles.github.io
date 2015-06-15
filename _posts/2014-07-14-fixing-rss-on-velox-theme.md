---
layout: post
title: Fixing RSS on Velox Theme
date: '2014-07-14'
---

<p>In the last <a href="http://eduardo-robles.github.io/ghost-and-github-pages/index.html">post</a> I got Ghost and Github pages setup. It is working great but I quickly stumbled upon a tiny problem. There was no direct way to view my RSS feed. </p>

<p>So I dug around a bit and learned <a href="http://www.ghostforbeginners.com/how-to-publish-a-rss-feed-like-feedburner-for-your-ghost-blog/">how RSS works</a> on Ghost blogs. Basically there is a some code on the default theme (casper) for Ghost that control the RSS functions. Here is what I found when I dug in the code</p>

<p>*on default.hbs, casper theme</p>

<p><code>&lt;a class="subscribe icon-feed" href="{{@blog.url}}/rss/"&gt;&lt;span class="tooltip"&gt;Subscribe!&lt;/span&gt;&lt;/a&gt; &lt;div class="inner"&gt;</code></p>

<p>The important part in this code is... </p>

<p><code>&lt;a class="subscribe icon-feed" href="{{@blog.url}}/rss/"&gt;</code></p>

<p>Pay close attention to the part '<strong>href="{{@blog.url}}/rss/</strong>", this bit of code is what produces your blog's RSS feed. The hack is simple just add this code to your theme's default.hbs with any of your modifications. </p>

<p>My final code looked this...</p>

<p><code>&lt;h5 class="text book small"&gt;&lt;a href="{{@blog.url}}/rss/" target="_blank" class="text bold"&gt;Subscribe!&lt;/a&gt;&lt;/h5&gt;</code></p>

<p>I moved things around to match the Velox theme footer which is where I placed my RSS link (Subscribe!). </p>

![RSS on Ghost Theme]({{site.url}}/assests/RSSonGhost.png)

<p>Well I hope you found this tutorial helpful because I know RSS is essential for some users.</p>
