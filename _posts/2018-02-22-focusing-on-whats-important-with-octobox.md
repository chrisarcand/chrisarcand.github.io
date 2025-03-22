---
layout: post
title: "Focusing on what's important: Managing GitHub notifications with Octobox"
tags: [Tech]
description: "I wrote something for devproductivity.io about how I use Octobox to focus my attention on what's important while still keeping an eye across many different projects."
---

I wrote something for [devproductivity.io][8] about how I use Octobox to focus
my attention on what's important while still keeping an eye across many
different projects.

[Link: "Focusing on what's important: Managing GitHub notifications with Octobox"][9]

---

Many people wonder how some individuals can be so productive. It might seem that someone you know gets an especially copious amount of work done given the same amount of time as anyone else.  There's a lot of discussion to be had about what 'productive' even means and the best methods to go about being productive, but ultimately you can boil it all down to two statements:

* Spend your time on what's important
* Don't spend your time on what's not important

These points might seem obvious, but applying them to every aspect of your day is harder than you might think.

As developers we spend a lot of time looking at issues and reviewing code changes, interacting asynchronously with other people on multiple subjects over an indeterminate amount of time. While this is reasonable to manage via email notifications on small projects, as your organization and codebase grows your inbox quickly turns in to a water hose of GitHub notifications that all start to look the same. This problem is compounded if you're active in open source where you'd like to follow the activity of different projects outside of your own - and further compounded when the projects you're interested in are [very][1], [very][2] [large][3].

Spending too much time figuring out what notifications are actually important
in the current moment isn't productive. Trying to remember which notifications you actually need to follow with up over and over again is also not productive. Most open source maintainers and GitHub staff end up using a complex combination of filters and labels in Gmail to manage their notifications from their inbox. The productive thing to do, however, is to maximize your time actually addressing those issues!

Enter [Octobox][6].

![Octobox screenshot][4]

Octobox is an application that manages your GitHub notifications via GitHub's
V3 REST API, allowing you to filter down by organization, project, notification type, or the reason *why* you are receiving the notification in the first place.

With GitHub's notifications UI, when notifications are marked as read they
disappear from the list. This makes it very hard to keep on top of which
notifications you still need to follow up on. Octobox adds an extra "archived" state to each notification so you can mark it as "done". If new activity happens on the thread/issue/pull request, the next time you sync your notifications the relevant item will be unarchived and moved back into your inbox.

With Octobox it's easy to drill down in to the issues that matter to you in the current moment while still allowing you to be subscribed to projects that you'd like to keep an eye on. You can quickly address threads that you've authored yourself or remember that you still need to submit your review that someone requested. You can "star" long-running issues that you know might be important down the road. And anyone refusing to touch their mouse will be pleasantly surprised at the amount of keyboard shortcuts for all of these actions. ;)

Working full-time in open source, I have to keep track of a lot of different
projects - but I also try and maximize my productivity by paying attention to
what's important at a particular moment. Octobox is a tool I use to do just
that.

*Octobox is open source and [available on GitHub][6]. You can try it immediately at [octobox.io][7]*

[1]: https://github.com/ManageIQ/manageiq
[2]: https://github.com/torvalds/linux
[3]: https://github.com/rails/rails
[4]: http://screenshots.chrisarcand.com/permhc15o.jpg
[5]: https://twitter.com/teabass
[6]: https://github.com/octobox/octobox
[7]: https://octobox.io/
[8]: https://devproductivity.io
[9]: https://devproductivity.io/focusing-on-whats-important-with-octobox/
