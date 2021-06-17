# git-review

OSP's Git Branches are setup in the following configuration

    master - Current Release Branch
    release/(Version) - Previous Official Releases
    development - Current Semi-Stable Test Branch for OSP vNext
    nightly - Current Nightly Test Branch for OSP vNext
    feature/(Name) - In-progress Feature Builds to be merged with the Development Branch

## Basic git operation
Check modification
$ git status

Check Detailed Modifications
$ git diff xxx.file

Add modification
$ git add xxx.file

If you would like to check the diff again:
$ git diff --staged xxx.file

Commit modification
$ git commit -m "we made some changes"

Or you can commit directly:
$ git commit -a -m "commit directly without add step"

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
## How to deal with confliction
$ git merge Tom
Auto merge failed; fix conflicts and commit the result

Check conflict file
$ git status
README.md

Check confliction in file

<<<<<<<HEAD
NIHAO
=============
Hello
>>>>>>>>>>>>

modify it to be like
Hello

Save and commit again on dev branch:
$ git add README.md
$ git commit -m "fix conflict"

When merge a branch, you can also disable fast forward merge
$ git merge --no-ff -m "merge with no-ff" dev

Check the branch history:
$ git log --graph --pretty=oneline --abbrev-commit

Show changes over time for a specific file:
$ git log -p xxx.file

# When fixing a bug, you will create a new branch, called issue-001
Before fix the bug, you need to save you current modification:

$ git stash
Save working directory and index state WIP on dev: f52c633 and merge

Use git status to check modification, and it will be clean
$ git status

# Determine which brach you want to start to fix the bug, maybe master branch
$ git checkout master

$ git checkout -b issue-001

$ vim xxx.file

$ git commit -a -m "fix bug issue-001"

$ git checkout master

$ git merge --no-ff -m "merged bug fix 101" issue-101

Now it's time to come back to dev branch and continue working:
$ git checkout dev
$ git status
nothing to commit, working tree clean(It's because that we stashed the modification)

$ git stash list
stash@{0}: WIP on dev: f52c633 and merge

Here are two way to recove the modifications:
1. git stash apply && git stash drop
2. git stash pop

Now, when you check the stash list, it will be empty:
$ git stash list

You can also use stash command multiple times and recove a specific version:
$ git stash list
$ git stash apply stash@{0}

当你的小伙伴从远程库clone时，默认情况下，你的小伙伴只能看到本地的master分支。不信可以用git branch命令看看：

$ git branch
* master

现在，你的小伙伴要在dev分支上开发，就必须创建远程origin的dev分支到本地，于是他用这个命令创建本地dev分支：

$ git checkout -b dev origin/dev

现在，他就可以在dev上继续修改，然后，时不时地把dev分支push到远程：


因此，多人协作的工作模式通常是这样：

    首先，可以试图用git push origin <branch-name>推送自己的修改；

    如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

    如果合并有冲突，则解决冲突，并在本地提交；

    没有冲突或者解决掉冲突后，再用git push origin <branch-name>推送就能成功！

如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name> origin/<branch-name>。

这就是多人协作的工作模式，一旦熟悉了，就非常简单。
小结

    查看远程库信息，使用git remote -v；

    本地新建的分支如果不推送到远程，对其他人就是不可见的；

    从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；

    在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；

    建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；

    从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。


TO DO:
add something to explain: what is "git rebase"?

如何建立远程dev分支：
1. 在github上建立dev分支
2. 在客户端建立并切换至dev分支
3. git fetch
4. git branch --set-upstream dev origin/dev
5. git merge issue-1
6. git push

If you have many remote branches that you want to fetch at once, do:
git pull -all 

Then, show all branches:
git branch -a

git log
git log -p -1
One of the more helpful options is -p or --patch, which shows the difference (the patch output) introduced in each commit. You can also limit the number of log entries displayed, such as using -2 to show only the last two entries.

git log -p filename
Show the modification history of a perticular file

git diff
Shows the changes between the working directory and the index. This shows what has been changed, but is not staged for a commit.

git diff --cached
Shows the changes between the index and the HEAD (which is the last commit on this branch). This shows what has been added to the index and staged for a commit.

git diff HEAD
Shows all the changes between the working directory and HEAD (which includes changes in the index). This shows all the changes since the last commit, whether or not they have been staged for commit or not.

if you want to see some abbreviated stats for each commit, you can use the --stat option
git log --stat

how to create a git patch?
git diff > my_custom_patch_file.patch
Then, another user can use this patch:
git apply patch_file.patch

Or, you can:
https://www.tutorialspoint.com/git/git_patch_operation.htm
git format-patch -1
Then, another user:
git apply 0001-xxxxx.patch

