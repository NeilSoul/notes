Git的使用最头疼的地方就在于合并的冲突。在多人协作时，如果没有管理好代码分工，是很容易产生冲突的。当然Git自带了一套fix conflict的机制，当我们merge失败时，会在文件上产生很多提示，对于我们手工修复带来很大便利。但当这一套不灵时，就有了下面几种方法。

## 分支覆盖法

当conflict太多的时候，我们甚至懒得去合并了，不想手工修改细节，只需要对冲突的文件，要么选择保留master分支版本，要么选择替代为所merge分支的版本。

这样的场景是很常见的，比如文件A被另一个分支完全重构了，而我本地的分支只是写了一些修改来调bug，最后希望替换成完全重构的那个版本时。

这就要用到`checkout`命令。

当我们执行完merge命令，产生一堆冲突时，可以用checkout命令来解决具体某个文件的版本选择。

```sh
git checkout --ours [filepath]
# 抛弃要merge的分支的版本，保留原版本
```

```sh
git checkout --theirs [filepath]
# 抛弃原版本，全部替换成要merge分支的版本
```

有一个很重要的提示，在没有commit之前，可以来回切换。

## 回滚到某一个版本

上述的checkout命令其实也是一种回滚命令，但是更常用的则是reset。

有时候本地不知道修改了什么，导致从远程pull分支时，有很多conflict。有一个很懒的方法就是放弃本地的修改，然后再pull（注意，除非你的修改足以忽略，才可以使用这种方法，不然放弃了是不能复原的，所以此方法要慎重使用！）。

```bash
git reset --hard
git pull ...
```

所谓版本[commit]，可以用git log命令（或者上github）查看commit对应的hashcode，或者是HEAD这种特殊标识符。

reset命令有3种方式：
 - `git reset -–mixed`：此为默认方式，不带任何参数的git reset，即时这种方式，它回退到某个版本，只保留源码(修改但未提交的本地内容），回退commit和index信息
 - `git reset -–soft`：回退到某个版本，只回退了commit的信息，不会恢复到index file一级。如果还要提交，直接commit即可
 - `git reset -–hard`：彻底回退到某个版本，本地的源码也会变为上一个版本的内容。

```sh
# Undoes all commits afer [commit], preserving changes locally
git reset [commit]
#Discards all history and changes back to the specified commit
git reset --hard [commit]
#回退所有内容到上一个版本 
git reset HEAD^ 
#回退a.py这个文件的版本到上一个版本 
git reset HEAD^ a.py 
#向前回退到第3个版本 
git reset –soft HEAD~3 
#将本地的状态回退到和远程的一样 
git reset –hard origin/master 
#回退到某个版本 
git reset 057d 
#回退到上一次提交的状态，按照某一次的commit完全反向的进行一次commit 
git revert HEAD 
```

如果我们某次修改了某些内容，并且已经commit到本地仓库，而且已经push到远程仓库了。
这种情况下，我们想把本地和远程仓库都回退到某个版本，该怎么做呢？
前面讲到的git reset只是在本地仓库中回退版本，而远程仓库的版本不会变化。
这样，即时本地reset了，但如果再git pull，那么，远程仓库的内容又会和本地之前版本的内容进行merge。
而且如果不pull直接push，则会提示错误，版本比远程仓库的要低。

这并不是我们想要的东西，这时可以有3种办法来解决这个问题：


1：用--force参数

```bash
#强行force   
git push --force origin master
```

这样有风险，会删除掉其他人同时在进行的所有提交，而且其他人pull也会有风险，因为本地仓库版本比远程要高，只能让他们执行checkout覆盖。

2: 用revert

revert命令是对某一次commit进行逆操作，把相应的改动都删除掉（注意这点!)，并commit。所以假如提交了commit 1和2，想回到1，则执行revert 2就可以了。

```bash
git revert [commit]
```

这个方法严格来说，也不能回到某一次commit。

3：reset+手工
reset回到某个版本，然后修改所有敏感的文件，执行pull操作，此时会产生冲突，然后用checkout覆盖法就可以了。
或者
reset回到某个版本，手工拷贝所有敏感文件，执行rm操作，checkout，再复制回来重新commit。


## 简单解释revert 和 rest的区别

 - git revert 是撤销某次操作，此次操作之前的commit都会被保留。
 - git reset 是撤销某次提交，但是此次之后的修改都会被退回到暂存区。

```
#具体一个例子，假设有三个commit， 
git st
commit3: add test3.c
commit2: add test2.c
commit1: add test1.c
# 当执行git revert HEAD~1时， commit2被撤销了
git log可以看到：
commit1：add test1.c
commit3：add test3.c
git status 没有任何变化
# 如果换做执行git reset --soft(默认) HEAD~1后，运行git log
commit2: add test2.c
commit1: add test1.c
运行git status， 则test3.c处于暂存区，准备提交。
# 如果换做执行git reset --hard HEAD~1后，
显示：HEAD is now at commit2，运行git log
commit2: add test2.c
commit1: add test1.c
运行git st， 没有任何变化
```



