---
title: "Perl Jails and You: Local::lib, Perlbrew, and Friends"
date: 2012-02-08T00:00:00-06:00
slug: "perl-jails-and-you-local-lib"

---

In the ruby world, many people are aware of the [ruby rvm](http://beginrescueend.com/) tool for ruby version management. Perl is not generally thought of as a virtual machine with an isolable environment, because of its frequent deep intertwining with the host operating system. However, there are tools that make this possible.

The problem with using Perl in this fashion is not that the solution does not occur to people, but that the solution occurs to *many* people simultaneously. Most of the hacker luminaries in the Perl world have invented a respective solution. [brian d foy](http://search.cpan.org/~bdfoy/) has written of his own [solution](http://stackoverflow.com/questions/398221/how-do-you-manage-perl-modules-when-using-a-package-manager/398397#398397), there is a [rubygems](https://metacpan.org/module/perlrocks) implementation in the wild, and the packaging problem has had multiple attempts made on it. Today, I'd like to review the best-in-class applications, and how to use them.

## local::lib

[local::lib](https://metacpan.org/module/local::lib) creates the the configuration configuration information for a perl package directory without affecting the system perl's library paths. Before worrying about your base perl installation, you should know `local::lib`. If you have a trustworthy sysadmin or are a junior dev on a team, this is most likely your situation.

`local::lib` lets you try install modules locally before installing them in a shared environment to experiment with new features, or install bleeding-edge versions of Perl-based command line apps like [App::GitGot](https://metacpan.org/module/App::GitGot), [App::Ack](https://metacpan.org/module/App::Ack) for personal use.

## perlbrew

Are you developing in an environment where you have to test against multipleo perl binaries? Then you should probably give [perlbrew](http://perlbrew.pl/) a try.

This application in a fully-featured suite for bootstrapping a perl environment. The perlbrew script allows you to install multiple versions of Perl and maintain library directories much more conveniently. In this regards, it is much more like rvm.

Another similar feature that perlbrew shares with rvm is its management of library assets. The perlbrew binary includes a command to install cpanminus, which hopefully you are using now already (If you are not, I will have to write another blog post!) This copy coordinates with the built-in lib functionality to install libaries to the current chosen perl, minimizing the fuss and clash and conflict.

## Combinations Thereof

If you are a solo developer who does not need to coordinate the usage of your development machine with other users, than *use perlbrew*. It provides the solutions of all the applications directly in a reproducible, scriptable manner.

If you are in a situation where setting up your own perl is a troublesome wreck, use `local::lib`. You will have a clean library path to experiment with, and you can feel no guild whatsoever when you muck up a dependency and need to blow it away.

Regardless of anything, use the right tool for the right job, and make sure you know that you are getting what you need. If you aren't, complain on [IRC](http://www.irc.perl.org/).
