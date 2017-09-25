# Protected Branch Workflow

There are several popular git workflows used in industry. A git workflow is a set of
steps and conventions that all project contributors adhere to while working in a
shared repository. A workflow describes
how a single developer's experimental code is eventually integrated into the main
project codebase once it is deemed to be of sufficient quality.

The main idea in the _protected branch workflow_ is that certain branches are only ever modified when the whole team agrees that specific commits should be added.
Protected branches usually represent a more stable, higher-quality snapshot of the project or
a version that might be released to users, for example.

* The master branch is always protected.
* Additional agreed-upon branches may also be protected. In the section below, we
  recommend one protected "staging" branch to represent a shared milestone that will
  eventually be merged with master. We call it a staging branch because that
  is where the team will combine and test their work before requesting to merge into the master branch.

### Terminology

*   __gitblarg__: Generic term to mean whichever repository hosting service used
    for your project: GitHub, GitLab, BitBucket, etc. The workflow is pretty much
    the same in all these services.
*   __P/MR__:  Request to merge a feature branch into a another branch in the repository; the     
    request is submitted through the Web UI of gitblarg. Different terms are used for this on
    different services: in GitHub this is called a Pull Request, in GitLab it's called a Merge
    Request. In this document, we will call it a P/MR. _A P/MR is reviewed and merged online in the
    Web UI provided by gitblarg._

## Code Review

In a P/MR, teammates discuss the proposed code and try to make it better.
There is usually one "author" -- the person who wrote the code and opened the
P/MR -- and one or two "reviewers" -- the people who read the code and comment about it.

### The purpose of code review

As a reviewer, you are not only checking if your teammate's code is "good enough". Code review is a key way in which teammates share knowledge about the project and learn from each other. The goals of code review are:

*   Understand the problem being solved by the P/MR.

    This should be clear
    from the commit message(s) and the P/MR description. Ask for clarifications
    as needed.

*   Understand the proposed code.

    Feel free to ask the author questions if parts are unclear to you.

*   Identify what works especially well in the code: feel free to
    write positive comments about something that the author has done
    particularly well.

*   Propose changes that would increase the quality of the code:

    * Is the code readable, is the style consistent, is it documented?
    * Is the code correct? Does it solve the problem? Are there simpler or    
      better ways to solve it?
    * Is the code well-tested? Do the tests pass?

#### Review flags

When participating in discussion on P/MRs, use a common shorthand to signal
specific requests and official responses/decisions:

*   `f?` - Please give me some informal feedback on this work-in-progress.
*   `f-` - I disagree with the general direction of this code. A different
    approach is needed.
*   `f+` - I think you're on the right track, keep going.
*   `r?` - Please review this finished code so that it can be merged.
*   `r-` - This code needs additional changes before it can be merged.
*   `r+` - This code is correct and complete, let's merge it!


### How to give good code review

Start by reading and understanding all the code in the P/MR. Feel free to
pull the branch to your local repo and actually run the code and run the
tests.

Write line-comments in the P/MR with your questions, opinions and suggestions.

Keep your feedback constructive: write specific comments that
are polite and respectful. Be clear about which changes are necessary to get an `r+` and which
changes are optional suggestions.

When you're done, write a summary comment about the whole P/MR that communicates your final
decision (f+,f-,r+,r-) and brief overview of the changes needed. `@`-mention the author so they get
a notification about your
finished review.

### How to receive good code review

Code review is hard work. Before you submit a P/MR, make sure your
code is easy to read, well-tested and well-documented.

Write commit messages that accurately summarize the changes and what
problem you're solving. The first line of the commit message should
be pretty short. You can add more detail in an extra paragraph if
needed.

Example commit message by author alice, with review from jlee:

```
Allow user to disable notification; r=jlee

Add a checkbox in the settings menu that disables
all system notifications for administrative users.

This separates the settings data into different classes
depending on the kind of user.

Authors: alice
```

When you make a P/MR, summarize your commits in a comment in the
gitblarg web UI so the reviewer understands the context. Ask a specific person
for either review (r?) or feedback (f?) and make it clear whether
your code is finished or still a draft, a work in progress.


## Recommended Team Workflow

We describe a workflow for a team, but a very similar process can be
used by an individual
working in a repository without any collaborators.

Let's assume that `origin` points to a repository shared by several students working on
a team assignment.

### Initial setup: Start a "staging" branch

This is the branch on which you record your __ready-to-submit__ work. When
your work is done, you will make a P/MR of the staging branch against the master branch.

Let's say you're working on Assignment 5, then you can call your
staging branch "Asst5".

Make sure that you're on the master branch, that the master branch up-to-date,
start the new branch:

```
git checkout master
git pull origin master
git checkout -b Asst5
git push origin Asst5
```

Other teammates will have to obtain the staging branch after they first
clone the team repo:

```
git clone url/to/project
```

Then in the root of the cloned repo:

```
git checkout Asst5
# If the above command doesn't work, try using fetch instead
git fetch origin Asst5:Asst5
```

### Individual member starts work on a feature

Work on your assignment in logical pieces, with each piece developed
on a separate branch. If it's a small/simple individual assignment,
maybe you only need one feature branch for all your work.

1.  Make sure that you're on your staging branch, that the staging branch is up-to-date, and start
    new branch:

    ```
    git checkout Asst5
    # in case new code from teammates has been merged on your staging branch
    git pull origin Asst5
    git checkout -b featureA
    ```

    __Never commit or push any protected branches__. In class assignments, your
    submission will be merged into master after it's graded. On the staging
    branch, only team-approved, working code can land by merging feature branches
    via P/MRs. If you're a team of
    one, you will decide when to merge feature branches.

2.  Work work work

    ```
    git add somefiles
    git commit
    ...
    ```

3.  You can push your branch to the remote whenever you like to back-up your work or make it
    visible to your teammates.

    ```
    git push origin featureA
    ```

4.  You can pull your branch from the remote if you switch to a different
    computer

    ```
    # first time on new computer
    git clone url/to/submission/repo
    cd path/to/submission/repo
    git fetch origin featureA:featureA
    # subsequent times just do
    git pull origin featureA
    ```

### Get feedback from teammates

When you want feedback on your work, open a P/MR for your feature branch against the
staging branch. Do this when you want early feedback from teammates (__f?__),
or when you want official code review (__r?__) from teammates so that your
work can be merged into the staging branch.

```
# in case teammates have landed new work on staging branch, bring it up to date
git checkout Asst5
git pull origin Asst5
git checkout featureA
# replay your changes onto latest Asst5 and fix any conflicts
git rebase Asst5
git push origin featureA
```

Then use the gitblarg UI online to open a P/MR against the Asst5 branch and flag a teammate for
review or discussion.

### Incorporating Feedback

1.  When you get feedback (__f-, r-__) on your P/MR and need to change things, just make new    
    commits on your feature branch and push again.

    ```
    git checkout featureA
    git add; git commit
    git push origin featureA
    ```

    The new commits will appear on the P/MR, and you can ask your teammates for
    feedback again.

2.  (Optional) Once your feature is approved in the P/MR, you can clean up your
    commits and commit messages if needed. Modify your commit message(s) to
    indicate the author(s) and reviewer(s) of each commit.

    ```
    # n is the number of commits you want to change (e.g. the last 5 commits)
    git rebase -i HEAD~n
    # We need -f to "force" the push because rebase rewrites history
    # It's okay to rewrite history because this is your own personal feature
    # branch
    git push -f origin featureA
    ```

    _Note:_ Never force-push (git push -f) to any protected branch, because this
    rewrites history that you share with your teammates! If you force push, your
    teammates will have trouble pulling and will have to waste their time fixing
    their repo! (It's okay to force push to a branch that you are working on alone
    because it doesn't contain any shared history so it won't ruin anything for
    anyone else.)

### If you are working alone

Instead of opening a P/MR, you can just merge your feature branch into your
staging branch yourself at the command-line whenever you are happy with your
feature.

```
git checkout Asst5
git merge featureA
```

### When your feature is approved by the team

1.  When your P/MR is approved (__r+__ from teammate), use the gitblarg web UI to __merge__ it
    (click the Accept/Merge button): now featureA gets merged into the staging branch (Asst5).

2.  Pull the new commits from the remote. The commits you made for featureA are
    now on the Asst5 branch.

    ```
    git checkout Asst5
    git pull origin Asst5
    ```

3.  _(Optional)_ Now you can also safely delete your featureA branch locally and on
    the remote because it's been merged. To delete it on the remote, find the
    delete button in the web UI. To delete it from your local repo:

    ```
    git branch -d featureA
    ```
4.  Keep working on more parts of the assignment on new branches. (Repeat the
    above process as needed. You can alternate working on several features at
    once; you just need to keep the different branches up to date.)

    ```
    git checkout Asst5
    git checkout -b featureB
    ```

### When you are ready to submit your finished work (for whole team/assignment)

You submit your work by making a P/MR of your staging branch (Asst5) against the master branch.  
__Only one teammate__ needs to do this:

```
# pull in all the latest changes on the staging branch (to make sure you have
# all your teammates' work)
git checkout Asst5
git pull origin Asst5
# check that the log makes sense, test the code,
# check that the commits look good
git log -p
# If any final changes need to be made at this point
# follow the steps starting at "Individual member starts work on a feature"
# to put your changes in a feature branch.
```

Next open the P/MR in gitblarg UI of Asst5 against __master__ and
request review from your teacher.

### When you want to incorporate feedback from teacher

After grading, teacher might require you to make some changes before approving your "submission
P/MR" and merging into master.

This is the same as working on a new feature. Start a feature branch based on the latest version of
your staging branch, Asst5. Make your changes. Open a P/MR of the _feature branch
against Asst5_. The team approves the P/MR and merges it to the Asst5 branch.

After merging, there are new commits on your Asst5 branch. These commits
will appear automatically in the P/MR you originally made against master.
Ask the teacher for re-review in that original "submission P/MR".
