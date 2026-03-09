---
title: "Iterators Make You Lazy"
date: 2012-06-24T00:00:00-06:00
slug: "iterators-make-you-lazy"

---

One of the most productive "A-Ha!" moments I've had as a developer came when I learned how to use iterators, and the dramatic effect that it has on my coding style. When we introduced the concept to one layer of code to handle a specific problem, the pattern of development proved infectious for dealing with similar workflows and managed to make our code much more succinct and understandable to newcomers.

At a past place of \$WORK, I worked on a project where we wanted to add a REST-ful API to our data, but we had to deal with blocking SQL queries whose completion took longer than any sane HTTP timeout should be. The result was that we needed to stream the formatted output of the query.

It turns out that most REST-type plugins for Catalyst don't handle streaming output, so we couldn't use something off the shelf. However, we did discover a module called [Iterator::Simple](https://metacpan.org/module/Iterator::Simple). This allowed us to construct on-the-fly formatters that we could then stream to the users. Now all of our code stopped looking like ugly while loops and temporary variables, and turned into this:

```perl
use Iterator::Simple qw(iflatten imap);

sub format_nodes {
    my ($nodes_rs) = @_;

    iflatten [
       '<nodes>',
        imap { format_node($_) } $nodes_rs,
        '</nodes>',
    ];
}
```

The fun didn't stop there. It started becoming clear that chaining and passing around iterators was a great way to quickly set up the construction of data sets, then pass those constructions along to consumers. A small library of macros had managed to invade our code and make it very, very lazy.

It turned out that many of our data preparation routines were expressible as computations that didn't need to live in memory. Anywhere we were running a loop and processing data, we could refactor that region with `imap` and a code block, and have the code execute on a per-needed basis.

The only one gripe that I have with this module is that its file handle emulation is incomplete. The module emulates the `getline` syntax; e.g.; the `&<>` operator, but not the method. Right now, [Catalyst::Engine::PSGI](https://metacpan.org/source/MIYAGAWA/Catalyst-Engine-PSGI-0.13/lib/Catalyst/Engine/PSGI.pm) assumes that the `getline` method is implemented on the resource to be streamed. This is true of file handles, but not of `Iterator::Simple::Iterator` objects. If the latter provided that interface, then this module would increase dramatically in utility. I have filed [a bug](https://rt.cpan.org/Public/Bug/Display.html?id=72629) and submitted a patch, so we'll see how it plays out.
