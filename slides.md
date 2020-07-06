---
marp: true
---

# Git Talk (beyond the basics)
by Raphael D. Pinheiro

---

# Warming up

* HEAD - is a pointer to the latest commit in a branch
* SHA - Commit hash to uniquelly identify it. Made by: **tree hash + author + timestamp + previous commit SHA**
* Git is a database for diff snapshots
* Git is scriptable
* Git has recoverable abilities (through `reflog`)
* Patch - Textual commit information that can be transported by any medium (like email) and applied by the repo owners.
* Upstream - The location where you are going be pushing or pulling.

---

# Merging

Integrate changes into. Happens for for both `git merge` and `git pull`

### Fast-Forward

**Important: Only happens when the base has no new commits!**

* Not always possible!
* base HEAD pointer "fast-forward" to the last commit in the topic branch
* Linear Git history
* Commit hash keep the same

`$ (master) git merge topic`

---

# Merging

### No Fast-Forward (Recursive)

* It re-creates each commit from the topic branch on the tip of the base branch
* At the end, it creates a "Merge commit" with two parents
* This type of merge is performed by GitHub when you click "Merge Pull Request"
* You end up with branches on your Git log

`$ (master) git merge topic --no-ff`

---

# Squashing

Imagine you have written a new feature that demanded you 90 commits in a topic branch.

You want that to be preserved for historical and documenting reasons yet the person reading the base branch is not really interested on HOW but WHAT.

# What happens when you press that "Squash and Merge" button on GitHub?

`$ (master) git merge topic --squash`

This is what is run behind the scenes.

---

# What is a Rebase?

Seems kinda complicated, hm?
Actually, quite simple. The name itself says that.

## Every time you give a commit a different base (AKA changes the previous commit), a rebase action is taking place

---

# Moments when a Rebase happens

* pure git rebasing
* interactive rebasing
* git cherry-pick
* git pull

---

# Now that you know what the concept is...

## (From easiest to grasp to hardest) Git Cherry Pick

**Ability to selectivelly pick any commit and apply on top of a branch.**

Is it dangerous? No. The person behind the Git commands is the dangerous one. :)

**Remember, when you use cherry-pick, it gets the state of the whole file as Git is a snapshot based version control system.**


---

# Now that you know what the concept is...

## git rebase -i <after_commit_sha>

Allows you to **rewrite history**: delete commits, edit, squash, fixup, etc...

**Basically, it is a time machine for Git!**


---

# Pure Git Rebase

Syntax: git rebase <base_branch>

## What happens behind the scenes?

* Git repo is checkout to `base_branch` at `HEAD`
* Git calculates what commits doesn't exist in `base_branch` that you have made in the topic one
* Apply commits from previous step on the tip of the checked branch
* If any problem, conflicts will tell!
* Profit!

---

# Git Rebase --onto (2 args)

Don't let the strange syntax complicate on you.

## git rebase --onto F D
`Read like: Starting from D, rebase all on top of F`

```
          Before                           After
    A---B---C---F---G (branch)        A---B---C---F---G (branch)
             \                                     \
              D---E---H---I (HEAD)                  E---H---I (HEAD)
```

Instead of automatically get the due commits and apply them on top of the branch, get all commits after D and apply on top of F.

---

# Git Rebase --onto (3 args)

Gives you more control.

## git rebase --onto F D H
`Read like: Starting from D until H, rebase the commit interval on top of F`

```
          Before                           After
    A---B---C---F---G (branch)        A---B---C---F---G (branch)
             \                                     \
              D---E---H---I (HEAD)                  E---H (HEAD)
```

---

# Rebase in Git Pull

Both integration actions (merge and rebase) works for `git pull` commands.

So you can pull into your local copy like this: `git pull --rebase`

## Advantage?

It depends on the flow you are working with. But if the topic branch is a long-lived one, maybe it is interesting because it doesn't create the merge commit all the time there is something new.

---

# Why conflicts happen? :S

**Stop fearing the unknown!**

* When pulling from upstream and you receive changes in the same lines of your local changes.
* When rebasing interactively and you remove/edit a commit that the next commits depends on and make them incompatible
* When cherrying-picking and git brings the **full snapshot** from other branch, when you wanted just the changed lines
