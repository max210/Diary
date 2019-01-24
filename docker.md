* 查看版本  docker version
* 启动 docker  sudo service docker start
* 列出本地的 iamge 文件  docker image ls
* 删除特定的 image 文件  docker image rm [imageName]
* 将 image 文件从仓库抓取到本地  docker image pull [imageName]
* 新建一个容器，执行一次生个一个  docke run [imageName]  开启特定的容器 docker start [id]
* 查看当前的运行 docker ps
* 列出正在运行的容器  docker ls [--all]
* 立即杀死特定的容器  docker kill [container id]   妥善关闭特定的容器 docker stop [container id]

- Ubuntu 14.04/16.04 (使用apt-get进行安装)
```
$sudo apt-get update
$sudo apt-get -y install apt-transport-https ca-certificates curl software-properties-common
# step 2: 安装GPG证书
$curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
# Step 3: 写入软件源信息
$sudo add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
# Step 4: 更新并安装 Docker-CE
$sudo apt-get -y update
$sudo apt-get -y install docker-ce
```
- CentOS 7 (使用yum进行安装)
```
# step 1: 安装必要的一些系统工具
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
# Step 2: 添加软件源信息
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
# Step 3: 更新并安装Docker-CE
sudo yum makecache fast
sudo yum -y install docker-ce
# Step 4: 开启Docker服务
sudo service docker start
```
- 进入到容器
$ sudo docker exec -it  [docker-id or docker-name] /bin/bash
- 从容器中拷贝文件到宿主机
$ docker cp contaienr-name:/folder/file ./
- 从宿主机拷贝文件到容器
$ docker cp ./file container-name:/folder
