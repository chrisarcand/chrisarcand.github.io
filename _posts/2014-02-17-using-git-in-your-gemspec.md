---
  layout: post
  title: "Using Git in Your Gemspec"
  tags: [Ruby,Git]
  description: "Over the past few weeks I've spent a lot of time working on a few of the modules we use here at Sport Ngin - mostly Ruby gems such as Opsicle, our OpsWorks CLI gem. As I've acquainted myself with architecting and testing gems locally, the use of git in gemspecs caught my attention."
---

This was originally written for [Coding in the Crease](http://www.codinginthecrease.com/). View the original
post [here.](http://www.codinginthecrease.com/news_article/show/350843)

Over the past few weeks I've spent a lot of time working on a few of the modules we use here at 
[Sport Ngin](http://www.sportngin.com) - mostly Ruby gems such as [Opsicle](https://github.com/sportngin/opsicle),
our OpsWorks CLI gem. As I've acquainted myself with architecting and testing gems locally, the use of 
[git](http://git-scm.com/) in gemspecs caught my attention.

### Gemspecs and 'git ls-files'

If you're unfamiliar with gems, a `.gemspec` file contains a Specification class with setter methods to assign various
bits of information on a gem. This class is then used to build the gem. One of the attributes, named `files`, contains
all of the files you wish to include in your build.

It's very common to see a shell command with git used for `files`. Usually some variation 
of `git ls-files` is used, which outputs a full list of your project's files.
If you use [Bundler](http://bundler.io/) to generate a new gem scaffold by running `bundle gem <gemname>`, you'll get a gemspec
file like this:

<script src="https://gist.github.com/ChrisArcand/8978372.js" width="400"></script>

### An issue you might git

Using `git ls-files` to get all of the files in the directory seemed pretty clever to me. 
Yet regardless of git being the de facto [VCS](http://en.wikipedia.org/wiki/Revision_control) of choice for most gems
([Here](https://github.com/rubygems/rubygems), [here](https://github.com/bundler/bundler),
[here](https://github.com/rails/rails), everywhere...), I still found it interesting that a shell command to an
external tool with its own configuration would be used. I shrugged my shoulders and continued working, until I ran
in to a small issue that took me a few moments to figure out:

**Using `git ls-files`, you get all of the files in the git index and the working tree (documentation,
[here](http://git-scm.com/docs/git-ls-files)). That means if you're playing around with a gem for use in another
project, you can't leave your musings in untracked files to test.**

For example, say you're tinkering with 'examplegem' and you add a new file:
`/examplegem/lib/examplegem/newfile.rb`. If you're using something like `gem "examplegem", :path => "../examplegem"`
in the Gemfile of a project in a sibling directory for local development, that file won't be included in the link.
If it's something really basic, you won't get any error for the functionality being missing when you try and test it.

You've got [at least] a few options:

1. **Add your files to the index**  
   Even if you don't plan on actually committing your `newfile.rb`, using `git add newfile.rb` will add it to the index
   and thus be included in your gemspec's files. 

2. **Add flags to your git command**  
   You could add `--deleted`, `--modified`, or `--others` (i.e. untracked files) to your git command in the gemspec,
   though this sort of solution seems hacky to me. 

3. **Stop using git for your gemspec's files**  
   You don't need it. 

   Instead of using git, you could use something along the lines of this:

   ```ruby
   spec.files = Dir.glob("{bin,lib}/**/*") + %w(LICENSE README.md)
   ```

   Or if you're looking for a well-known example, here's the gemspec from rails/actionpack:

   ```ruby
   spec.files = Dir['CHANGELOG.md', 'README.rdoc', 'MIT-LICENSE', 'lib/**/*']
   ```

### Using git affects your project

This subtle little side effect may seem insignificant, but I do like to know the
consequences of my decisions, even the minor ones. It's probably not often that you run in to issues with completely
unindexed files in your gem, using it in the situation I managed to find myself in. 

Still, this got me curious and I looked around at other projects and how the use of git may be a good or bad thing.
Take [Bundler's gemspec](https://github.com/bundler/bundler/blob/master/bundler.gemspec), where they have to append `/man/`
because they want it in the gem, but not under source control when you generate it locally:

<script src="https://gist.github.com/ChrisArcand/8985638.js" width="400"></script>

On the opposite end of the spectrum, check out [Vagrant's gemspec](https://github.com/mitchellh/vagrant/blob/master/vagrant.gemspec),
where Mitchell Hashimoto isn't afraid to put forth some effort to get exactly what he wants based on `.gitignore` without actually using git:

<script src="https://gist.github.com/ChrisArcand/8985651.js" width="400"></script>

### A more 'correct' solution?

As is the case with most things in development, the answer to the correctness of any of these methods is It Depends™.
*Do you care whether or not a bunch of extra files are in your gem?* *Do you want to meticulously pick out which files you
want in the gem?* For many projects this may be a fairly inconsequential issue, and you shouldn't spend a great deal of time thinking
about the perfect solution unless you are working on a very large project with specific needs. 

Using `git ls-files` is a fine default for Bundler to feed you when you want to throw together a gem quickly -
but no matter what you actually go with (I'm going with git, myself) considering these small details and their [minor] consequences
is important when trying to architect something well.
