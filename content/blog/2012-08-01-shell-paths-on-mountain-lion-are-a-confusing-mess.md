---
title: "Shell Paths on Mountain Lion Are a Confusing Mess"
date: 2012-08-01T00:00:00-06:00
slug: "shell-paths-on-mountain-lion-are-a-confusing-mess"

---

I am setting up an OS X 10.8 (Mountain Lion) machine as a development machine. For my own personal use I prefer using a Linux box, but between my own nostalgia for [Gentoo](http://www.gentoo.org/) and the peculiar yet undeniable strength of [homebrew](http://mxcl.github.com/homebrew/), I am perfectly happy to use it as a day-to-day development machine.

In order to use homebrew to its fullest, I edited the [/etc/paths](https://developer.apple.com/library/mac/#documentation/Darwin/Reference/Manpages/man8/path_helper.8.html) file to make things right:

```bash
/usr/local/sbin
/usr/local/bin
/usr/sbin
/usr/bin
/sbin
/bin
```

So far, no problems. Will report back later.
