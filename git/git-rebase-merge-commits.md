# git rebase workflow

Use `git rebase -r` if you want to preserve the merge commits.

See https://mtyurt.net/post/2019/git-rebase-merges-option.html

Older versions, use `git rebase -p`

# Rebasing Merge Commits in Git

Note: This post hasn't been updated in over 2 years.

The TL;DR version is this: **When rebasing, always use the -p flag**,

First, though, a small diversion – why rebase is part of my normal git workflow.
 

### Why I starting using pull –rebase

Using `git pull --rebase` is becoming more popular to avoid unnecessary merge commits when  fetching the latest code from master. There are a few blog posts on the  matter, such as [[1\]](http://web.archive.org/web/20150527173852/http://blog.woobling.org/2009/05/git-rebase-considered-awesome.html) [[2\]](http://web.archive.org/web/20150527173852/http://yehudakatz.com/2010/05/13/common-git-workflows/)

I’ll give a brief summary of why this convinced me. For me, it boils down to two simple cases:

#### 1. You haven’t made any changes to your branch

In this case, `pull` and `pull --rebase` will simply fast-forward. No problems.

#### 2. You’ve got one or two small changes you forgot to push

In this case, the default `pull` will actually merge the remote changes into your branch, making a merge commit. This is bad for a couple of reasons, messiness is one, but I  actually consider the problems it causes for `git bisect` more compelling (I must remember to write about that one day).

With `git pull --rebase`, you simply replay those commits on top of the new head. Now, if you  push, you have linear history, rather than a divergence/merge. I think  this is a better result. Usually, I follow the ‘always work on a branch’ and ‘merges are meaningful and good’ practice (partly inspired by [[3\]](http://web.archive.org/web/20150527173852/http://nvie.com/posts/a-successful-git-branching-model/)), but there’s no semantic difference between `master` and `origin/master`, so linear history makes sense.

So in general, `git pull --rebase` is better than a `git pull`. To make it the default, see [[4\]](http://web.archive.org/web/20150527173852/http://www.viget.com/extend/only-you-can-prevent-git-merge-commits/).

There is one major problem with it, though – merge commits.

### Rebasing deletes merge commits

This is best explained with an example

#### First, one that doesn’t fail

Given this simple repo:

```
[master] git pull --rebase
First, rewinding head to replay your work on top of it...
Applying: little fix
Applying: forgot to push this
```

```
[master] git push
Counting objects: 6, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (5/5), 526 bytes, done.
Total 5 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (5/5), done.
To /Users/glen/envato/demo-origin
   f9c3cb8..e4a2e92  master -> master
```

Linear history, just as we wanted!

#### All aboard the failboat

Say you’ve been working on your little feature for a while, like this:

Then you merge to master (using `--no-ff`, of course [[3\]](http://web.archive.org/web/20150527173852/http://nvie.com/posts/a-successful-git-branching-model/))

```
[master] git merge --no-ff feature
Merge made by recursive.
 b |    3 +++
 1 files changed, 3 insertions(+), 0 deletions(-)
 create mode 100644 b
```

Then you go to push, but somebody got in there first (`origin/master` has moved on)

```
[master] git push
To /Users/glen/envato/demo-origin
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to '/Users/glen/envato/demo-origin'
To prevent you from losing history, non-fast-forward updates were rejected
Merge the remote changes (e.g. 'git pull') before pushing again.  See the
'Note about fast-forwards' section of 'git push --help' for details.
```

Of course, trying to push hasn’t updated our reference to `origin/master`, we need to `git fetch` to see the full picture

```
[master] git fetch
remote: Counting objects: 5, done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (3/3), done.
From /Users/glen/envato/demo-origin
   49ab1cf..9f3e34d  master     -> origin/master
```


 Ah, yes. Someone has pushed a commit ‘sneaky extra commit’ before we  were able to push our commit (merging in of branch feature). So, we  would normally just `git pull --rebase` to get ready to push, but if we do that, the **merge commit gets deleted!**

```
[master] git pull --rebase
First, rewinding head to replay your work on top of it...
Applying: my work
Applying: my work
Applying: my work
```

Our merge commit has disappeared!

This is bad for a whole lot of reasons. For one, the feature commits are  actually duplicated, when really I only wanted to rebase the merge. If  you later merge the feature branch in again, both commits will be in the history of master. And `origin/feature`, which supposed to  be finished and in master, is left dangling. Unlike the awesome history  that you get from following a good branching/merging model, you’ve  actually got *misleading* history.

For example, if someone looks at the branches on origin, it’ll appear that `origin/feature` hasn’t been merged into master, even though it has! Which can cause all kinds of problems if that person then does a deploy. It’s just bad news all round.

Worst of all, you did everything ‘right’. You used `merge --no-ff` and `git pull --rebase`. Sad face.

In case it’s not obvious, this is what we wanted to happen:

You can recover from this situation (if you discover it before you push) by resetting and redoing the merge:

```
[master] git reset --hard origin/master
HEAD is now at 9f3e34d sneaky extra commit
[master] git merge --no-ff feature
Merge made by recursive.
 b |    3 +++
 1 files changed, 3 insertions(+), 0 deletions(-)
 create mode 100644 b
```

### The solution!

In the manpage for git-rebase

```
-p
--preserve-merges
Instead of ignoring merges, try to recreate them.

This uses the --interactive machinery internally, but combining it with the --interactive option explicitly
is generally not a good idea unless you know what you are doing (see BUGS below).
```

Or, to put it another way:

So, instead of using `git pull --rebase`, use a `git fetch origin` and `git rebase -p origin/master`:

```
[master] git fetch
remote: Counting objects: 5, done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (3/3), done.
From /Users/glen/envato/demo-origin
   49ab1cf..9f3e34d  master     -> origin/master
```



```
[master] git rebase -p origin/master
Successfully rebased and updated refs/heads/master.
```


 Win!

### Downsides

#### Git pull is dead

The `-p` flag doesn’t apply to `git pull --rebase`, so you have to start explicitly fetching and rebasing. To be honest, I  think this is more an upside. Fetching explicitly is good, since it  refreshes your entire copy of the remote, and lists what branches have  moved on (handy on a fast-moving project). But for those used to a  single-step pull, this is slightly more work.

#### ORIG_HEAD is no longer preserved

ORIG_HEAD, once you get used to using it, is really handy to undo a destructive operation. Sadly, `git rebase -p` sets ORIG_HEAD for each commit being rebased, so you can’t use it to  quickly return to the start of a rebase, something I ran in to working  on this post.

#### Branch tracking is not used

Unlike `git pull --rebase`, which will fetch changes from the branch your current branch is tracking, `git rebase -p` doesn’t have a sensible default to work from. You have to give it a  branch to rebase onto. With a good alias, however, that can be made  painless.

### Aliases

So, how about some aliases to make this all idiot-proof? I’ve decided to call mine `gup` (I’ve taken to calling it gee-up), and I’ve got it in a gist for bash, fish and zsh [here](http://web.archive.org/web/20150527173852/http://gist.github.com/590895). I’ve also included my `gpthis` alias for pushing without branch tracking.

`gup` will do a fetch of origin and `rebase -p` of the branch on origin with the *same name* as the current branch. For 99% of cases, this is exactly what I want.

### Conclusion

This morning, I had no idea about the `--preserve-merges` flag on rebase, and was about ready to cry foul on using rebase at all, considering how bad this problem can get on a big project. But, as with everything Git, once you understand it a bit better, there’s usually a  more complex way that sucks a whole lot less. Which is why aliases like `gup` are really handy – you can keep changing what your aliases mean without having to learn a new habit.

I’d welcome comments and suggestions, you can either reply here or hit me up on twitter – [@glenmaddern](