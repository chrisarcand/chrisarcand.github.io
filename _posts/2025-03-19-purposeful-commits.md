---
layout: post
title: "Purposeful Commits"
tags: [Git]
description: "Git history shouldn't be just a chaotic diary of your development process—it's a communication tool that tells a story. Purposeful commits create a cleaner, more logical narrative that makes your code easier to review, understand, and maintain."
image: /images/posts/socialmediacards/purposeful_commits_card.png
---

Merge commits, squash commits, rebasing, requiring a linear history...how a team chooses - or does
_not_ choose - to manage changes to their codebase is an inevitable discussion at any software
company. I've had this conversation many times before for Git, so I thought it was time to write
down my own take on the subject. I'm no pedant, I don't enforce a strict policy doing this on things
I review, I mostly just quietly structure my own work this way and am thrilled when others do the same.

Look at most Git repositories and you'll find one of two problematic approaches to commit history:

On one end we have **merge commits**, which introduce a Git history on `main` with dozens of messy commits like "WIP",
"fix tests", and "oops forgot this file".

<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 800 300">
  <!-- Main branch -->
  <line x1="50" y1="60" x2="700" y2="60" stroke="#0366d6" stroke-width="3" />

  <!-- Feature branch -->
  <path d="M150,60 Q170,90 190,120 L600,120 Q620,90 650,60" fill="none" stroke="#28a745" stroke-width="3" />

  <!-- Commit dots on main branch -->
  <circle cx="100" cy="60" r="10" fill="#0366d6" />
  <circle cx="150" cy="60" r="10" fill="#0366d6" />
  <circle cx="250" cy="60" r="10" fill="#0366d6" />
  <circle cx="350" cy="60" r="10" fill="#0366d6" />
  <circle cx="550" cy="60" r="10" fill="#0366d6" />
  <circle cx="650" cy="60" r="13" fill="#6f42c1" stroke="#000" stroke-width="2" />

  <!-- Commit dots on feature branch -->
  <circle cx="190" cy="120" r="10" fill="#28a745" />
  <circle cx="270" cy="120" r="10" fill="#28a745" />
  <circle cx="350" cy="120" r="10" fill="#28a745" />
  <circle cx="430" cy="120" r="10" fill="#28a745" />
  <circle cx="510" cy="120" r="10" fill="#28a745" />
  <circle cx="600" cy="120" r="10" fill="#28a745" />

  <!-- Text labels for branches -->

<text x="80" y="35" font-family="Arial" font-size="14" font-weight="bold">main</text>
<text x="380" y="100" font-family="Arial" font-size="14" font-weight="bold">feature-branch</text>

  <!-- Vertically rotated commit messages for feature branch -->

<text x="190" y="140" font-family="Arial" font-size="12" text-anchor="start" transform="rotate(90, 190, 140)">"Initial commit"</text>
<text x="270" y="140" font-family="Arial" font-size="12" text-anchor="start" transform="rotate(90, 270, 140)">"WIP"</text>
<text x="350" y="140" font-family="Arial" font-size="12" text-anchor="start" transform="rotate(90, 350, 140)">"Fix failing tests"</text>
<text x="430" y="140" font-family="Arial" font-size="12" text-anchor="start" transform="rotate(90, 430, 140)">"Oops forgot this file"</text>
<text x="510" y="140" font-family="Arial" font-size="12" text-anchor="start" transform="rotate(90, 510, 140)">"More WIP"</text>
<text x="600" y="140" font-family="Arial" font-size="12" text-anchor="start" transform="rotate(90, 600, 140)">"Finally works"</text>

  <!-- GitHub merge commit message -->

<text x="650" y="140" font-family="Arial" font-size="12" text-anchor="start" transform="rotate(90, 650, 140)">"Merge pull request #41"</text>

  <!-- Connectors to the merge commit -->
  <path d="M600,120 Q620,90 650,60" fill="none" stroke="#6f42c1" stroke-width="2" stroke-dasharray="5,3" />
  <path d="M550,60 L650,60" stroke="#6f42c1" stroke-width="2" stroke-dasharray="5,3" />

  <!-- Annotations -->

<text x="650" y="40" font-family="Arial" font-size="12" fill="#6f42c1" font-style="italic">Merge point</text>
</svg>

While these accurately document someone's development journey - complete with mistakes and
backtracking - they create noise that makes it difficult to understand the actual changes later.
Scrolling through twenty commits to understand what should be a simple feature addition wastes
everyone's time. Seeing a change that never actually saw the light of day because it was changed
five minutes later in the same exact pull request is pointless. Finding regressions and reverting
them are annoyingly difficult, as the changes that appear to have been done long in the past are
actually just a few minutes old in a deployment - the dates and times on the commits don't matter,
they weren't included in the branch until the moment in time of the merge commit that they belong
to.

Sadly, because it's the easiest way to merge changes, and likely due to it being the default
strategy for GitHub PRs, this is the most common approach while also being the messiest and least
useful. In my experience, most of the time this method is used not intentionally but because of
indifference.

On the other end of the spectrum, we have the single **squash commit** approach - clean but lacking
detail. The changes from set of commits included in a pull request are 'squashed' to a single commit
to be actually merged to the main trunk, which discards valuable context about how the solution
evolved.

<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 800 300">
  <!-- Main branch -->
  <line x1="50" y1="60" x2="700" y2="60" stroke="#0366d6" stroke-width="3" />

  <!-- Feature branch commits (colored dots) -->
  <circle cx="190" cy="150" r="10" fill="#28a745" />
  <circle cx="260" cy="150" r="10" fill="#28a745" />
  <circle cx="330" cy="150" r="10" fill="#28a745" />
  <circle cx="400" cy="150" r="10" fill="#28a745" />
  <circle cx="470" cy="150" r="10" fill="#28a745" />
  <circle cx="540" cy="150" r="10" fill="#28a745" />

  <!-- Feature branch connect lines -->
  <line x1="190" y1="150" x2="260" y2="150" stroke="#28a745" stroke-width="2" />
  <line x1="260" y1="150" x2="330" y2="150" stroke="#28a745" stroke-width="2" />
  <line x1="330" y1="150" x2="400" y2="150" stroke="#28a745" stroke-width="2" />
  <line x1="400" y1="150" x2="470" y2="150" stroke="#28a745" stroke-width="2" />
  <line x1="470" y1="150" x2="540" y2="150" stroke="#28a745" stroke-width="2" />

  <!-- Main branch commits -->
  <circle cx="100" cy="60" r="10" fill="#0366d6" />
  <circle cx="170" cy="60" r="10" fill="#0366d6" />
  <circle cx="240" cy="60" r="10" fill="#0366d6" />
  <circle cx="310" cy="60" r="10" fill="#0366d6" />
  <circle cx="380" cy="60" r="10" fill="#0366d6" />
  <circle cx="520" cy="60" r="13" fill="#f97583" stroke="#000" stroke-width="2" />

  <!-- Branch labels -->

<text x="80" y="35" font-family="Arial" font-size="14" font-weight="bold">main</text>
<text x="360" y="130" font-family="Arial" font-size="14" font-weight="bold">feature work</text>

  <!-- Vertically rotated commit messages for feature branch -->

<text x="190" y="170" font-family="Arial" font-size="12" text-anchor="start" transform="rotate(90, 190, 170)">"Initial commit"</text>
<text x="260" y="170" font-family="Arial" font-size="12" text-anchor="start" transform="rotate(90, 260, 170)">"WIP"</text>
<text x="330" y="170" font-family="Arial" font-size="12" text-anchor="start" transform="rotate(90, 330, 170)">"Fix failing tests"</text>
<text x="400" y="170" font-family="Arial" font-size="12" text-anchor="start" transform="rotate(90, 400, 170)">"Oops forgot this file"</text>
<text x="470" y="170" font-family="Arial" font-size="12" text-anchor="start" transform="rotate(90, 470, 170)">"More WIP"</text>
<text x="540" y="170" font-family="Arial" font-size="12" text-anchor="start" transform="rotate(90, 540, 170)">"Finally works"</text>

  <!-- Squash commit message -->

<text x="520" y="40" font-family="Arial" font-size="12" text-anchor="start">"Add user authentication feature"</text>

  <!-- Squash triangle -->
  <path d="M190,150 L540,150 L520,60 Z" fill="#f97583" fill-opacity="0.15" stroke="#f97583" stroke-width="2" />

  <!-- Annotations -->

<text x="550" y="95" font-family="Arial" font-size="12" fill="#f97583" font-style="italic">Squash commit</text>
</svg>

Condensing six hours of work that included refactoring an interface to make it pluggable, changing
some test structure to make more sense to add to, and adding a new feature all in a single "Add user
authentication" commit tells future developers almost nothing about the decisions made along the
way.

While this approach is cleaner and certainly preferrable over merge commits, it's often _too_ clean.
Unless you are always merging the tiniest of changes at time, it's likely that you'll lose valuable
context about the purpose of individual decisions that supported the final overarching solution to
the problem at hand, unless authors are willing to walk through everything in the squash commit's
description.

What happens more often when folks 'use squash commits' is the worst of both worlds: With the
automatic ['Squash and Merge' function on
GitHub](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/incorporating-changes-from-a-pull-request/about-pull-request-merges#squash-and-merge-your-commits),
developers make 50 commits ala 'typo' and 'fix tests' for a pull request, then use the 'Squash and
Merge' button to combine them all into a single commit without amending the commit message for all
those changes. Git will take all the commit messages and combine them all together in the squash
commit, so a future developer looking for context on a change finds this very helpful commit message
explaining the changes:

```plaintext
commit 2b2bce0d228763880ba19cd089a166429ab5a118 (HEAD -> main, origin/main)
Author: Jonathan Smith <jon@jonsmith.com>
Date:   Thu Mar 13 16:27:25 2025 -0500

    change user.go to use new interface

    Update openapi/spec/schemas/my-feature/thing.yml

    Co-authored-by: Betty Sue <betty@sue.com>

    refactor #execute method

    Merge branch 'main' into jonsmith/user-authentication

    oops, typo

    fix test
```

Sigh.

Now, if folks are disciplined enough to rewrite the squash commit message at merge time, **squash
commits are still pretty agreeable** and add very, very little overhead for folks, so squash commits
are the minimum thing I usually actively advocate for. Great! Do that.

Ideally, I prefer something a little different that requires a bit more rigor, a middle ground
between merge commits and squash commits. I've always just considered it 'the right way to do it',
as I emulated it from mentors early in my career. Maybe there's a term for it I've never heard; the closest I've found are '[atomic commits](https://en.wikipedia.org/wiki/Atomic_commit#Atomic_commit_convention)', but it's not quite right.

So I guess I'll come up with a term for it.

## Purposeful Commits

Purposeful commits are a way of organizing your Git history to tell a clear, coherent story of how a
codebase is evolving. Each commit should represent a logical step in the progression of your work,
building on the previous one to achieve a specific goal.

> "For each desired change, make the change easy (warning: this may be hard), then make the easy change." --Kent Beck

The quote above from Kent Beck is easily my favorite quote about software. It's amazing how often
it's applicable, and it's not by coincidence that my preference for how to organize commits in Git
history is really just an embrace of this idea. When I work on a feature or bug fix, I structure my
commits to showcase this iterative approach to changes.

**Purposeful commits are about breaking down a task into smaller, more manageable steps, each with a
clear purpose:**

1. **The first commit might refactor existing code to make it more amenable to change**
2. **The second commit introduces the actual feature I wanted to add, which is now super easy to see based on the refactor in (1)**

Imagine you're extending your authentication logic beyond the basic password you have today, to accept Google OAuth as well.
Good software engineering practice would be to (1) refactor things to allow for a pluggable strategy first, then (2) add the new strategy.

Instead of one massive commit with both intentions wrapped together, or twenty disorganized ones, your PR might contain a few _purposeful_ commits:

#### **Commit 1**: "Making the change easy..."

```diff
commit 8d7f2c1e4b5a9c8d7f1e2b3a4c5d6e7f8a9b0c1d
Author: Developer <dev@example.com>
Date:   Wed Mar 19 09:47:18 2025 -0700

    refactor: Refactor user model to accept multiple auth providers

diff --git a/auth/authentication.go b/auth/authentication.go
index 6e2731a..f9a8b21 100644
--- a/auth/authentication.go
+++ b/auth/authentication.go
@@ -1,9 +1,19 @@
- func Authenticate(email, password string) (*User, error) {
-   var user User
-   // Find user by email...
-   if !ValidatePassword(password, user.PasswordHash) {
-     return nil, ErrInvalidCredentials
-   }
-   return &user, nil
- }
+ type Authenticator interface {
+   Authenticate(credentials map[string]string) (*User, error)
+ }
+
+ var authenticators = map[string]Authenticator{
+   "local": &LocalAuthenticator{},
+ }
+
+ func Authenticate(provider string, credentials map[string]string) (*User, error) {
+   authenticator, exists := authenticators[provider]
+   if !exists {
+     return nil, ErrUnsupportedProvider
+   }
+   return authenticator.Authenticate(credentials)
+ }

diff --git a/auth/authenticators/local.go b/auth/authenticators/local.go
new file mode 100644
index 0000000..a2b5079
--- /dev/null
+++ b/auth/authenticators/local.go
@@ -0,0 +1,6 @@
+ // LocalAuthenticator handles password-based authentication
+ type LocalAuthenticator struct{}
+
+ func (a *LocalAuthenticator) Authenticate(credentials map[string]string) (*User, error) {
+   // Implementation of password validation
+ }
```

#### **Commit 2**: "Making the easy change"

```diff
commit 2fd8901ab45c38e7b2c5d7f23cfb98d21e4fa3b2
Author: Developer <dev@example.com>
Date:   Wed Mar 19 10:12:45 2025 -0700

    feat: Google OAuth authentication backend

diff --git a/auth/authenticators/google.go b/auth/authenticators/google.go
new file mode 100644
index 0000000..a2b5079
--- /dev/null
+++ b/auth/authenticators/google.go
@@ -0,0 +1,6 @@
+ // GoogleAuthenticator handles OAuth authentication with Google
+ type GoogleAuthenticator struct{}
+
+ func (a *GoogleAuthenticator) Authenticate(credentials map[string]string) (*User, error) {
+   // Implementation of Google OAuth validation
+ }

diff --git a/auth/authentication.go b/auth/authentication.go
index f9a8b21..3e57f91 100644
--- a/auth/authentication.go
+++ b/auth/authentication.go
@@ -2,6 +2,7 @@

 var authenticators = map[string]Authenticator{
   "local": &LocalAuthenticator{},
+  "google": &GoogleAuthenticator{},
 }
```

#### **Commit 3** Isolate an impactful broader change with more context

```diff
commit d7bf6e05c8a93f2e1a9d4b7c6e5d4c3b2a1f0e9d
Author: Developer <dev@example.com>
Date:   Wed Mar 19 10:21:36 2025 -0700

    chore: Increase default session timeout to 2 hours

    The Google OAuth token refresh mechanism requires longer sessions than
    our default 30 minute timeout allows. Without this change, users would be
    logged out while their Google token is still valid, creating a confusing
    user experience.

    We've tested this change extensively and found no security concerns when
    using tokens with proper expiration validation, which our implementation
    handles correctly.

diff --git a/config/settings.go b/config/settings.go
index 1234567..89abcde 100644
--- a/config/settings.go
+++ b/config/settings.go
@@ -12,7 +12,7 @@ var (
     // Other settings...

     // Session timeout in minutes
-    SessionTimeout = 30
+    SessionTimeout = 120

     // Other settings...
 )
```

You can imagine these changes being in a single pull request - the overarching goal here was to add Google OAuth. But it took three steps:

- **Commit 1:** I refactored the authentication logic to allow for multiple providers. A new
  interface was introduced, a reviewer can easily see how it works, and most imporantly I can validate
  and test that my refactor works before moving on to the next step. The tests at this commit should
  pass!
- **Commit 2:** I added the Google OAuth provider. This is the actual feature I wanted to add. With
  the refactor in place, this was a very easy change to make. And you guessed it, the tests at this
  commit should pass!
- **Commit 3:** Uh oh! When I was testing the Google OAuth provider, I realized that the
  application's `SessionTimeout` was breaking things. That's a big change that affects far more than
  just the Google OAuth provider. So I made that change in a separate commit with a detailed
  explanation of _why_ it was changed.

<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 800 300">
  <!-- Main branch -->
  <line x1="50" y1="60" x2="700" y2="60" stroke="#0366d6" stroke-width="3" />

  <!-- Feature branch commits (colored dots) -->
  <circle cx="280" cy="150" r="10" fill="#28a745" />
  <circle cx="390" cy="150" r="10" fill="#28a745" />
  <circle cx="500" cy="150" r="10" fill="#28a745" />

  <!-- Feature branch connect lines -->
  <line x1="280" y1="150" x2="390" y2="150" stroke="#28a745" stroke-width="2" />
  <line x1="390" y1="150" x2="500" y2="150" stroke="#28a745" stroke-width="2" />

  <!-- Main branch commits -->
  <circle cx="100" cy="60" r="10" fill="#0366d6" />
  <circle cx="170" cy="60" r="10" fill="#0366d6" />
  <circle cx="280" cy="60" r="13" fill="#6f42c1" stroke="#000" stroke-width="2" />
  <circle cx="390" cy="60" r="13" fill="#6f42c1" stroke="#000" stroke-width="2" />
  <circle cx="500" cy="60" r="13" fill="#6f42c1" stroke="#000" stroke-width="2" />

  <!-- Branch labels -->

<text x="80" y="35" font-family="Arial" font-size="14" font-weight="bold">main</text>
<text x="130" y="150" font-family="Arial" font-size="14" font-weight="bold">feature-branch</text>

  <!-- Vertically rotated commit messages for feature branch -->

<text x="280" y="170" font-family="Arial" font-size="12" text-anchor="start" transform="rotate(90, 280, 170)">"refactor: Refactor user model"</text>
<text x="390" y="170" font-family="Arial" font-size="12" text-anchor="start" transform="rotate(90, 390, 170)">"feat: Google OAuth backend"</text>
<text x="500" y="170" font-family="Arial" font-size="12" text-anchor="start" transform="rotate(90, 500, 170)">"chore: Increase session timeout"</text>

  <!-- Direct lines connecting feature to main -->
  <line x1="280" y1="150" x2="280" y2="60" stroke="#6f42c1" stroke-width="2" stroke-dasharray="5,3" />
  <line x1="390" y1="150" x2="390" y2="60" stroke="#6f42c1" stroke-width="2" stroke-dasharray="5,3" />
  <line x1="500" y1="150" x2="500" y2="60" stroke="#6f42c1" stroke-width="2" stroke-dasharray="5,3" />
</svg>

Now, note a few things:

- This isn't any different from 'squash merge' in the sense that the merge strategy is exactly the
  same: rebase the changes on top of the main branch, then commit them. The only difference is that
  the commits used in the feature branch are preserved and all applied to the main branch, without
  squashing. 'Squash merge' and purposefully committing are both examples of _linear histories_.
  [You can require a linear history (either method!) on
  GitHub](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches#require-linear-history),
  too!
- Structuring your commits like this is more artistic than scientific. I _could_ have
  made the `SessionTimeout` change in Commit 2, since it was 'for the Google OAuth provider', but at
  the very least I would have explained in that commit message why I was changing it, just like in
  Commit 3. Someone in the future wondering why we have a 120 minute session timeout would see Commit 3 above, or:

  ```diff
  commit d7bf6e05c8a93f2e1a9d4b7c6e5d4c3b2a1f0e9d
  Author: Developer <dev@example.com>
  Date:   Wed Mar 19 10:21:36 2025 -0700

      feat: Google OAuth authentication backend

      Note that I had to change the application's SessionTimeout value because
      the Google OAuth token refresh mechanism requires longer sessions than
      our default 30 minute timeout allows. Without this change, users would be
      logged out while their Google token is still valid, creating a confusing
      user experience.

      We've tested this change extensively and found no security concerns when
      using tokens with proper expiration validation, which our implementation
      handles correctly.
  ```

Clean. Easy to follow. Tells a story. More than a monolithic squash commit, but always quite a bit
less than a pile of meaningless merge commits: you'll find that this approach almost always yields
1-3 commits per pull request, with an occasional outlier (< 10) for a larger change.

#### A Note on Conventional Commits

You might have noticed my use of ["conventional
commits"](https://www.conventionalcommits.org/en/v1.0.0/) with their `feat:`, `fix:`, and other
prefixes. While this format has benefits for automation tools and is compatible with purposeful
commits, it addresses a different concern. Conventional commits focus on categorizing changes, while
purposeful commits focus on organizing changes into a coherent narrative. Use the prefixes, or
don't! Not terribly important to the point here.

### Why This Matters

Purposeful commits aren't just about keeping your own work organized; they dramatically improve team collaboration:

1. **Easier Code Reviews**: Reviewers can understand your approach by following the logical progression of commits, rather than trying to mentally dissect a large, monolithic change.

2. **Safer Merges**: When conflicts arise, more granular commits make it easier to understand and resolve them correctly.

3. **Better Bisect Results**: When using `git bisect` to track down when a bug was introduced, purposeful commits provide clearer demarcation points between functional states.

4. **Improved Documentation**: Well-structured commits serve as documentation of design decisions and evolution, becoming an invaluable resource for future developers Git spelunking through the codebase years later.

5. **Better Software Thinking Habits**: You'll find that in structuring your changes this way,
   you'll reenforce asking yourself questions that you already should be in designing the software at
   all: What logical steps will get me to my goal? How can I break this down into meaningful
   increments? What parts might need to be refactored first?

### Pedantry-free Zone

This is usually where some folks scoff and point at GitHub. "Everyone uses GitHub! Just use PR descriptions and GitHub comments!"

Here's why that argument falls short: Ignoring the fact that PR descriptions are sometimes just as
neglected as commit messages, Git is the only persistent truth for your codebase. Everything else
is temporary. When your company switches from GitHub to another platform, or from Asana to Jira,
those external references become dead links. I've experienced this transition multiple timesin my
career - Tools change, companies change, repositories move. When projects are open-sourced,
external context disappears entirely.[^1]

Well-structured Git history travels with your code wherever it goes.[^2] It's the one
context that remains accessible regardless of which hosting service or project management tool you
use next.

**Regardless of wherever else you record context and information, having a clean, well organized,
and well annotated history in your version control software isn't pedantry—it's practicality.**

### Tools of the Trade

If you follow the purposeful commit pattern you'll quickly find that you need to be very, very
comfortable with Git history. The many revisions you make to your code both in development and in
response to pull request feedback mean you need to know how to view it and how to quickly and
constantly revise it.

Take our example from earlier:

<div class="git-log-container" style="font-family: Consolas, Monaco, 'Andale Mono', monospace; font-size: 14px; margin-bottom: 10px;">
  <pre style="background-color: #282c34; color: #abb2bf; padding: 16px; border-radius: 6px; overflow: auto; line-height: 1.45; white-space: pre; margin: 0;"><span style="color: #e6c07b; font-weight: bold;">$ git log --oneline --graph --all</span>
  <span></span>
  <span style="color: #61afef; font-weight: bold;">*</span> <span style="color: #98c379; font-weight: bold;">d7bf6e0</span> - <span style="color: #e06c75; font-weight: bold;">(feature-branch)</span> <span style="color: #dcdfe4;">chore: Increase default session timeout to 2 hours</span> <span style="color: #7f848e; font-style: italic;">(Wed Mar 19 10:21:36 2025)</span> <span style="color: #d19a66;">&lt;Developer&gt;</span>
  <span style="color: #61afef; font-weight: bold;">*</span> <span style="color: #98c379; font-weight: bold;">2fd8901</span> - <span style="color: #dcdfe4;">feat: Google OAuth authentication backend</span> <span style="color: #7f848e; font-style: italic;">(Wed Mar 19 10:12:45 2025)</span> <span style="color: #d19a66;">&lt;Developer&gt;</span>
  <span style="color: #61afef; font-weight: bold;">*</span> <span style="color: #98c379; font-weight: bold;">8d7f2c1</span> - <span style="color: #dcdfe4;">refactor: Refactor user model to accept multiple auth providers</span> <span style="color: #7f848e; font-style: italic;">(Wed Mar 19 09:47:18 2025)</span> <span style="color: #d19a66;">&lt;Developer&gt;</span>
  <span style="color: #61afef; font-weight: bold;">*</span> <span style="color: #98c379; font-weight: bold;">12e48b1</span> - <span style="color: #c678dd; font-weight: bold;">(origin/main)</span> <span style="color: #dcdfe4;">The commit at the head of `main`</span> <span style="color: #7f848e; font-style: italic;">(Wed Mar 19 09:00:00 2025)</span> <span style="color: #d19a66;">&lt;Developer&gt;</span></pre>
</div>

What if a reviewer asks you to change something in the refactor commit? What if you found a linter
error in the Google OAuth commit? You wouldn't want to add "oops" commits to the branch, or you're
creating noise in the history again.

You'll want to _change_ the refactor and Google OAuth commits to incorporate the feedback on those
changes, respectively. You'll need to know how to _amend_ things to keep your history clean and your
changes organized.

Which is where the concept of **[rebasing](https://git-scm.com/book/en/v2/Git-Branching-Rebasing)**
comes in. Rebasing is a powerful tool that I feel gets very misunderstood by junior (and honestly,
even senior) engineers as a Very Scary, Very Risky thing - likely because in the context of your
`main` trunk, that's true! But in the context of your feature branch, rebasing is a safe, powerful
tool that allows you to 'rewrite history' _of your changes_ before committing that history
permanently (merging to `main`).

Rebasing isn't nearly as complicated as most people think and it's not hard to have a very efficient
workflow for quickly amending your commits to follow the ideas of purposeful commits - but going
over [committing
fixups](https://git-scm.com/docs/git-commit#Documentation/git-commit.txt-code--fixupamendrewordltcommitgtcode),
[`rebase`](https://git-scm.com/docs/git-rebase), and
[autosquashing](https://git-scm.com/docs/git-rebase#Documentation/git-rebase.txt---autosquash) is
its own topic for another post.

### Happy Git Spelunking

I hope this has given you some ideas on how to structure your Git history in a way that's helpful
for future you and your team. Remember that it's practicality, not pedantry. It'll change how you
think about approaching software changes in general and make you a better engineer in the
process.

[^1]:
    When I worked at Red Hat, I worked on ManageIQ, an open-source project that was previously a closed-source one.
    The commit history needed to be purged of all context as part of the open-sourcing process, so the 'initial commit'
    of the open-source project was a massive commit that included all the changes from the closed-source project. But if you were a Red Hat employee, you'd be given
    the closed-source Git history to insert into your local repository, to see all prior context. Not the links to the old Bugzilla(?) tickets, though!

[^2]:
    _Pragmatically speaking._ Sure, there's a chance that you'll need to switch to a non-Git
    VCS in the future, but that's a _very_ rare experience in my experience, and there's usually
    ways to convert Git history to other VCSs. Aside: I've never used [Gerrit](<https://en.wikipedia.org/wiki/Gerrit_(software)>) but the concept of
    [patch sets](https://gerrit-review.googlesource.com/Documentation/concept-patch-sets.html) -
    iterative development on individual commits - sounds _exactly_ like a system built around
    purposefully committing :)

**RELATED POSTS:**

<a href="https://chrisarcand.com/git-autofix-for-amending-pull-requests/">
  <img src="/images/posts/socialmediacards/autofix_card.png" alt="Git Autofix for Amending Pull Requests" />
</a>
