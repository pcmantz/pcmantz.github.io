---
title: "Large Files in Emacs"
date: 2012-02-05T00:00:00-06:00
slug: "large-files-in-emacs"

---

I have started work on a new project where a lot of the source files are huge; files are commonly over 1,000 lines in length. While this is a massive [code smell](http://www.codinghorror.com/blog/2006/05/code-smells.html) that needs to be dealt with, it doesn't preclude the necessity of working with the file in the first place.

It appears that emacs has no problem navigating the files, but editing large files is a royal pain in the rear end. It appears that the only reasonable solution to shut off `'font-lock-mode`.

In addition, I have been using [outline-minor-mode](http://www.emacswiki.org/emacs/CPerlModeOutlineMode) to narrow the focus of my attention. Having a top-down view of the file which I am working with really helps.

I wonder how much hacking would be involved to make comments associated with their own header; or in the case of indented comments, with the level above them; as sort of a "floating" header level rather than a specific one.
