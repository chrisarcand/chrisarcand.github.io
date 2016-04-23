---
layout: post
title: "Perl Compatible Regular Expressions in vim"
preview: "TLDR: If you don't want to have to deal with vim's non-Perl-like regular expressions in substitutions, you can easily enable Perl support and use :perldo <substitution> to get the job done."
tags: [Vim]
---

**TLDR:** *If you don't want to have to deal with vim's non-Perl-like regular expressions in substitutions, you can
easily enable Perl support and use `:perldo <substitution>` to get the job done.* 

Substitutions in vim are immensely powerful with regular expressions; I use them constantly. <MORE>

When I first began to use vim exclusively and saw the basic syntax of substitutions, I happily went about my way doing
simple things like `/s/cat/dog/` and `:%s/^\w+\s+/` with only needing my past regex experience. I went on like this
for over a month until I came across a need to have some really complex regex to heavily edit a very large file. I
pondered about it for a while, tapped out my expression, and hit return. An error message appeared. I double checked,
then triple checked my regex. I even went to [Rubular][1], a very helpful regex editor for Ruby (which treats regular
expressions 'as I knew them' closely enough) and it worked perfectly.

Having seen and used regular expressions a lot, I got frustrated for a bit until I quickly found the [commonly known, derp] reason:

**Vim uses its own implementation of regular expressions. Namely, the syntax is _not_ the same as PCRE (Perl Compatible
Regular Expressions), the style of regular expressions most commonly found across the industry since Perl in languages
such as PHP, Ruby, Java, Javascript, and Python.**

It should be noted that 'PCRE' is actually a C library written by Phillip Hazel and not a specificiation, but the
term is used interchangeably to mean 'Perl-like' regular expressions - that is, if a language uses PCRE it's said
to have regular expressions heavily influenced by Perl's implementation. It's the style of regex that I know and,
to my knowledge, the most widely used.

Well, crap. I really didn't feel like learning a whole new set of rules, having spent so much time learning and using
PCRE. While I embrace learning new things, I didn't feel like 'reinventing the wheel' on something such as this, especially
when knowing PCRE extremely well seems _more useful_ than spending any effort knowing how to convert back and forth for vim.

### Perl support in vim
So how do you use PCRE in vim? There's no configuration option for PCRE at all. [There are some incredibly complex scripts][2]
to convert PCRE to vimscript, but I really just wanted to be able to use PCRE more naturally. The answer is simple: just use Perl.

Although there are many added plugins you can install via something like Vundle or Pathogen, vim has support for multiple languages
including Lua, Python, Perl, and Ruby. To check to see if support is enabled for Perl, just use the `:ver` command. In the list of
features, you'll either see `+perl` or `-perl`, meaning that Perl is enabled or disabled respectively.

If Perl support is not enabled, you should be able to easily look up how to recompile vim for your platform. I use Homebrew with OSX,
so I just uninstalled vim and reinstalled it with the Perl support flag: `brew install vim --with-perl`

Once Perl support is enabled, you can now easily execute PCRE just as you would any other substitution with the `:perldo` command:

```
:perldo s/\A[a-f0-9]{8}-(?:[a-f0-9]{4}-){3}[a-f0-9]{12}\z/platypus/g
# replace all UUIDs with the word 'platypus'
```

Woo!

Note that if you really wanted, you could do this with something like, say, Ruby. Just install `--with-ruby` and you can suddenly
do the same thing using `gsub` if you want (seems pretty unnecessary to me). 

### Are vim's regular expressions really that different?
Arguably, the distinctions between PCRE and vim's regex are small - they have similar features and for most people it's probably just
learning a few little bits of syntax differences.
[There's even a special 'very magic' sequence you can use to makes expressions the same as the extended regular expressions used by egrep][3],
so you can have fairly normal-looking regex in native vim. You can use `:help perl-patterns` to see summary of some of the syntax difference between
the two styles.

There are some actual differences, though. Oleg Raisky and Steve Kirkendall sum up the main differences nicely:

> * Perl doesn't require backslashes before most of its operators. Personally, I think it makes regexps more readable - the less backlashes, the better.
> * Perl allows you to convert any quantifier into a non-greedy version by adding an extra ? after it.
> * Perl supports a lots of weird options that can be appended to the regexp, or even embedded in it.
> * You can also embed variable names in a Perl regular expression. Perl replaces the name with its value; this is called "variable interpolation".

### In summary
I'm very comfortable with PCRE, and when I'm trying to write out an extremely long or complex expression, I don't want to have to think about
how to translate my existing (and honestly, more valuable given PCRE's ubiquitous nature) knowledge into vimscript, even if it takes only minimal
effort to look up a few differing syntax rules. *While I still use native vim regex in my substitutions about 90% of the time*, if I've got
something complex I just use Perl support and call it good.

[1]: http://rubular.com/
[2]: https://github.com/othree/eregex.vim/blob/master/plugin/eregex.vim
[3]: http://vim.wikia.com/wiki/Simplifying_regular_expressions_using_magic_and_no-magic
