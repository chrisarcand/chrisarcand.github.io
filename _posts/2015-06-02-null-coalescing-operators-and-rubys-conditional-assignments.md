---
layout: post
title: "Null Coalescing Operators and Ruby's Conditional Assignments"
tags: [Ruby, Programming]
image:
  feature:
  credit:
  creditlink:
---

A long time ago in a galaxy far, far away (not really) I started learning and writing Ruby professionally by diving in
to an existing Rails project. Having the opportunity to learn in a 'trial by fire' sort of setting with other developers
more experienced with a language is a great way to pick something up very quickly. I remember the first time coming
across something similar to the following:

```ruby
def something
  @something ||= Something.new
end
```

This is probably the most basic and ubiquitous form of [memoization][1] in Ruby. In this case, I was told that with the combination of the
`||=` operator and Ruby's implicit `return` this means:

> Assign `@something` to a new Something object if it isn't already initialized and return it, otherwise return the preexisting value of `@something`

It's a simplistic answer but a sufficient one to tell a newcomer in the middle of a project. Having experience in other
languages, I thought to myself _Ah! Ruby's null coalescing operator!_ I wondered about it but it never really occurred
to me _why_ the syntax of such an operator was the double pipe (`||`). _Shrug_, move on.

After writing Ruby for a quite a while and having written various versions of this same pattern many, many times, I
was recently burned by `||=`. I had to very quickly come up with a toggle in a class and came up with this:

```ruby
def some_toggle
  @some_toggle ||= true
end
```

Note that not only is this a dumb use for using this memoization pattern (there are many ways to create such a toggle
with a default value of true - don't use this one that I rushed to), it also _doesn't work_. After a very short
investigation of the issue and actually taking a moment to rediscover this operator myself, I quickly realized my
(perhaps obvious) misunderstanding.

### True Null Coalescing Operators
A true [null coalescing operator][2] is "a binary operator that is part of the syntax for a basic conditional
expression" where you can both specify a value to be evaluated which is returned if not null and a value to be returned
if the first value _is_ null. That mouthful can be seen in this pseudo-code:

```
/* Given two arguments... */

if $first_argument is NOT NULL
  return $first_argument
else
  return $second_argument
```

'NOT NULL' could also just mean 'undefined'. Some programming languages don't have an implicit notion of NULL. Many
languages implement null coalescing operators. Here are just a few examples:

**Perl's version is `//`**

```perl
my $greeting = $more_personal_greeting // "Hello.";
```

**In C#, the operator is `??`**

```csharp
string greeting = morePersonalGreeting ?? "Hello.";
```

**Swift takes a cue from C# and also has `??`**

```swift
var greeting: String = morePersonalGreeting ?? "Hello."
```

**From PHP5 and on, you can leave out the middle part of a ternary operator to create a binary one.**

```php
// Use a full ternary expression...
$greeting = $more_personal_greeting ? $more_personal_greeting : "Hello.";

// Or omit the 'if true' portion
$greeting = $more_personal_greeting ?: "Hello.";
```

Apparently future versions of PHP might have a true `??` operator like C# and Swift.

**Groovy took PHP's thunder and actually made a separate operator out of `?:`, calling it the 'Elvis operator'. Look at it like an emoji. See Elvis's hair curl?**

```groovy
def greeting = morePersonalGreeting ?: "Hello."
```

Of course, all of these examples are null coalescing without implicit assignment; We're assigning `$greeting` based on
the existence of `$more_personal_greeting`. In Perl (at least) there's a variation on `//` that acts and looks even more like
Ruby's `||=` where assigning and checking `$greeting` is done implicitly:

```perl
my $greeting //= "Hello.";
```

`$greeting` is assigned to `$greeting` if it's defined (no change) or `"Hello."` otherwise.

### Ruby's Conditional Assignments

**Although it's often used like it, Ruby's `||=` is not a null coalescing operator but a [conditional assignment operator][4].**
In my quickly written Ruby example, `@some_toggle ||= true` _always_ returns true, even if `@some_toggle` was previously
assigned to `false`.

The reason is because this operator, 'Or Equals', coalesces `false` as well as `nil`. In Ruby anything that _isn't_ `false` or `nil` is truthy.
'OrEquals' is a shortcut for writing the following:

```ruby
# Generally
a || a = b

# Our example
@some_toggle || @some_toggle = true
```

Oh. So _that's_ why the syntax is 'double pipe equals', `||=`.  All this operator does is take Ruby's easy `||` logic
operator and combine it with the assignment operator `=`. In hindsight - after having a lot more experience with
Ruby logic and logical operators in general - it makes perfect sense. It's not wrong, it's just not a true null
coalescing assignment operator. Don't fall victim!

A Ruby version of what we're trying to do could look like this:

```ruby
def some_toggle
  defined?(@some_toggle) ? @some_toggle : true
end
```

Lastly, saying '`||=` is a conditional assignment operator' is only a subset of the larger
truth. The mix of various operators and assignment is simply referred to as [abbreviated assignment][3]. You've probably
seen this in the form of `+=` to increment a counter or add to a string. In fact, you can mix _any_ of the following
operators in the pattern `<operator>=`: `+, -, *, /, %, **, &, |, ^, <<, >>`.

[1]: http://en.wikipedia.org/wiki/Memoization
[2]: http://en.wikipedia.org/wiki/Null_coalescing_operator
[3]: http://ruby-doc.org/core-2.2.2/doc/syntax/assignment_rdoc.html#label-Abbreviated+Assignment
[4]: http://en.wikibooks.org/wiki/Ruby_Programming/Syntax/Operators#1._Assignment
