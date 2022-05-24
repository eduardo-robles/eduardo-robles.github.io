+++
title = "256 Color In SSH Sessions"
date = 2022-01-07T23:00:00-06:00
draft = false
+++

I found myself going a bit crazy over theme rendering in my SSH sessions. Especially when I wanted to use `emacs -nw` in an SSH session. Recently I've been using GNU Screen as my terminal multiplexer and it comes with 256 color support. But you need to set it up and thanks to the Arch Wiki it's rather easy. All you have to do is put `term xterm-256color` somewhere in your `.screenrc` file. This tells your GNU Screen session to pull the correct colors based on what your `$TERM` supports.

An easy way to find out if you terminal emulator has 256 color support is by running `tput colors`, if `256` is your output then you have support! The main idea of ensuring that you get 256 colors working correctly is to make sure that you explicitly set it up. In other words if you use TMUX, be sure to let TMUX know to use 256 colors.

{{< figure src="/images/emacs-colors-gnuScreen.png" caption="<span class=\"figure-number\">Figure 1: </span>Showing 256 Colors in Emacs -nw inside of GNU Screen." >}}
