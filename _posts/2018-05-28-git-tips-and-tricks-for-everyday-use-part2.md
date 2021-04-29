---
layout: post
title:  "Git tips & tricks for everyday use - part 2"
description: "After the succes of the previous tips and tricks post, I gathered some tricks to make my every day git use a bit easier."
---

The [first part of my git tips & tricks blogpost](https://jeroenthora.be/post/git-tips-and-tricks-for-everyday-use) got
some really good responses. It's been a while so I think the time is right to compose a second list of tips & tricks!
<!--more-->

## (Force) push the current branch

While force pushing you will always have to check you push the right branch. Otherwise it is possible you accidentally
force push another branch, for example the master branch. If that branch was behind on the origin master you will override
all changes done by others. To avoid this you can just use `HEAD` as the branch indicator:

```bash
git push origin HEAD -f 
```

## Force push with lease

To avoid overriding changes on the remote branch because you force push, git has a built-in safety check.
You can use the `--force-with-lease` flag, this way git will check that the remote branch has not been
updated since you last fetched it.

```bash
$ git push origin HEAD --force-with-lease
To /tmp/test-repo
 ! [rejected]        dev -> dev (stale info)
error: failed to push some refs to '/tmp/test-repo'
```

## Bisect run

Git bisect is a great tool to trace bugs to the commit that introduced them. But this can be a long process when it's an complex issue or it was committed a long time ago. In order to make this process easier and faster, you can use `git bisect run`.

This sub-command of the bisect tool allows to pass an automated check if a commit is good or bad. You can for example pass a phpunit test that will validate all commits that bisect will check. If the test(s) fail it's a bad commit otherwise it's a good commit.

```bash
git bisect start

git bisect good 85730ab # We know the functionality worked at commit 85730ab

git bisect bad fbe6fb8 # And we know that the current commit is bad

git bisect run phpunit tests/BrokenFunctionalityTest.php # Bisect is setup, so run it with our test to validate the functionality

# Result
ad1a436f0c15676cd5251e1d73c3af667e739a72 is the first bad commit
```

## Partial reverts

Sometimes you want to revert a "bad" commit but keep some of the changes. This can easily be done by the `
revert --no-commit (-n)` command.

```bash
git revert -n $bad_commit    # Revert the commit, but don't commit the changes
git reset HEAD               # Unstage the changes
git add --patch .            # Add the changes you want to keep
git commit                   # Commit those changes
```

## Easily switch to previous branch

Just like on the command line you can switch to the previous directory by using `cd -`, you can use this same trick to
switch easily to the previous branch.

```bash
git checkout feature-branch1

git status
    On branch feature-branch1
    
git checkout master

git status
    On branch master
    
git checkout -

git status
    On branch feature-branch1
```

## Gitignore templates

Github has provided a repository with a whole bunch of gitignore templates to get you started on a new project.
For example when you start a symfony project from scratch, just run the following command and you will have a
correct .gitignore to get started.

```bash
wget https://raw.githubusercontent.com/github/gitignore/master/Symfony.gitignore -O .gitignore
```

To find all available gitignore templates visit the [github/gitignore](https://github.com/github/gitignore) repository.

## Gitignore alias

With this alias you can easily add extra files or directories to the gitignore right from the command line.

```bash
git config --global --add alias.ignore '!f() { echo $1 >> .gitignore; }; f'

# Execute the command

git ignore /path/to/ignore/dir/*
```

## Change the base branch

When you accidentally create a feature branch from the incorrect branch. For example you started a branch from a feature
branch instead of the master branch. By using `rebase --onto` you can rebase the feature branch from the correct branch.
The syntax is like `rebase *original starting branch* --onto *correct branch*`.

```bash
git rebase feature-1 --onto master
```

## Git standup alias

A lot of teams use standups to recap the work each team member did the last day or so. When you work on a lot of
different issues it's hard to remember what you exactly did in that timespan. Therefor this alias makes it easy to retreive all the work
done in the past day in all branches.

```bash
git config --global --add alias.standup '!git log --branches --remotes --tags --no-merges --author="`git config user.name`" --since="$(if [[ "Mon" == "$(date +%a)" ]]; then echo \"last friday\"; else echo \"yesterday\"; fi)" --format=%s'

# Now you can run the following command and get all commits you did yesterday

git standup
```

## Bonus: lazy standup

This is a fun little extension to the `git standup` alias. We can use the built-in text-to-speech tool so you
don't even have to speak during the standup :)

```bash
git config --global --add alias.lazy-standup '!git standup-simple | spd-say -e' # On osx you can use the say command instead of spd-say

# Just run git lazy-standup and just sit back and relax :)
```

If you have other cool git tips and tricks or you like any of these, let it know in the comments below!
