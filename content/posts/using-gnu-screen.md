+++
title = "Using GNU Screen"
publishDate = 2021-12-18
draft = false
+++

## The Problem {#the-problem}

So it all started with a simple problem. _How to I connect to the console port on my Extreme Summit X440 switch?_ Luckily in the past I remembered that I could use `minicom`. It a great application to connect to console sessions. I used it way back in the day to connect to Cisco switches. If it wasn't for the fact that I had use an actual Cisco switch for a class I would have totally forgotten about it. But what does this have to do with `GNU Screen`?


## A Surprising Solution {#a-surprising-solution}

One afternoon I was surfing the web and came across a blerb of information that blew my mind. Screen can be used to connect to console sessions! I had recently been trying to redo my workflow to incorporate a terminal multiplexer. Most folks use TMUX and TMUX is a great choice. But at first glance the keybinding just seem weird and not very intituive for me. So went down the rabbit hole of Youtube videos on Screen vs. TMUX. In the end I decided to give Screen a try and see if it was really true that you can connect to serial console sessions.


## Screen and ttyUSB {#screen-and-ttyusb}

In Linux console cables interface with `/dev/ttyUSB` ([My console cable](https://www.amazon.com/OIKWAN-Compatible-Opengear-Aruba%EF%BC%8CJuniper-Switches/dp/B075V1RGQK/ref=sr%5F1%5F1%5Fsspa?crid=2MB6VVSMG5FAG&keywords=console%2Bcable&qid=1639881636&sprefix=console%2Bcabl%2Caps%2C190&sr=8-1-spons&spLa=ZW5jcnlwdGVkUXVhbGlmaWVyPUFIMExNREg5MUozSEcmZW5jcnlwdGVkSWQ9QTEwMjA3NzIzSEIwVllKTTBOM0JCJmVuY3J5cHRlZEFkSWQ9QTAwOTQxMzgzTEdHNTE3NktaWlVOJndpZGdldE5hbWU9c3BfYXRmJmFjdGlvbj1jbGlja1JlZGlyZWN0JmRvTm90TG9nQ2xpY2s9dHJ1ZQ&th=1)), this allows me to connect programs like minicom or screen to the serial connection. I did run into one small permissions issue when trying to run `screen /dev/ttyUSB0 9600`. Which led me to find out that I needed to add my user to the `dialout` group ([Arduino post](https://www.arduino.cc/en/guide/linux#toc6)). So I added my user like so `sudo usermod -a -G dialout myuser` and a quick reboot (a logout will work too) just to get things sorted. Once you log back in all you have to do is execute `screen /dev/ttyUSB0 9600` and you will get connected to your console session.


## Old tools to the job {#old-tools-to-the-job}

Sometimes old, tried, and true tools are the best. I'm glad I found out that GNU Screen can connect to console sessions. This allows me the flexibility of having a terminal multiplexer that is well rounded.


## Some useful links {#some-useful-links}

-   GNU Screen Manual: <https://www.gnu.org/software/screen/manual/screen.html>
-   Screen Baud Rate: <https://www.cyberciti.biz/faq/unix-linux-apple-osx-bsd-screen-set-baud-rate/>
