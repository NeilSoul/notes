## 配置

```bash
# 提交代码的log里面会显示提交者的信息
git config --global user.name [username]
git config --global user.email [email]

# 在git命令中开启颜色显示
git config --global color.ui true
```

`--global`是针对当前user，`--system`则是对所有user的配置。

## 使用Git的最简单Example

```bash
git init
git remote add origin [git repo url]
git pull origin master
# 编辑操作之后, git branch , git merge, git branch -d,等等
git add .
git commit -m "comment"
git push origin master
```

## 删除与忽略

常用方法是先用`git rm`命令删除你想忽略的文件（如果已经add进来），然后把规则写在`.gitignore`上。

```
vi .gitignore
```

`.gitignore` is a file that describes what should be ignored in git pushing and pulling operation. For example, the config on individual environment should be different. So it cant be changed when you push you modification.

```
# global rules
*.sln
*.mdb

# specified folder or files
Debug/
config/config.txt
```

It's worth noting that `.gitignore` only apply to that files not added to repository. If it had been added into respository, u should `git rm` to remove it.


## 免登录

当你拷贝.Git仓库地址时，会区分ssh和https。我们要提交时，需要用户校验信息，下面是两种免登陆的办法。
### ssh key (ssh)
Generating a new SSH key
```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
# Creates a new ssh key, using the provided email as a label
Generating public/private rsa key pair.
```
When you're prompted to "Enter a file in which to save the key," press Enter. This accepts the default file location.Or you can specify a filename.
```bash
Enter a file in which to save the key (/Users/you/.ssh/id_rsa): [Press enter]
```
At the prompt, type a secure passphrase.
```bash
Enter passphrase (empty for no passphrase): [Type a passphrase]
Enter same passphrase again: [Type passphrase again]
```
Ensure ssh-agent is enabled:
```bash
# start the ssh-agent in the background
eval "$(ssh-agent -s)"
Agent pid 59566
```
Add your SSH key to the ssh-agent. If you used an existing SSH key rather than generating a new SSH key, you'll need to replace id_rsa in the command with the name of your existing private key file.
```bash
ssh-add ~/.ssh/id_rsa
```
Add the SSH key to your **GitHub account**.

### https
1. mac os , keychain, it will only ask u to input username and password at first time.
2. GitDestop, add user name.


## free git space

1. GitCafe <https://gitcafe.com/>
2. bitbucket <https://bitbucket.org/>
3. <https://coding.net> , china, free private space

