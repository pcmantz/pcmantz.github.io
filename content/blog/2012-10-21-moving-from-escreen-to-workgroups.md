---
title: "Moving From Escreen to Workgroups"
date: 2012-10-21T00:00:00-06:00
slug: "moving-from-escreen-to-workgroups"

---

I use Emacs to manage multiple different projects simultaneously. Every professional engagement I've had so far has had me working on multiple codebases at once. Add these to this my elisp and Org Mode files, and that's a lot of frames to have open!

My first attempt at managing multiple projects had the unintended consequence of holding me back from fully realizing the full potential of Emacs. I ran `emacsclient` sessions in GNU Screen for each project I worked on. I could flip between different frame configurations, each one set up for an individual project. This worked well enough that it kept me from running Emacs as a graphical interface.

Eventually, [Jon Rockway](http://jrock.us/) convinced me that I should abandon command-line emacs and use Emacs as it was intended; as a graphical application. However, I had developed a dependence on multiplexing in my workflow that was hard to break. I first tried [Elscreen](http://www.emacswiki.org/emacs/EmacsLispScreen), which allowed me to try multiplexing my projects natively in Emacs. However, the default configuration took up screen real estate and I didn't have the patience to try and remove it; so I quickly started to look for alternatives.

## Escreen

The next option I came across was [escreen](http://www.emacswiki.org/emacs/EmacsScreen). This was better suited to my workflow, and it used existing resources to manage workspace switching. Specifically, it embedded the window configurations into the buffer used to store frame configuration information, which I thought was ingenious. I was able to configure it so that my transitions between my constantly open GNU Screen terminal and Emacs was seamless. I would combine this with matching up the screen numbers per project, which allowed me to navigate my project transitions with very little mnemonic overhead.

Escreen is an excellent package, but still left a few features wanting. Specifically, I wanted:

- Named Workgroups - While matching up terminal numbers goes a long way, it would be much better if I could give my window configurations meaningful names.
- Persistence - I set my windows up in the same configuration and order, and reconfiguring emacs takes a while. The more I can automate that process, the better.
- Active Development - `escreen` hasn't seen any development in a few years, and I do not have the time and/or experience to round off the sharp edges.

## Workgroups

A bit of research led me to check out [workgroups](http://www.emacswiki.org/emacs/WorkgroupsForWindows). Puns aside, this is an excellent piece of software for managing window configurations. I was able to install the package from MELPA and configure it close enough to my `escreen` configuration so that I haven't been able to functionally notice the difference. Here is what I have so far:

```lisp
;; workgroups
(require 'workgroups)
(setq wg-prefix-key (kbd "C-z")
      wg-no-confirm t
      wg-file (concat elisp-dir "/workgroups")
      wg-use-faces nil
      wg-switch-on-load nil)

(defun wg-load-default ()
  "Run `wg-load' on `wg-file'."
  (interactive)
  (wg-load wg-file))

(defun wg-save-default ()
  "Run `wg-save' on `wg-file'."
  (interactive)
  (when wg-list
    (with-temp-message ""
      (wg-save wg-file))))

(define-key wg-map (kbd "g") 'wg-switch-to-workgroup)
(define-key wg-map (kbd "C-l") 'wg-load-default)
(define-key wg-map (kbd "C-s") 'wg-save-default)
(define-key wg-map (kbd "<backspace>") 'wg-switch-left)
(workgroups-mode 1)
(add-hook 'auto-save-hook 'wg-save-default)
(add-hook 'kill-emacs-hook 'wg-save-default)
```

Most of the code here was provided by this Stack Overflow [post](http://stackoverflow.com/questions/11263960/what-is-the-best-emacs-workspaces-plugin).

So what do I have now?

- I can now switch to workgroups by name! This makes it much easier for me to name projects and completely forget about matching up numbers. I can still match up numbers if I want, but why bother?

- I can restore my workgroup list after a restart. This makes it much easier to reboot and get back to where I left off.

- Re-ordering workgroups is easy and awesome. Using my configuration, `C-z C-,` and `C-z C-.` will move a workgroup left and right, respectively. This makes it much easier to arrange things in case something gets out of whack.

- All my key bindings are still the same. I don't have to worry about altering any of my muscle memory to accommodate for a new workflow; it is just the same plus more features.

## The Ugly Remains

`workgroups` is definitely a much more mature and polished product. Despite this, `escreen` was able to dodge a lot of rough edges and rely on Emacs's own built in abstractions by using the frame buffer to host workgroups that I am missing with `workgroups` so far:

- Per-workgroup switch-buffer lists - With `escreen`, each workgroup had its own list of visited buffers. This meant that my buffer visiting activities bled into each other less frequently. It appears that with `workgroup` that the visited buffer list is not localized to the workgroup, which is frustrating.

## Final Steps

Despite the progress, There are a few things left on my to-do list for `workgroups`.

- Persistent buffers - I haven't yet managed this. The only thing that gets saved and restored right now are my buffer names.

- Default new workgroups - I would like to have some default behavior when creating new windows. I name my windows after my projects; if I open a workgroup `foo` and there exists a directory `$HOME/git/foo`, I would like for the window to have a magit buffer there open to the project, waiting for me. If I could get this to work in conjunction with [eproject](https://github.com/jrockway/eproject) or [projectile](https://github.com/bbatsov/projectile), that would be icing on the cake.
