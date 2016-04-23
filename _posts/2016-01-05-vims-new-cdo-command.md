---
layout: post
title: "Vim's new :cdo command"
tags: [Vim]
description: "Vim's new :cdo command offers new flexibility with your quickfix list. One such example is a near-native way to do project-wide find-and-replace operations."
---

**TLDR: The new `:cdo` in vim allows you to execute whatever command you wish on your quickfix entries.**

![A demo of substitution with :cdo](../images/posts/cdo_demo.gif)

[A few months ago with 7.4.858, Vim gained a new command: `:cdo`.][1] If you
use the quickfix window you might guess from the `:c<suffix>` and other
similar-looking commands like `:bufdo` and `:windo` that it allows you to run
any command you wish on the entries in your quickfix list.

...so?

This is awesome! A _lot_ of people use the quickfix window for things like the
edit-compile-edit cycle (what quickfix was originally designed for), text
searching with [ack.vim][3], or super quick TDD with [tpope's vim-dispatch][4].
There are uncountable things you can do with this.

For example, one thing I've always felt Vim lacked over modern/GUI text editors
was a simple, native way to perform project-wide find-and-replace operations.
Sure, there are _thousands_ of ways to do such a thing. In Vim itself, this
would involve weird bastardizations of things involving `:bufdo`, `:windo`,
`:tabdo` (and their `:next` commands)  to extra commands and plugins that
implement `:QFDo`. Piping around different magical spells of `grep`, `ack`
and/or `sed` is the usual way to do it on the command line.

But I wouldn't remember any command line solution for more than a week
(admittedly, I probably should have just created some sort of alias), and
certainly didn't feel like any Vim solution was anything more than a hack.
Yesterday marked yet another day that I looked up yet another way to accomplish
this. It has easily been #1 on my short list of things that vim doesn't provide
some better way to accomplish over more modern text editors (IMO, naturally).

_Most_ vimmers I know use [ack.vim][3]/[ag.vim][5]; combined with the new
`:cdo` command, an intuitive and near-native project-wide find-and-replace
solution is now available. To replace all instances of `foo` with `bar`:

```
:Ack foo
:cdo s/foo/bar/g | update
```

That's it. You're done.

In fact, `:cdo` isn't the only command that was added around this functionality:

* `:cdo[!] {cmd}`   - Execute {cmd} in each valid entry in the quickfix list.
* `:cfdo[!] {cmd}`  - Execute {cmd} in each file in the quickfix list.
* `:ld[o][!] {cmd}` - Execute {cmd} in each valid entry in the location list for the current window.
* `:lfdo[!] {cmd}`  - Execute {cmd} in each file in the location list for the current window.

Intuitive and simple, no [extra plugins or scripts.][2]

[1]: https://github.com/vim/vim/commit/aa23b379421aa214e6543b06c974594a25799b09
[2]: https://github.com/DouglasRoyds/vimrc/blob/master/plugin/qfdo.vim
[3]: https://github.com/mileszs/ack.vim
[4]: https://github.com/tpope/vim-dispatch
[5]: https://github.com/rking/ag.vim
