---
layout: post
title: "Programming Easter Eggs"
tags: [Ruby, Programming]
modified: 04-05-2016
---

Perhaps these shouldn't be called Easter Eggs - 'programming novelties'? - but the little additions
in projects which don't necessarily need to exist are always satisfying and amusing to me.
There are a number of them that my colleagues or I have stumbled upon. Here's a little collection I curated.

The headers link to where the code exists in its project. Have any additions? [Let me know][2] and I'll add them to the list!

### [The 'forty_two' array accessor in Rails][3]

<script src="https://gist.github.com/chrisarcand/9da0dee4fab8d34173036b7416098744.js?file=rails_forty_two.rb"></script>
<noscript>{{ site.gist_js_warning | markdownify }}</noscript>

### [The '❨╯°□°❩╯︵┻━┻' method in Sidekiq][4]

I'm pretty certain this one is my favorite.

<script src="https://gist.github.com/chrisarcand/9da0dee4fab8d34173036b7416098744.js?file=sidekiq_table_flip.rb"></script>
<noscript>{{ site.gist_js_warning | markdownify }}</noscript>

### [The 'wtf!?!?' method in Pry][5]

<script src="https://gist.github.com/chrisarcand/9da0dee4fab8d34173036b7416098744.js?file=pry_wtf.rb"></script>
<noscript>{{ site.gist_js_warning | markdownify }}</noscript>

### ['nyan-cat', 'get-naked', and poems in Pry][9]

_From Enrico Genauck (@enricogenauck), Added 5/4/15_

The `pry` gem also includes a [dedicated file to Easter Eggs][9] with nyan cat
and a few text snippets from Jermaine Stewart, T.S. Eliot, and Leonard Cohen,
and Fernando Pessoa.

### The 'isUserAMonkey' function in the android.app.ActivityManager package

_From Ian Ehlert (@ehlertij)_

<script src="https://gist.github.com/chrisarcand/9da0dee4fab8d34173036b7416098744.js?file=android_isUserAMonkey.java"></script>
<noscript>{{ site.gist_js_warning | markdownify }}</noscript>

I'm not sure if this is all that strange considering the Monkey is a well documented, actual
command line tool to stress test your UI. But that's part of the much larger joke.

# Honorable Mentions

### [The 'fortnight' method in Rails][6]

<script src="https://gist.github.com/chrisarcand/9da0dee4fab8d34173036b7416098744.js?file=rails_fortnight.rb"></script>
<noscript>{{ site.gist_js_warning | markdownify }}</noscript>

I don't know if it was really intended to be a novelty or taken seriously, but I find it amusing.
And yes, I do actually try and use this - in tests, anyway.

### [The 'seppuku' command in RVM][7]

Wayne Seguin added this as an alias for `implode`. The change reads "Added 'rvm seppuku' in honor
of tsykoduk who can't spell so it saved his life."

<script src="https://gist.github.com/chrisarcand/9da0dee4fab8d34173036b7416098744.js?file=rvm_seppuku.bash"></script>
<noscript>{{ site.gist_js_warning | markdownify }}</noscript>

I especially enjoy the `rvm_log "Hai! Removing $rvm_path"`

### [The 'question' and 'answer' commands in RVM][8]

These commands seemed to have been removed since they were added in v0.1.30

<script src="https://gist.github.com/chrisarcand/9da0dee4fab8d34173036b7416098744.js?file=rvm_question_answer.bash"></script>
<noscript>{{ site.gist_js_warning | markdownify }}</noscript>

More entertaining is the bugfix for these commands that shortly followed:

> Bugfix: 'rvm answer' now uses perl, since the universe is written in Perl. As it is obvious... the universe has bugs, and perl is the only language that can have bugs not even god could sort out.

[1]: http://www.i-programmer.info/history/computer-languages/2340-coded-easter-eggs.html
[2]: http://www.twitter.com/chrisarcand
[3]: https://github.com/rails/rails/blob/3108e0809f36eb1afebd70211335435fdc600655/activesupport/lib/active_support/core_ext/array/access.rb#L70-L75
[4]: https://github.com/mperham/sidekiq/blob/cef8f2ec4db65032c49f3b5e704974fc90bffe6b/lib/sidekiq.rb#L48-L50
[5]: https://github.com/pry/pry/blob/f5c97cac103a9f04b854ed5b117a8cc27c7e8dd6/lib/pry/commands/wtf.rb
[6]: https://github.com/rails/rails/blob/3108e0809f36eb1afebd70211335435fdc600655/activesupport/lib/active_support/core_ext/numeric/time.rb#L58-L64
[7]: https://github.com/rvm/rvm/blob/93842ee4c23778af4e64e3436b8bb901b61acdb2/scripts/cli#L886-L889
[8]: https://github.com/rvm/rvm/blob/0a7a2e0c7001c191d17132569eb7499c619fd7f6/scripts/cli#L417-L418
[9]: https://github.com/pry/pry/blob/f5c97cac103a9f04b854ed5b117a8cc27c7e8dd6/lib/pry/commands/easter_eggs.rb
