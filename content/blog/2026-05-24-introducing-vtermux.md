---
title: "Introducing vtermux"
date: 2026-05-24T00:00:00-06:00
slug: "introducing-vtermux"
---

The terminal is still where most developers live. The UNIX philosophy of being able to shuffle data
from one application to another is essential when one's job is building things; reusable components
are a critical necessity. As such, when developers build tools, even when not thinking about
interoperability, the first class audience is the terminal. Now with LLMs making it almost trivial
to build decent UIs, terminal user interfaces (TUIs) are popping up to supplement command-line
interfaces (CLIs) that came from the first command prompts.

If you are an Emacs user, you might have a terminal open on another screen next to your full-screen
Emacs instance to handle the various commands you will need to enter, and this is a perfectly valid
workflow that many of us use. However, terminal emulation in emacs has gotten pretty good in the
past few years; and in particular `vterm` is amazingly performant. Unless you are doing something
that is pushing the boundaries of performance, `vterm` is able to handle your needs.

Unfortunately, the window management for `vterm` is pretty abysmal. Creating new windows gives you
`vterm <1>`, `vterm <2>`, `vterm <3>`, and so on, which isn't very helpful and has an obnoxious
cognitive overhead when working with multiple windows. The package `multi-vterm` helps by
associating one `vterm` instance per project directory, but in order to go beyond that, falling back
to `vterm` numbered buffers is still necessary.

I want to run multiple terminal buffers per project, and sometimes even multiple instances of the
same program in the same project, and be able to name them semantically. This isn't a huge ask, and
multiple packages do various parts of this, but not altogether. So that is why
[vtermux](https://github.com/pcmantz/vtermux) exists.

[vtermux](https://github.com/pcmantz/vtermux) allows you to specify any number of programs that you
can run individually in the context of a directory. The package exports one macro, `vtermux-define`,
which allows you to specify the programs you want in a straightforward way:

```elisp
(use-package vtermux
  :ensure
  (:host github :repo "pcmantz/vtermux")
  :after vterm
  :bind ("C-c v" . vtermux-run)
  :config

  ;; shells
  (vtermux-define bash)
  (vtermux-define zsh)

  ;; dev tools
  (vtermux-define pitchfork :args "tui")
  (vtermux-define claude)
  (vtermux-define opencode :args "-m")

  ;; ops tools
  (vtermux-define btop)
  (vtermux-define htop))

```

For claude, this gives you the following commands:

|Command |Description |
|--|--|
|claude|Launch claude.|
|claude-new| Create a new claude instance.|
|claude-next|Switch to the next claude buffer, skipping OFFSET buffers|
|claude-prev|Switch to the previous claude buffer, skipping OFFSET buffers|
|claude-select|Select a claude buffer with completing-read.|

For those following at home, this gives you a TUI or CLI interface to *any* existing program in
Emacs in one line.

So what can you do with this? Well, during development, I had OpenCode working on fixing bugs while
I had Claude do code review. You could even use two instances, as the `claude-new` command that gets
generated takes a label argument, so you would end up with `*claude - ~/git/vtermux/*` and
`*claude - ~/git/vtermux/* (review)`, which makes it easier to manage an ever-increasing amount of
tools in one place.

Rather than having to run every command manually, there is a built-in launcher `vtermux-run` which
can be bound from the `use-package` statement as seen above. so with the above configuration, you
get the following prompt:

```
vtermux b c h o p t z:
```

Which launches the corresponding program. If you already have a corresponding buffer for the project
(or directory, read the docs!), you will be asked for a label and you'll have both your programs
running happily together.

Please give [vtermux](https://github.com/pcmantz/vtermux) a try! I've been using it happily in my efforts to turn Emacs into an
LLM-powered IDE (article forthcoming...). I suspect this package can put to rest

Many thanks to the authors of the packages
[reformatter](https://github.com/purcell/emacs-reformatter),
[multi-term](https://www.emacswiki.org/emacs/MultiTerm "multi-term"),
[multi-vterm](https://github.com/suonlight/multi-vterm/), and
[vterm](https://github.com/suonlight/multi-vterm/) itself for all the inspiration and time they have
saved me by allowing me to cobble together an effective workspace to get things done. I hope this
helps others in the same fashion.
