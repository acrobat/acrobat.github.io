---
layout: post
title:  "Git tips & tricks for everyday use"
---

I've been working with git for the last few years and I have some tricks to make my every day use a bit easier. So I wanted to share them and hopefully help
someone with it.

This article is aimed at somebody who has already some experience using git.
<!--more-->

## Get a file from another branch without changing your current branch

When you are working on a feature branch but you need a file from another branch, you could stash your changes, switch to the
other branch and copy the file. But you can do this with a single action.

```bash
git checkout <other-branch> -- path/to/file
```

## Partial adds

You did a lot of changes but you want to commit them in separate commits. Look no further!

```bash
git add -p
```

## Highligh word changes with git diff

Full diffs can give not enough information. Sometimes all text was on a single line so you won't see the exact difference. So
you can use the option `--word-diff`

```bash
git diff --word-diff
```

## Better logging

A cleaner git log output with colors and a tree visualization.

```bash
git log --pretty=format:"%h %ad | %s%d [%an]" --graph --date=short
```

Output:

```bash 
* b034f07 2016-11-22 | Merge feature-2 into master
|\  
| |\  
| | * 914ab62 2016-11-22 | More changes
* | | 43941b5 2016-11-22 | New feature 2
|/ /  
* | 7d191da 2016-11-22 | Starting on feature xxx
|/  
* 127c16f 2016-11-22 | bugfix xxx
```

or without the custom format

```bash
git log --oneline --decorate --graph
```

## Using git bisect to track down bugs

Sometimes it is hard find the commit which caused a certain issue. You could manually checkout out specific commits and hope you choose somewhat right
or you use the `bisect` tool.

```bash
git bisect start
git bisect good {{good-commit-hash}}
git bisect bad HEAD

```

Bisect will checkout a commit in the middle between `good-commit-hash` and `HEAD`. Then you will have to review this commit and mark it as
good or bad.

```bash
git bisect good/bad
```

This will continue until you've found the commit which caused the issue. Run the following commands to return to the original state and get a summary from `git bisect`.

```bash
git bisect reset
git bisect log
```

## Git aliases

Git supports aliases for git commands, this way you tweak existing command to perform better or abbreviate commands. You can add aliases by editing your `.gitconfig` file or by running the `git config` command.
These are a few examples, check out [my gitconfig](https://github.com/acrobat/dotfiles/blob/master/gitconfig) for more aliases and custom configs.

```bash
git config --global alias.stat = status
git config --global alias.remotes = remote -v
```

## Command autocompletion

The git repository already ships with some autocompletion helpers, you will only have to download and activate them.
See the [autocompletion directory](https://github.com/git/git/tree/master/contrib/completion) for the different available formats.

Execute the following commands to enable autocompletion on your git commands:

```bash
cd ~
curl -O https://raw.githubusercontent.com/git/git/master/contrib/completion/git-completion.bash
mv git-completion.bash .git-completion.bash
```

Finally place this line in your `.bashrc` file and you are good to go!

```bash
source ~/.git-completion.bash
```

## Ignore whitespace changes

When checking a diff or checking code with `git blame` it's possible you want to ignore whitespace changes. Just use the `-w` option

```bash
git diff -w
git blame -w
```

## Bonus: use legit

Legit is a complementary command-line interface for Git, optimized for workflow simplicity. It is heavily inspired by GitHub for Mac.
It will add some extra functionality to your git installation.

```bash
git sync
# Syncronizes current branch. Auto-merge/rebase, un/stash.

git switch <branch>
# Switches to branch. Stashes and restores unstaged changes.

git publish <branch>
# Publishes branch to remote server.

git unpublish <branch>
# Removes branch from remote server.

git branches
# Nice & pretty list of branches + publication status.
```

See [git-legit.org](http://www.git-legit.org/) for more information.

These are my git tips and tricks and I hope this was useful for you. What are your git tricks? Please share them in the comments below!
