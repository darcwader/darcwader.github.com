---
layout: post
title: "Git FAQ for Pull Request"
date: 2013-04-02 15:29
comments: true
categories: [code]
---

Git is a filesystem. It is not a cool thing anymore and has become a necessity to understand and get a good hold on using `Git`. Here are the steps to understand the pull request model.

## Basic structure of Pull Request mode

{% img /images/posts/2013-04-02/pull-request-structure.png %}

## Which repository do I clone?

As you can only commit to your own repository. It is best to clone from your repo. After cloning setup your 

```
git clone <myrepositoryurl> <folder>
git remote add upstream <upstreamurl>
```

### HEAD, master and other things

Git is more like a linked list. with different pointers. 

* `master` is a pointer in local repository. 
* `origin/master` is a pointer for master branch to remote named origin.
* `upstream/master` is a pointer to main master


## How do I add new remote?

every machine is a remote. if you clone from github repository then that repository is one remote.  by default the repository you clone from is added as `origin`.

```
git remote -v
git remote add <name> <url>
git remote rename <oldname> <newname>
```

if you are setting up a fork. usually you will have 2 remotes. by convention your repo will be named `origin` and the main repo will be named `upstream`.


## What is the difference between master and origin/master and upstream/master?

Any other repostitory for git is treated as `remote`. If you clone from a url , by default that url would be added as `origin`.

master is local branch and same branch exists in server. its pointer shows `origin/master`.

`git pull origin master` is actually two commands

```
git fetch origin
git merge origin/master
```

`fetch` would synchronize all pointers of remote. if someone committed to origin/master then our origin/master would move up but master would not.
`merge` would add our code into origin/master.

same applies to upstream

```
git fetch upstream
git merge upstream/master
```

## How do I pull latest changes from main repository
```
git fetch upstream
git merge upstream/master
```

## How do I get a ongoing feature from a colleague

Lets say your colleague is *bill* and you want his changes into your feature branch _feature-x_

1. first add your colleague's repository as a remote
2. get his branch code into your feature branch

current branch is feature-x

```
git remote add bill  <billremote>  
git fetch bill
git merge bill/change-branch-name
git push origin feature-x:feature-x
```

now `feature-x` has bill's changes.  once complete create a pull request from this branch.

## What all should be included in each pull request?

Goto the website and click pull-request and choose the branch from which it has to be created.

General Guideline

* Each pull request should be one feature
* Pull request should have complete shippable 
* Try to create different branch for each new feature


## I messed up, can I start over?

Two ways to handle a messed up pull request

1. Just start from scratch and create a new one
2. Create a new commit fixing the issues and send another pull request. Older one automatically gets updated.


## How to change branch of already added commits ?

lets assume your commits are in `master` and you want 2 commits to goto `new-branch`
```
git checkout master
git branch new-branch
git reset --hard HEAD~2     or git reset --hard <sha>
git checkout new-branch
```

## How do I delete a branch after finishing development?

```
git branch -d done-feature
git push origin :done-feature
```

## References

* [Git Scm Book](http://git-scm.com/book/en)
