# Git Workflow

The simple workflow we use has a few guiding principles:

* master is always production-like and deployable
* master-next is always production-like and deployable
* rebase during feature development, explicit merge when done.
* only rebase when you are in your own branches

## 1) Pull changes from master

Start by pulling the latest changes from master

```
git checkout master
git fetch origin
git merge master
```

You can also do the same as above by doing git pull

On branch master

```
git checkout master
git pull origin master
```

## 2) Branch off to isolate the feature or bug-fix in a branch

If you don't have an issue created in github yet, please create a new one in the relevant repository (github.com/stickerkid/[repo-name]/issues). For example, if working on the [StickerKid website](https://github.com/stickerkid/sk-ecommerce/issues). Once you have the issue number:

```
git checkout -b [issue-number]-awesome-feature
```

## 3) Work on the feature

On branch [issue-number]-awesome-feature

```
git add .
git commit -m "[org-or-user]/[repo-name]#[issue-number] - awesome commit"
```

## 4) To keep up with master branch, use rebase (only rebase in your own branches)

On branch [issue-number]-awesome-feature

```
git fetch origin
git rebase origin/master
```

## 5) When ready for feedback, a) push your branch remotely and b) create a pull request on github (https://github.com/[org]/[repo]/pulls)

On branch [issue-number]-awesome-feature

```
git push -u origin [issue-number]-awesome-feature
```

## 6) Once pull request is approved, you can do a final rebase cleanup

On branch [issue-number]-awesome-feature

```
git rebase -i origin/master
```

## 7) When development is complete (code has been reviewed), merge into master-next and push

```
git checkout master-next
git reset --hard origin/master
git merge [issue-number]-awesome-feature
git push origin master-next
```


