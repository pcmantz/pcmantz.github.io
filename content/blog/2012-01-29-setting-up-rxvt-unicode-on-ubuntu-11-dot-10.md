---
title: "Setting Up Rxvt-unicode on Ubuntu 11.10"
date: 2012-01-29T00:00:00-06:00
slug: "setting-up-rxvt-unicode-on-ubuntu-11-dot-10"

---

I've been an Ubuntu & Mint user for the past few years. I am not particularly focused on usability or community, but it is nice using an operating system where most of the kinks have been ironed out for me by the efforts of others.

Lately, I decided to abandon [Terminator](http://www.tenshu.net/p/terminator.html) in favor of [rxvt-unicode](http://software.schmorp.de/pkg/rxvt-unicode.html), a.k.a. `urxvt`. Already I've found it to be a lot snappier, but installation was not simple. There are a few hoops to jump through yet.

## Install ncurses-term

Apparently the ubuntu packagers haven't gotten around to bundling the [terminfo](http://en.wikipedia.org/wiki/Terminfo) entry for rxvt-unicode into the package itself, as they probably don't think you need programs like `screen` or `tmux`. Rather, Ubuntu wants to to install yet another package, which has no related dependencies. be sure to install the ncurses-term package, which seems like a good idea anyways. And if you'd like to tell the Ubuntu packagers that they should fix this, you can go check out this [bug report](https://bugs.launchpad.net/ubuntu/+source/rxvt-unicode/+bug/772924).

## Installing rxvt-unicode

This part is relatively easy, just install the `rxvt-unicode` package and its dependencies.

## Setting up the environment

Configuring `rxvt-unicode` is done through the `~/.Xdefaults` configuration file, which is the historic location of all X11 application data. Since it is a grab-bag of a file, it is a little harder to find examples. However, I have munged through a few and posted my end product in [this gist](https://gist.github.com/1721879).

If you take nothing else from this configuration, *please* change the default colors. Your eyes will thank me for it, since the blue will be legible.

## Where From Here

By no means is this journey over. It turns out that my console is only supporting 88 colors, and that there are [others](http://blog.oldworld.fr/index.php?post/2010/03/21/256-colors-terminal-with-tmux-and-urxvt) who have had the same problem.
