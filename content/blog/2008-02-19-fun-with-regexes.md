---
title: "Fun With Regexes!"
date: 2008-02-19T00:00:00-06:00
slug: "fun-with-regexes"

---

A gem of a post came across my [alma mater](http://www.uchicago.edu)'s computer science mailing list: [Prime detection using regular expressions](http://http://www.noulakaz.net/weblog/2007/03/18/a-regular-expression-to-check-for-prime-numbers/). It's quite a clever hack; what it actually defines is all numbers that are *not* prime, but the negation of that creates the primes.

It's that pesky `/\1+/` that makes this whole expression work, and it's what prevents this regex from being a regular expression. Regexes have long since stopped being regular expressions, but it's a programming language unto itself.

A while back, I did LaTeX parsing in Perl, and used a one-level parser (custom for LaTeX) and a incredible amount of regexes in order so slice and dice this massive corpus of data. Unfortunately, I didn't know about Parse::RecDescent then, or else I would have put myself out of a job very quickly. But I wonder, can parentheses (a recursive language) be validated as a regular expression language yet? Better yet, is the language itself Turing complete? Perl regexes, at least, have a lot of very extended features, and this will require some unusual digging.
