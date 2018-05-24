* 查看版本  docker version
* 开启docker  sudo service docker start
* 列出本地对iamge文件  docker image ls
* 删除特定对image文件  docker image rm [imageName]
* 将 image 文件从仓库抓取到本地  docker image pull [imageName]
* 新建一个容器，执行一次生个一个  docke run [imageName]  开启特定的容器 docker start [id]
* docker ps 确认一下库已经正常运行起来
* 列出正在运行的容器  docker ls
* 立即杀死特定的容器  docker kill [container ID]   妥善关闭特定的容器 docker stop [id]
