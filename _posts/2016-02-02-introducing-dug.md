---
layout: post
title: "Introducing Dug"
tags: [Programming, GitHub, Ruby]
description: "Dug is a simple, configurable gem to organize your GitHub notification emails in ways Gmail can't and in an easier-to-maintain way than large, slow Google Apps Scripts."
---

Organizing GitHub notifications has always been annoying to me.

* Let's face it - the notifications screen in GitHub itself is fairly lacking and you never click on it regularly, so using the GitHub UI is out.
* Consequently, most people flock to Gmail and its label system. This sucks because GitHub doesn't allow you to subscribe to email on a per-project
basis.  Gmail filters are fairly powerful, and you can at least sort out the projects by the sender, the message body,
etc. But even that is still lacking as Gmail won't parse [GitHub's custom headers][3] that they include in emails
telling you _why_ you are receiving the notification.
* Even if you do make Gmail filters work out for you, maintaining them in Gmail's UI is painful, dumb experience.

### Google Apps Scripts
A while ago I stumbled on [a post][1] by [Lyzi Diamond][2] about managing GitHub notification messages in Gmail with
Google Apps scripts. The basic idea is that you can have a script written in [Google Apps Script][4] (Javascript,
essentially) and set a cron-like timer for Google Apps to run it against your Gmail Inbox.

Thanks Lyzi! This solution works well for [some people's workflows.][5] However, when I tried it there were a number of
unforgivable drawbacks in my case:

* Google Apps Scripts are _slow_. Like, really slow. Looking at the logs for my script, it took ~4
  seconds to add/remove a label. If you're someone who wants to have lots of notifications sorted, this is simply too
  long as the script times out and notifications coming in start piling up unprocessed.
* As a result of the previous point, even if you manage to page through and get all the messages processed Google has a
  hard cap of [1 hour of scripting time][6], and gave me a lovely `Service using too much computer time for one day.`
  message within the first day of my use of it.
* It's written in Javascript. ^\_^

In summation, labeling large quantities of GitHub notifications with all of the previous methods had too many weird,
annoying drawbacks and hacks that I'd never remember or want to maintain moving forward.

### The Goal

* I want organized, labeled notifications for all of the projects that I'm interested in on GitHub.
* I want it broken down by organization and repository so I can see what's going on where, and easily ignore what I want
  to at any time.
* I want notifications labeled according to my interaction with them: When I'm mentioned in a notification, when I'm
  assigned to something, when I'm generally participating...
* I want the management of these labels to be _stupid_ easy. Setting a new one up should be as simple as adding a label
  in Gmail and adding that label to an organized configuration file, at most.
* I want notifications that don't involve me directly to go straight to my archive, and notifications that do involve me
  to land right in my inbox with an unread notification popup.

### _[D]amn yo[u], [G]mail!_

So I wrote a gem, and now I have all I want.

![](http://screenshots.chrisarcand.com/permxqkc5.jpg)

Labeling is controlled by a simple YAML:

```yaml
---
organizations:
  - name: rails
    label: Rails
  - name: rspec
    label: RSpec
    repositories:
      - name: rspec-expectations
        label: RSpec/rspec-expectations
  - ManageIQ
reasons:
  author:
    label: Participating
  comment:
    label: Participating
  mention:
    label: Mentioned
  team_mention:
    label: Team mention
  assign:
    label: Assigned to me
```

And it needn't even be _this_ complex - this is just showing the ways you can specify whatever label names you want

It's called Dug, and it's [open source and available on GitHub.][7]

Dug is meant to be stupid simple. It depends on a single, stupid easy Gmail filter that you'll never have to change.
It's practically a gemified script with a configurable API, and as such you can programmatically configure and execute
Dug's runner class in any way you see fit (within a web app hook, a Rake task, a script, whatever). I personally use it
as an installed gem with a config/yaml file and cron job. That's it. It works marvelously. And it processes messages
near instantly.

Let me know what you think! Dug needs a few more goodies in the form of body parsing and some more built-ins (like
"Closed" Issues/PRs), and contributions are always welcome.

[1]: http://lyzidiamond.com/posts/github-notifications-google-script/
[2]: https://twitter.com/lyzidiamond
[3]: https://developer.github.com/v3/activity/notifications/#notification-reasons
[4]: https://en.wikipedia.org/wiki/Google_Apps_Script
[5]: https://boinkor.net/2016/01/better-filters-for-gmail-with-google-apps-scripts/
[6]: https://developers.google.com/apps-script/guides/services/quotas?hl=en
[7]: https://github.com/chrisarcand/dug
