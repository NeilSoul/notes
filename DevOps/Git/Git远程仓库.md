
## 查看远程仓库

```bash
git remote
git remote -v
# -v 参数会显示更详细信息
```

## 删除远程仓库

```bash
git push origin :[remote branch name]
```

## 多个远程仓库
```bash
git remote add [远程仓库名] [url]
git remote -v
```
更改
```bash
git remote rename [远程仓库原名] [远程仓库现名]
git remote set-url [远程仓库名] [新url]
git remote remove [远程仓库名]
```

