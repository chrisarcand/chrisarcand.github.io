---
layout: post
title: "Upgrading to OSX Yosemite"
modified: 2014-11-15 19:52:52 -0600
tags: [Misc]
image:
  feature: posts/yosemite-os-wallpaper.jpg
  credit: 
  creditlink: 
comments: 
share: 
---

After a bit of time to let the dust settle and important libraries and tools catch up to support OSX Yosemite,
I decided to give an upgrade a go. Here's some collected information about what it took to get my machine back up and running;
hopefully it will save you some time. This is mostly useful for Ruby developers. 

**Pre-installation warning**: Apple's upgrade process moves around your `/usr/local` directory across your disk - 
depending on what you have there, (namely, _Homebrew_ uses `/usr/local` and it has a _lot_ of small files). I didn't know
this ahead of time and it took many, many hours to upgrade. In hindsight, I would have probably just completely removed
Homebrew and started fresh afterward, at minimum.

\<Insert obligatory statement about how you should close this window and not upgrade without cloning your machine first...\>

* **Install Xcode**  
  I have found that being stubborn and refusing to do so only causes problems - even if you don't use Xcode directly - because Apple™.
  You can just install it via the App Store. 

* **Install Xcode Command Line Tools**  
  Separate step. You can `xcode-select —install` to trigger installation on the command line.

* **Fix Postgresql**  
  Postgres would not start properly at all using launchctl. This is because the Yosemite upgrade misplaces a few directories
  that Postgres uses when it's throwing files around your disk o_O  

  Source of this fix [here][1]

  ```bash
  cd /usr/local/var/postgres 
  mkdir pg_tblspc pg_twophase pg_stat_tmp
  ```

* **Fix Pow**  
  Yosemite initially broke Pow because [ipfw][2] has been completely removed. You’ll need to reinstall.
  Do not use the Homebrew version of Pow - it doesn’t work as of this writeup, it appears (still uses ipfw).

  Install the normal Pow way via `curl get.pow.cx | sh`

  Information about this issue [here][3]

* **Upgrade Powder**  
  If you use the [Powder][9] gem in your projects, you'll need to upgrade that to `>= 0.3.0` if you haven't already done so.
  I didn't keep the original error message but it's essentially 'Cannot do anything with Pow, try powder up?' any time you
  use a `powder` command.

  Yosemite is different enough where a separate loading process has been merged in - and yes, it kept the old code
  to be backwards compatible with Mavericks. You can see the commit that was merged [here][10]

* **Reinstall Brew Gems**  
  At Sport Ngin we use [brew-gem][4] for some tools to be available system-wide and easily updated. Running a command on
  any of these gems (such as [Opsicle][5] or [Octopolo][6]), you may get messages that look like this:

  ```
  Ignoring <gem> because its extensions are not built.  Try: gem pristine <gem>
  ```

  Simply reinstalling these gems solved the issue for me. (`brew gem uninstall <gem>; brew gem install <gem>`)

* **Fix Oh My Zsh PS1 prompt for RVM**  
  If you use zsh with Oh My Zsh, you may run into an issue with your PS1 prompt if you display the project path where
  it just shows `RVM_PROJECT_PATH`. 

  Oh My Zsh has already merged the fix; just `upgrade_oh_my_zsh` on the command line.

  Information about this issue [here][7]

And there you have it. Really, this wasn't much effort at all for my simple setup - mostly just updating things
to their Yosemite-friendly versions at this point. RVM appears to work perfectly fine; Existing Rubies seem good,
and I installed a new Ruby version with no issues. Homebrew itself also seems fine. I pretty much updated everything
anyway, even if it didn't appear broken.

Find other issues and/or solutions? Let me know and I'll add them to the list.

[1]: http://stackoverflow.com/questions/25970132/pg-tblspc-missing-after-installation-of-os-x-yosemite
[2]: https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/ipfw.8.html
[3]: https://github.com/basecamp/pow/issues/452
[4]: https://github.com/soupmatt/brew-gem
[5]: https://github.com/sportngin/opsicle
[6]: https://github.com/sportngin/octopolo
[7]: http://stackoverflow.com/questions/26369548/rvm-project-path-in-oh-my-zsh-prompt
[9]: https://github.com/Rodreegez/powder
[10]: https://github.com/rodreegez/powder/commit/1225959df689502eec9447816527d1818da8ef14
