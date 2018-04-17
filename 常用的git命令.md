#### 常用的`git`命令

学习和工作中都是用的`git`，整理一些常用的`git`用法

* `git init` `git add .` `git commit -m " "`

一个项目要是用git，首先要git初始化(`git init`)，当文件修改之后用`git`追踪全部文件(`git add .`)，然后写入这次文件变更的信息(`git commit -m "you_message"`)

* `git pull` `git push`

当`commit`以后，我们要把我们本地的更改更新到`GitHub`上去(`git push`)，如果有同事已经更新了代码，你需要先(`git pull`)，如果没有冲突，然后就可以`push`上去你本地的代码，如果有冲突，需要解决冲突之后，然后commit信息后，再(`git push`)

* `git checkout -b ` `git checkout ` `git merge ` `git branch -d `

当新增一个需求时，我们可以在`master`分支上新建一个dev分支(`git checkout -b dev`)，在`dev`分支上开发，开发并测试好了以后，切换到`master`分支(`git checkout master`)，然后再把`dev`分支的合并到master分支(`git merge dev`)，可以删除`dev`分支(`git branch -d dev`)
