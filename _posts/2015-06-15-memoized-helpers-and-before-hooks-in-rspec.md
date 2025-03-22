---
layout: post
title: "Memoized helpers and before hooks in RSpec"
tags: [Ruby,Tech]
image:
  feature:
  credit:
  creditlink:
description: "A quick overview of the similarities and differences between memoized helpers and before hooks in RSpec."
---

In RSpec you can accomplish the same 'surface behavior' - which I define as behavior that results in the spec passing -
using any number of different combinations of memoized helpers (most commonly, `let` and `let!`) and before/after hooks.
Something set up in a block via `before` can be asserted the same as something set up in a memoized helper, for
instance.

When writing very simplistic spec it can mostly be a matter of semantics, but for more complex spec any
ambiguity of these constructs can lead a newer developer to some frustrating test failures (caused by the spec, not
the code the spec is testing) and a mountain of 'technical [spec] debt'. Yes. That's a thing. I've seen codebases with
really bloated and awful spec which made adding new tests difficult and time consuming.

So let's throw ambiguity out the window and take a look at the differences between them. The documentation for these
[memoized helpers][1] and [before hooks][2] isn't bad; but let's go over them quickly and then review a bunch of code
highlighting these differences.

It should be noted that these aren't specific to RSpec. Minitest's `setup` is pretty synonymous with `before`, for
example.  Also, Minitest can and does support the same sort of behavior with it's `MiniTest::Spec`, an RSpec-like module
that implements most of these concepts.

### before
`before` and its sibling `after` are RSpec's way of supporting common setup and teardown. It's simply a block of code
run before or after each example (by default `:example` is used; you can also specify `:context` or `:suite` instead). Instance
variables declared in `before(:example)` are accessible within each example group in the current example group (the block you
wrote using `context` or `describe`).

### let
`let` defines a [memoized][3] helper method. This means that the value of a call to this method will be cached across
multiple calls in an example (`it`, `example`). It's not cached across examples. `let` is [lazy-evaluated][4], which
means that the expression within the block is not evaluated until the defined method is called for the first time.

So, your `let` method is only evaluated if and when you call the method in your example and the value it returns is
cached and won't need to be computed again.

### let!
`let!` is the same as `let` except your method is automatically called in a `before` hook.

Using these three methods, you can create the 'similar surface behavior' I mentioned:

```ruby
describe "using let" do
  let(:a_thing) { code_that_creates_a_thing }

  before do
    a_thing
  end

  it "tests something here..."
end

describe "using let!" do
  let!(:a_thing) { code_that_creates_a_thing }

  it "tests something here..."
end

describe "using a before hook" do
  before do
    code_that_creates_a_thing
    # You can also assign the thing to an instance variable
  end

  it "tests something here..."
end
```

All three of the examples can assert the same sort of thing. So what _are_ the differences?

### 1. Execution order

An example of execution order:

```ruby
# Used for properly intended output
FOO = "    FOO"
BAR = "    BAR"

describe "before(:each) and memoized helpers" do
  context "before(:each)" do
    before(:each) do
      puts FOO
    end

    example "FOO appears before BAR" do
      # because it's called within a before hook
      puts BAR
    end
  end

  context "let (lazy-eval)" do
    let(:memo) do
      puts FOO
    end

    example "FOO never appears" do
      # because `memo` is never called in this example
      puts BAR
    end

    example "FOO appears after BAR" do
      # because I called it that way in this example
      puts BAR
      memo
    end

    example "FOO appears before BAR" do
      # because I called in that way in this example
      memo
      puts BAR
    end
  end

  context "let!" do
    let!(:memo) do
      puts FOO
    end

    example "FOO appears before BAR" do
      # because the memo gets run as a before hook with let!
      puts BAR
    end
  end

  context "let with let!" do
    let!(:memo) do
      another_memo
      puts FOO
    end

    let(:another_memo) do
      puts "    BAZ"
    end

    example "FOO appears after BAZ but before BAR" do
      # because `memo` calls `another_memo` in its block
      # and is called within a before hook
      puts BAR
    end
  end
end
```

```
Output with `--format documentation`:

before(:each) and memoized helpers
  before(:each)
    FOO
    BAR
    FOO appears before BAR
  let (lazy-eval)
    BAR
    FOO never appears
    BAR
    FOO
    FOO appears after BAR
    FOO
    BAR
    FOO appears before BAR
  let!
    FOO
    BAR
    FOO appears before BAR
  let with let!
    BAZ
    FOO
    BAR
    FOO appears after BAZ but before BAR
```

Note that the "before(:each)" and "let!" examples are essentially identical. That's because they are (in terms of call
order).

> Use `let!` to define a memoized helper method that is called in a `before` hook

The documentation is great, but it's something that seems to confuse people often. You can go spelunking in RSpec to prove it to yourself:

```ruby
def let!(name, &block)
  let(name, &block)
  before { __send__(name) }
end
```

### 2. Memoization and lazy-evaluation
We talked about this one already. The helpers have both of these things, a simple `before` block does not.

### 3. Overriding helpers in lower contexts

You can't 'cancel' or override a hook in a parent example group. You can, however, override (or reassign) a memoized helper.

```ruby
describe "Person#greet_chris" do
  subject(:greeting) { person.greet_chris }
  let(:person) { create(:person) }

  it "returns a default greeting" do
    expect(greeting).to eq "Hi, Chris!"
  end

  context "as a musical student of mine" do
    let(:person) { create(:person, type: 'student') }

    it "returns a more formal greeting" do
      expect(greeting).to eq "Hi, Mr. Arcand!"
    end

    context "who is scared because they didn't practice" do
      let(:person) { create(:person, type: 'student', hours_practiced_for_lesson: .25) }

      it "returns a terrified greeting" do
        expect(greeting).to eq "Hello Mr. Arcand. I brought you a burrito..."
      end
    end
  end
end
```

In this example I use [FactoryGirl][5] methods to demonstrate creating different types of `person` to `#greet_chris`.
I'm testing that my `person` object addresses me as I'd expect, given their type or mood - and I can easily change the
`person` for a different context and don't need to constantly rewrite what `greeting` is.

If you're wondering, for the purposes of this post `subject` works exactly the same as `let`.

## Conclusion

My examples here might be trivial but in real world practice it makes all the difference. You might be running end to
end tests that are expecting sample data to already be loaded in the database, and forget that your memoized dummy
creation is never actually run.  In doing Rails controller testing, I find that many people fall victim to not realizing
when exactly a request is made if conveniently DRY'd up in a helper - some tests make assertions on the response
(expectation after request is made) and some make assertions on an change of state (expectation before a request is
made). Confusion might ensue over how to efficiently override setup code in specific contexts because someone else
just threw a bunch of code in a top-level `before` block.

Having complete control over these constructs is crucial in writing clean and efficient tests. You'll discover that if
you take the time to put the same quality in your tests as in your code, future you and the other developers on your team
will really love you for it.

[1]: https://relishapp.com/rspec/rspec-core/v/3-3/docs/helper-methods/let-and-let
[2]: https://relishapp.com/rspec/rspec-core/v/3-3/docs/hooks/before-and-after-hooks
[3]: https://en.wikipedia.org/wiki/Memoization
[4]: https://en.wikipedia.org/?title=Lazy_evaluation
[5]: https://github.com/thoughtbot/factory_girl
