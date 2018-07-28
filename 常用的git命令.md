#### 常用的`git`命令

学习和工作中都是用的`git`，整理一些常用的`git`用法

* `git init` `git add .` `git commit -m " "`

一个项目要是用git，首先要git初始化(`git init`)，当文件修改之后用`git`追踪全部文件(`git add .`)，然后写入这次文件变更的信息(`git commit -m "you_message"`)

* `git pull` `git push`

当`commit`以后，我们要把我们本地的更改更新到`GitHub`上去(`git push`)，如果有同事已经更新了代码，你需要先(`git pull`)，如果没有冲突，然后就可以`push`上去你本地的代码，如果有冲突，需要解决冲突之后，然后commit信息后，再(`git push`)
其实工作当中我们需要先fork公司代码到自己仓库，然后把自己仓库的代码clone到本地，更改代码并本地commit后，先获取公司最近到代码，(`git pull name branch`) name 是在你本地中公司远程的名字，branch是要更新公司代码的分支，更新好以后，再上传到自己仓库(`git push name branch`) name 是你本地中自己仓库远程的名字，branch是要推本地哪个分支，推上去之后，就可以提PR了，老大检查完代码，就可以把你改的代码merge到公司仓库了。

* `git checkout -b ` `git checkout ` `git merge ` `git branch -d `

当新增一个需求时，我们可以在`master`分支上新建一个dev分支(`git checkout -b dev`)，在`dev`分支上开发，开发并测试好了以后，切换到`master`分支(`git checkout master`)，然后再把`dev`分支的合并到master分支(`git merge dev`)，可以删除`dev`分支(`git branch -d dev`)

* `git remote -v` `git remote add xxx ssh`

第一个是查看现在这个项目关联了多少远程，第二个是增加关联到仓库，xxx自己规定名字，ssh是要关联到ssh地址。
