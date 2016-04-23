---
layout: post
title: "Manually setting languages on GitHub with Linguist's (Improved) Vim Modeline Detection"
tags: [Vim, Git]
description: "Useful even for non-Vim users: You can use Vim modelines to manually set the programming language for a file on GitHub (syntax highlighting, stats) - now with more flexible modeline syntax."
---

[TL;DR. Show me the good stuff!][11]

As you may know, GitHub has had filetype specific syntax highlighting since [forever] and a fun little breakdown of
languages on a repository's home page [since 2012][2]. The latest incarnation of the latter looks like this:

![][1]

Languages are recognized for both of these features by GitHub's [Linguist][4], an open source Ruby gem [released in 2011
from GitHub's original highlighting lexers.][3] It works fairly well using file extensions, and common project layouts
(example: ignoring vendored file locations). 

However, for something with an uncommon layout or extension it's
understandably inaccurate. My personal dotfiles are a good example, where I designate a file to be automatically
symlinked to my home directory by the installer scripts by adding a `.symlink` extension. Here's my zshrc from a while
ago:

![][5]

### Manually setting languages for Linguist

The other day I discovered an interesting feature of Linguist added last year: you can manually set the reported
language in a file using Vim or Emacs modelines. To summarize for everyone, [Vim modelines][6] are magic comments that
set options for Vim for the particular file.

For example, I may decide that I want Vim to turn on autocorrect for a foreign language
in one file only. I can add a comment (with whatever syntax for a comment is necessary given the file type it is) in the
first or last few lines to set the spellcheck to German and wrap text at 120 characters with `vim: spell spelllang=de tw=120`.

Or, more realistically, I can set the syntax for a file with an awkward or very general file extension - like (gasp) the
`zshrc.symlink` example from above. With `# vim: syntax=zsh`, Linguist should recognize that it's a shell file, set the
correct syntax, and report it as such!

### No soup for you

...Except it didn't when I tried it. I found out that Linguist's parser is very restrictive as to the format of
it's acceptable modelines:

* In Vim there are two different ways to use a modeline: `vim: syntax=java` or `vim: set
  syntax=java:`. The latter is compatible with versions of Vi, Vim's predecessor. Linguist does not support the first, more
  common syntax.
* Linguist recognizes the Vim `filetype` option and not the Vim `syntax` option. They do different things in the editor.
  It's very common that people don't care about the `filetype` and only want the `syntax` to be correct, and so use that
  one instead.

<a name="tldr"></a>

### But you can haz all Vim modelines, now

I _love_ regular expressions, so I hunted down where Linguist does this modeline parsing and [submitted a patch that's
been accepted and merged.][8] When the [next version of Linguist is released][7] (current version: v4.7.4), you'll be
able to use all the various common ways of [identifying a file on GitHub with Vim modelines.][10]

From the updated Linguist README:
![][9]


[1]:  http://screenshots.chrisarcand.com/perm3co1k.jpg
[2]:  https://github.com/blog/1037-highlighting-repository-languages
[3]:  https://github.com/blog/881-linguist
[4]:  https://github.com/github/linguist
[5]:  http://screenshots.chrisarcand.com/perme3clf.jpg
[6]:  http://vimdoc.sourceforge.net/htmldoc/options.html#auto-setting
[7]:  https://github.com/github/linguist/releases
[8]:  https://github.com/github/linguist/pull/2812
[9]:  http://screenshots.chrisarcand.com/perml3utn.jpg
[10]: https://github.com/github/linguist#using-emacs-or-vim-modelines
[11]: #tldr
