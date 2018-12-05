# git-review

## Basic git operation
Check modification
$ git status

Add modification
$ git add xxx.file

Commit modification
$ git commit -m "we made some changes"

Or you can commit directly:
$ git commit -a -a "commit directly without add step"

## git tag
To view existing tags:
$ git tag
v0.1
v1.3

To create a tag:
$ git tag -a v1.4 -m 'my version 1.4'
$ git tag
v0.1
v1.3
v1.4

Show information about v1.4
$ git show v1.4

## git branching

Check all existing branch
$ git branch

Create a new branch
$ git branch dev

Switch to dev branch
$ git checkout dev

Master branch is very stable and is used to release stable versions.
All developers should commit to dev branch, and work on their own
branches.

You can also create and switch to new branch directly
$ git checkout -b 'Tom'

Make some changes and commit
$ vim README.md
$ git commit -a -m 'fixed a bug'

Switch to dev node
$ git checkout dev

Merge the changes to dev
$ git merge Tom

If you no langer need the branch, just delete
$ git branch -d Tom
## Add some testing words
