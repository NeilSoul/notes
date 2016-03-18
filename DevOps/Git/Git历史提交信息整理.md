
如何让Git项目的commit graphs更清晰？

## 如何整合所有的历史commit

1: 使用rebase
```bash
git rebase --root
git push -f origin master
```
缺点是要等待很久，而且可能有冲突。

2:使用空白分支（不带commit，作为暂存区）
```bash
git checkout --orphan newbranch
git add -A
git commit
git branch -D master
git branch -m master
git push -f origin master
```
注意到使用了push --force，所以会带来隐患，其他人的版本就貌似是“新版”，而且依然保留历史commit。所以解决方法是，约好其他人删掉.git信息，重新再pull项目。

## 使用rebase让树的脉络更清晰

`pull --rebase`,用来消除pull的分支节点。

`merge --squash`, 消除分支上的commit信息。