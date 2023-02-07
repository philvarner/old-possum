# Git

## Resources

* [Learn Git The Hard Way](https://leanpub.com/learngitthehardway) by Ian Miell
* [Atlassian Git Tutorials](https://www.atlassian.com/git/tutorials), especially the advanced ones
* [Oh Shit, Git!?!](https://ohshitgit.com/) or [Dangit, Git!?!](https://dangitgit.com/en)
* [The Architecture of Open Source Applications - Git](http://aosabook.org/en/git.html) by Susan Potter
* [Pro Git](https://git-scm.com/book/en/v2) by Scott Chacon and Ben Straub (2016, so a little out-of-date, but an excellent description of the internals)
* Adam Ruka [OneFlow – a Git branching model and workflow](https://www.endoflineblog.com/oneflow-a-git-branching-model-and-workflow) and [Gitflow considered harmful](https://www.endoflineblog.com/gitflow-considered-harmful) -- I used gitflow for a long time and found it hard to use for SaaS, and this nails a lot of the things I didn't like and had to unlearn
* [Chris Beams - How to Write a Git Commit Message](https://chris.beams.io/posts/git-commit/)
* [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) and [commitlint](https://github.com/conventional-changelog/commitlint)
* [joshnh/Git-Commands](https://github.com/joshnh/Git-Commands)

## Config

* [A great example .gitconfig from pksunkara](https://gist.github.com/pksunkara/988716)

### aliases in ~/.gitconfig [alias]

```
[alias]
  sw = switch
  ci = commit
  st = status
  re = restore
  br = branch
  l = log --oneline --graph --decorate  --all
  pick = cherry-pick -n
```

## Commands

### Checkout is confusing

`checkout` was a confusing command, an has now mostly been replaced by other commands.

Instead of `git checkout some_branch`, use `git switch some-branch`

Instead of `git checkout file_in_working_tree`, use `git restore file_in_working_tree`

Instead of `git checkout -b some_branch`, use `git branch some-branch -u origin/some-branch` (but then have to sw into the branch)


git br docs-itertools-examples
git push -u origin docs-itertools-examples

### Forking a GitHub repo

Based on [GitHub Fork a Repo docs](https://docs.github.com/en/github/getting-started-with-github/fork-a-repo#keep-your-fork-synced) and [Merging an Upstream Repo into your Fork](https://help.github.com/articles/merging-an-upstream-repository-into-your-fork/)

Use the GitHub UI to fork the project, e.g., fork `git@github.com:octocat/Spoon-Knife.git to` `git@github.com:octocat/Spoon-Knife.git`. Then:

```
git clone git@github.com:YOUR-USERNAME/Spoon-Knife.git
git remote add upstream git@github.com:octocat/Spoon-Knife.git
git fetch upstream
```

If you already cloned from what you now want to be the upstream before forking:

```
git remote set-url origin git@github.com:YOUR-USERNAME/Spoon-Knife.git
git remote add upstream git@github.com:octocat/Spoon-Knife.git
git branch -D main
```

Then, create local branches tracking branches of the same name in the upstream:

```
git branch main -u upstream/main

# OR

git branch develop -u upstream/develop
git branch master -u upstream/master
```

### Fetch a remote

The entire remote:
```
git fetch origin
git fetch upstream
```

only one branch of a remote:
```
git fetch origin main
```

## Merge a remote to local 
on `main`:

```
git merge origin/main
```

## Rebase

Rebase a branch against it's parent, on the branch, run:
```
git rebase main
```

To interactively rebase (e.g., to squash)

```
git rebase -i HEAD~4
```

## Reset

* `--hard` -- resets both both index and working tree (e.g., trashes anything not committed)
* `--mixed` -- resets the index but not the working tree
* `--soft` -- leaves index and working tree as they are. Useful for getting some changes "out" of a branch to modify and re-commit.

Examples:
```sh
git reset --hard ${hash}
git reset --hard HEAD~4 # four commits before HEAD
```

## Push

Force push to a remote.  Dangerous if anyone else is using it!

```
git push --force origin my_branch
```

## Rename branch locally and remotely

```
git branch -m old_branch new_branch         # Rename branch locally    
git push origin :old_branch                 # Delete the old branch    
git push --set-upstream origin new_branch   # Push the new branch, set local branch to track the new remote
```

## Delete a branch locally and remote

```sh
git branch -d {branch_name}
git push {remote_name} -d {branch_name}
```

## Stash 

Show all the patch-diff format for a stash:

```
git stash show -p stash@{0}
```

## Submodules

```
git clone --recursive 
git submodule init 
git submodule status
git submodule update
```

## Hooks

Hooks are not part of a clone, but you can either (1) put the scripts in the repo and require users to symlink to them or (2) add them to the [Template Directory](http://git-scm.com/docs/git-init#_template_directory)

`.git/hooks/pre-commit` is a local hook run prior to committing. to bypass, use `git commit --no-verify`

`.git/hooks/pre-receive` is a server hook run prior to accepting a push

## Clean branches that don't exist on the remote

```
git fetch --prune
```

## Reflog

Lost a commit some how? check the reflog ("reference log")

## Bisect

Useful for finding a breaking change if you can easily validate a bug against it. 
