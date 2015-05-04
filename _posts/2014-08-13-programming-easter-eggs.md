---
layout: post
title: "Programming Easter Eggs"
tags: [Ruby, Programming]
image:
  feature:
  credit: 
  creditlink: 
comments: 
share: 
---

Perhaps these shouldn't be called Easter Eggs - 'programming novelties'? - but the little additions 
in projects which don't necessarily need to exist are always satisfying and amusing to me. 
There are a number of them that my colleagues or I have stumbled upon. Have any additions,
especially non-Ruby ones? [Let me know][2] and I'll add them to the list. 

###[The 'fourty_two' array accessor in Rails][3]

  ```ruby
  # Equal to <tt>self[41]</tt>. Also known as accessing "the reddit".
  #
  #   (1..42).to_a.forty_two # => 42
  def forty_two
    self[41]
  end
  ```

###[The '❨╯°□°❩╯︵┻━┻' method in Sidekiq][4]

  ```ruby
  def self.❨╯°□°❩╯︵┻━┻
    puts "Calm down, bro"
  end

  describe "❨╯°□°❩╯︵┻━┻" do
    before { $stdout = StringIO.new }
    after  { $stdout = STDOUT }

    it "allows angry developers to express their emotional constitution and remedies it" do
      Sidekiq.❨╯°□°❩╯︵┻━┻
      assert_equal "Calm down, bro\n", $stdout.string
    end
  end
  ```
###[The 'wtf!?!?' method in Pry][5]

  ```ruby
  class Pry
    class Command::Wtf < Pry::ClassCommand
      match(/wtf([?!]*)/)
      group 'Context'
      description 'Show the backtrace of the most recent exception.'
      options :listing => 'wtf?'

      banner <<-'BANNER'
        Usage: wtf[?|!]

        Show's a few lines of the backtrace of the most recent exception (also available
        as `_ex_.backtrace`). If you want to see more lines, add more question marks or
        exclamation marks.

        wtf?
        wtf?!???!?!?

        # To see the entire backtrace, pass the `-v` or `--verbose` flag.
        wtf -v
      BANNER
    ...
  ```
  \* I would argue this _needs_ to exist.

###['nyan-cat', 'get-naked', and poems in Pry][9]
  
  _From Enrico Genauck (@enricogenauck), Added 5/4/15_

The `pry` gem also includes a [dedicated file to Easter Eggs][9] with nyan cat and
a few text snippets from Jermaine Stewart, T.S. Eliot, and Leonard Cohen, and Fernando Pessoa.

###The 'isUserAMonkey' function in the android.app.ActivityManager package
  
  _Submitted by Ian Ehlert_

  ```bash
  public static boolean isUserAMonkey ()   Since: API Level 8

  Returns "true" if the user interface is currently being messed with by a monkey.
  ```

  I'm not sure if this is all that strange considering the Monkey is a well documented, actual
  command line tool to stress test your UI. But that's part of the much larger joke.

# Honorable Mentions

###[The 'fortnight' method in Rails][6]

  I don't know if it was really intended to be a novelty or taken seriously, but I find it amusing.
  And yes, I do actually try and use this - in tests, anyway.

  ```ruby
  def fortnights
    ActiveSupport::Duration.new(self * 2.weeks, [[:days, self * 14]])
  end
  alias :fortnight :fortnights
  ```
###[The 'seppuku' command in RVM][7]

  Wayne Seguin added this as an alias for `implode`. The change reads "Added 'rvm seppuku' in honor
  of tsykoduk who can't spell so it saved his life."

  ```ruby
  implode|seppuku)
    source "$rvm_scripts_path/functions/implode"
    __rvm_implode
    ;;
  ```

  I especially enjoy the `rvm_log "Hai! Removing $rvm_path"`

###[The 'question' and 'answer' commands in RVM][8]

  These commands seemed to have been removed since they were added in v0.1.30

  ```ruby
  answer) __rvm_Answer_to_the_Ultimate_Question_of_Life_the_Universe_and_Everything ; result=42 ;;
  question) __rvm_ultimate_question ; result=42 ;;
  ```

  More entertaining is the bugfix for these commands that shortly followed:

  > Bugfix: 'rvm answer' now uses perl, since the universe is written in Perl. As it is obvious... the universe has bugs, and perl is the only language that can have bugs not even god could sort out.

[1]: http://www.i-programmer.info/history/computer-languages/2340-coded-easter-eggs.html
[2]: http://www.twitter.com/chrisarcand
[3]: https://github.com/rails/rails/blob/master/activesupport/lib/active_support/core_ext/array/access.rb#L57
[4]: https://github.com/mperham/sidekiq/blob/master/lib/sidekiq.rb#L31
[5]: https://github.com/pry/pry/blob/master/lib/pry/commands/wtf.rb
[6]: https://github.com/rails/rails/blob/master/activesupport/lib/active_support/core_ext/numeric/time.rb#L59
[7]: https://github.com/wayneeseguin/rvm/blob/master/scripts/cli#L873
[8]: https://github.com/wayneeseguin/rvm/blob/0.1.30/scripts/cli#L417
[9]: https://github.com/pry/pry/blob/master/lib/pry/commands/easter_eggs.rb
