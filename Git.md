#### 常用的`git`命令

- `git init` git 初始化

- `git config --global user.name "xxx"` 配置用户名

- `git config --global user.email "xxx@xxx.com"` 配置邮件

- `git add` git add . 把所有变化提到暂存区 git add xxx 把制定文件提到暂存区

- `git status` 查看当前文件状态

- `git commit -m ""` 提交更新

- `git commit -am 'xxx'` 将add和commit合为一步, 但只能cover到已被track的文件

- `git show commit_id` 显示某个提交的详细内容

- `git log` 查看commit日志

- `git reset --hard commit_id` 回退到某个commit

- `git revert commit_id` 进入到某个commit

- `git remote -v` 查看本地关联的远程仓库

- `git remote add xxx 地址` 关联远程仓库，名字为xxx

- `git remote rename oldname newname` 修改远程仓库的名字

- `git remote rm name` 删除名字为name的远程仓库的关联

- `git pull name branch` 拉取名字为name的远程仓库的branch分支

- `git push name branch` 推送名字为name的远程仓库的branch分支

- `git checkout -b branch [remote/master]` 新建并进入一个名字为branch的分支 可选参数指在某个分支基础上新建

- `git checkout branch` 切到名字为branch的分支

- `git branch -D branch` 删除名字为branch的分支

- `git branch -a` 查看所有分支 包括本地和远程

- `git clone 地址` 克隆项目到本地

- `git fetch [name] [branch]` 将获取远程仓库的更新取回本地，取回的代码对本地的开发代码没有影响，无参数时默认取所有

- `git merge branch` 把branch分支合并到当前分支

- `git push name :branch` 删除名字为name的远程的branch分支

  `git rebase -i HEAD~x` `git rebase -i commi_id` (commi_id不参与合并的) 合并多个commit, pick改为s, 如有冲突，解决以后继续 `git add .` `git rebase --continue` 取消合并 `git rebase --abort`

- `git tag name [commit_id]` 增加名字为name的tag, commit_id制定commit处打tag

- `git tag` 查看所有tag，按字母排序

- `git tag -d name` 删除名字为name的tag

- `git push origin tagname` 把名字为tagname的tag推到远程

- `git push --tags` 把所有tag推送到远程仓库

- `git push origin :refs/tags/<tagname>` 删除远程tag

- 新生成 ssh key `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`
