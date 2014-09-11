---
layout: post
title: Building a Landing Page
date: '2014-09-10'
---

For one of my class assignments I had to build a server. Either Windows or Linux and then host a webpage of some sort. Building a server was not a my main concern since I have had some prior experience with build them. The same idea applied with the webpage but I wanted to give myself a small challenge.

So I decided to build a _landing page_ using Twitters' open source CSS framework _Bootstrap_. I went to the [Bootstrap](https://getbootstrap.com) website and learned how build a landing page around the CSS framework. The steps were not hard and _Bootstrap_ is very customizable. So let's dig into what I did...

**Building a LAMP stack**

There are literally hundreds of resources online on how to build a LAMP stack so I won't dive into this. But I will provide a great tutorial over on [Digtal Ocean](https://www.digitalocean.com/community/tutorials/how-to-add-and-delete-users-on-ubuntu-12-04-and-centos-6) site which outline setting up an Ubuntu server with Apache, MySQL, and PHP. 

**Using Bootstrap**

To use Bootstrap you can either download the _zip_ file on https://getbootsrap.com or use _git_ to download it. There are several options you can use I went with the basic _zip_ file. The basic option will provide a basic layout of the default Bootstrap theme. 

To build my landing page I customized a specialized Bootstrap theme called "Cover". Unfourtantely, I could not find a direct download of the "Cover" theme on the Bootstrap site so I used _git/Github_ to download the files. 

An interesting thing about Bootstrap is that I uses [_Jekyll_](http://jekyllrb.com/)(a Ruby on Rails technology) to build and serve up webpages locally. So if you have _Jekyll_ installed like me you can simply execute the _Jekyll_ server to test you site locally like so...

`exec jekyll serve`

**Conclusions**