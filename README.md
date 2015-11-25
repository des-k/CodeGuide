--------------------------------------------------------------------------------

Code Guide 

--------------------------------------------------------------------------------

Git Guide based on git docummentation

(0) Decide what to base your work on.

In general, always base your work on the oldest branch that your
change is relevant to.

 - A bugfix should be based on 'maint' in general. If the bug is not
   present in 'maint', base it on 'master'. For a bug that's not yet
   in 'master', find the topic that introduces the regression, and
   base your work on the tip of the topic.

 - A new feature should be based on 'master' in general. If the new
   feature depends on a topic that is in 'pu', but not in 'master',
   base your work on the tip of that topic.

 - Corrections and enhancements to a topic not yet in 'master' should
   be based on the tip of that topic. If the topic has not been merged
   to 'next', it's alright to add a note to squash minor corrections
   into the series.

 - In the exceptional case that a new feature depends on several topics
   not in 'master', start working on 'next' or 'pu' privately and send
   out patches for discussion. Before the final merge, you may have to
   wait until some of the dependent topics graduate to 'master', and
   rebase your work.

 - Some parts of the system have dedicated maintainers with their own
   repositories (see the section "Subsystems" below).  Changes to
   these parts should be based on their trees.

To find the tip of a topic branch, run "git log --first-parent
master..pu" and look for the merge commit. The second parent of this
commit is the tip of the topic branch.

(1) Make separate commits for logically separate changes.

Unless your patch is really trivial, you should not be sending
out a patch that was generated between your working tree and
your commit head.  Instead, always make a commit with complete
commit message and generate a series of patches from your
repository.  It is a good discipline.

Give an explanation for the change(s) that is detailed enough so
that people can judge if it is good thing to do, without reading
the actual patch text to determine how well the code does what
the explanation promises to do.

If your description starts to get too long, that's a sign that you
probably need to split up your commit to finer grained pieces.
That being said, patches which plainly describe the things that
help reviewers check the patch, and future maintainers understand
the code, are the most beautiful patches.  Descriptions that summarise
the point in the subject well, and describe the motivation for the
change, the approach taken by the change, and if relevant how this
differs substantially from the prior version, are all good things
to have.

Make sure that you have tests for the bug you are fixing.  See
t/README for guidance.

When adding a new feature, make sure that you have new tests to show
the feature triggers the new behaviour when it should, and to show the
feature does not trigger when it shouldn't.  Also make sure that the
test suite passes after your commit.  Do not forget to update the
documentation to describe the updated behaviour.

Speaking of the documentation, it is currently use of US English 
norms for spelling and grammar, which is somewhat
unfortunate.  A huge patch that touches the files all over the place
only to correct the inconsistency is not welcome, though.  Potential
clashes with other changes that can result from such a patch are not
worth it.  We prefer to gradually reconcile the inconsistencies in
favor of US English, with small and easily digestible patches, as a
side effect of doing some other real work in the vicinity (e.g.
rewriting a paragraph for clarity, while turning en_UK spelling to
en_US).  Obvious typographical fixes are much more welcomed ("teh ->
"the"), preferably submitted as independent patches separate from
other documentation changes.

Oh, another thing.  We are picky about whitespaces.  Make sure your
changes do not trigger errors with the sample pre-commit hook shipped
in templates/hooks--pre-commit.  To help ensure this does not happen,
run git diff --check on your changes before you commit.


(2) Describe your changes well.

The first line of the commit message should be a short description (50
characters is the soft limit, see DISCUSSION in git-commit(1)), and
should skip the full stop.  It is also conventional in most cases to
prefix the first line with "area: " where the area is a filename or
identifier for the general area of the code being modified, e.g.

 - archive: ustar header checksum is computed unsigned

 - git-cherry-pick.txt: clarify the use of revision range notation

If in doubt which identifier to use, run "git log --no-merges" on the
files you are modifying to see the current conventions.

The body should provide a meaningful commit message, which:

 - explains the problem the change tries to solve, iow, what is wrong with the
   current code without the change.

 - justifies the way the change solves the problem, iow, why the result with the
   change is better.

 - alternate solutions considered but discarded, if any.

Describe your changes in imperative mood, e.g. "make xyzzy do frotz"
instead of "[This patch] makes xyzzy do frotz" or "[I] changed xyzzy
to do frotz", as if you are giving orders to the codebase to change
its behaviour.  Try to make sure your explanation can be understood
without external resources. Instead of giving a URL to a mailing list
archive, summarize the relevant points of the discussion.

--------------------------------------------------------------------------------

Determine how are you going to solve the problem

This is the “how” step, where you determine how you are going to solve the
problem you came up with in step 1. It is also the step that is most neglected
in software development. The crux of the issue is that there are many ways to
solve a problem -- however, some of these solutions are good and some of them
are bad. Too often, a programmer will get an idea, sit down, and immediately
start coding a solution. This often generates a solution that falls into the bad
category.

Typically, good solutions have the following characteristics:

 - They are straightforward.

 - They are well documented (especially any assumptions being made).

 - They are built modularly, so parts can be reused or changed later without
   impacting other parts of the program.

 - They are robust, and can recover or give useful error messages when something
   unexpected happens.

When you sit down and start coding right away, you’re typically thinking “I want
to do _this_”, so you implement the solution that gets you there the fastest.
This can lead to programs that are fragile, hard to change or extend, or have
lots of bugs.

Studies have shown that only 20% of a programmer’s time is actually spent
writing the initial program. The other 80% is spent debugging (fixing errors) or
maintaining (adding features to) a program. Consequently, it’s worth your time
to spend a little extra time up front before you start coding thinking about the
best way to tackle a problem, what assumptions you are making, and how you might
plan for the future, in order to save yourself a lot of time and trouble down
the road.
