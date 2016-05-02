# Git

<!-- MarkdownTOC -->

- [Rename local branch](#rename-local-branch)
- [Move most recent commit\(s\) to a new branch](#move-most-recent-commits-to-a-new-branch)
    - [Moving to a new branch](#moving-to-a-new-branch)
    - [Moving to an existing branch](#moving-to-an-existing-branch)

<!-- /MarkdownTOC -->

## Rename local branch

Current branch:

```bash
git branch -m <newname>
```

Any branch:

```bash
git branch -m <oldname> <newname>
```

[Source](http://stackoverflow.com/a/6591218/1238596)

## Move most recent commit(s) to a new branch

### Moving to a new branch

Unless there are other circumstances involved, this can be easily done by branching and rolling back.

```bash
git branch newbranch
git reset --hard HEAD~3 # Go back 3 commits. You *will* lose uncommitted work.*1
git checkout newbranch
```

But do make sure how many commits to go back. Alternatively, instead of `HEAD~3`, simply provide the hash of the commit you want to 'revert back to" on the `master/current` branch, e.g:

```bash
git reset --hard a1b2c3d4
```

*1 You will only be "losing" commits from the `master` branch, but don't worry, you'll have those commits in `newbranch`!

### Moving to an existing branch

**WARNING:** The method above works because you are creating a new branch with the first command: `git branch newbranch`. If you want to use an existing branch you need to merge your changes into the existing branch before executing `git reset --hard HEAD~3`. If you don't merge your changes first, they will be lost. So, if you are working with an existing branch it will look like this:

```bash
git checkout existingbranch
git merge master
git checkout master
git reset --hard HEAD~3 # Go back 3 commits. You *will* lose uncommitted work.
git checkout existingbranch
```

[Source](http://stackoverflow.com/a/1628584/1238596)
