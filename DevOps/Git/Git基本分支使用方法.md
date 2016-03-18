## Branch (Group changes)

This is a guide about git branch commands. We use branch command to manage serval authors works in a group, Even the works of ur self locally.

Firstly I will introduce the base command about branch. Secondly I will teach u how to handle the conflicts.  

```sh
$ git branch
List all local branches in the current repository.
$ git branch [branch-name]
Creates a new branch.
$ git checkout [branch-name]
Switches to the specified branch and updates the working directory.
$ git checkout -b [branch-name]
Create a new branch and switches to it.
$ git merge [branch-name]
Combines the specified branchs history into the current branch.
$ git branch -d [branch-name]
Deletes the specified branch.
$ git branch -D [branch-name]
Force delete when some change need to be ignored.
```

## Base operation about create and merge branches

Now lets assume there are two branch, A and B. A edit this documents like below.

```
# Original changes
A--begin
Branch A write something there.
Hello, This is A.
A--end
```
We want B can combines the change A made, after then delete branch A(because A finished its job). So what is the correct operations sequence?

```sh
$ git checkout A
first switch to branch A.
$ do somethins about the doc...
notes that , before u add and commit the changes, all changes will be seen by each branch on local.
$ git add [doc name]
$ git commit -m 'A changes'
Importantly! commit the changes.
$ git checkout B
$ git merge A
swithc to B and merge the changes made by A. Now we reach our goal.
$ git branch -d A
A can be deleted.
```

## Conflicts
Assume that two branch edit one doc, and comit it respectively. Then when we merge the two branch , conflict happens.
Git will show the warning whe merge and list the conflict in the doc, and u need to fix it manually.

```sh
# confilct of A changes
A--begin
A make some change on this doc  when B edit cocurrently.
Then A commit it.
A--end
```

Correct Sequences.
```sh
# A edit specified doc and commit it.
# B edit specified doc and commit it.
Note that when branch A and B have conflict,at each steps u should commit the change before u switch to antother. So I have point out at the base operation, when u not commit the change, each will see the change and commit it self to make it its change. When conflict happens, git will not allow u to switch other branch when the conflict not commit.
# B merge A
git will merge failed, and give some warning.At this time, manually to fix it.It will be easy because git have do most job and mark some tips on the conflicted doc like below.
<<<<<<< HEAD
master changes
============
>>>>>>> A
branch A changes
===========
# B commit the change (Importantly!)
$ git branch -D A
delete the branch A is a good choice.
```

## Remote & Local
Remote branch and local branch are isolated but related.

Create and update remote branch.
```sh
$ git push [alias] [branch-name]
Upload all local branch commits to GitHub and auto create if the remote branch not existed.
$ git push [alias] [local-branch]:[remote-branch]
Upload specified branch to remote.
```
Synchronize with remote branch
```sh
$ git fetch [alias] [branch-name]
$ git fetch [alias] [remote-branch]:[local-branch]
Download history of remote branch
$ git merge [bookmark]/[branch]
Then merge it. Or the below command pull is the union commands of fetch and merge  
$ git pull [alias] [branch-name]
$ git pull [alias] [remote-branch]:[local-branch]
```

## 储蓄
应急场景，把手头的工作隔离开，去单单解决bug，然后提交bug，然后再进行手头工作。
```sh
$ git stash ..
```

## 分支策略
良好的分支策略有几个原则
 - **master**分支应该是十分稳定的，用于发布产品
 - **dev**分支用于开发，是不稳定的，到某个发布节点的时候，再合并到**master**分支上
 - 每个人都自己的分支，分工合作，有一个人来协调往 **dev**分支上的合并

保留`merge`信息，git默认使用`Fast forward`模式，在删除分支后，会丢掉分支信息，使用`--no-ff`参数来merge就可以保留信息
```sh
$ git merge --no-ff -m "merge with no-ff" dev
$ git log --graph --pretty=oneline --abbrev-commit
```

