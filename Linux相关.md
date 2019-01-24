- 查看进程 ps aux ps aux | grep xxx ps -ef

- 查看3000进程的PID `lsof -i:3000`

- 解压 .tgz => tar -zxvf  .tar.xz => xz -d  .tar.gz => tar -xzvf  .tar => tar -xvf

- PATH是一个变量，它决定了 shell 将到哪些目录中寻找命令或程序。如果要执行的命令的目录在 PATH 中，您就不必输入这个命令的完整路径，直接输入命令就可以了。添加路径export `PATH=​PATH:/some/directoryecho PATH` 来看看到底有哪些目录被定义出来了 echo有『显示、印出』的意思，而 PATH 前面加的 ​ 表示后面接的是变量，所以会显示出目前的 PATH 。如果当命令xx没有在PATH中，需进入xx所在的文件，执行./xx才可执行。source  ~/.bashrc 使其生效

- source 是bash shell的内置命令。点命令，就是个点符号 . 通常用于重新执行刚修改的初始化文档，如 .bash_profile 和 .profile 等等

- alias lm='ls -al' 代理命令

- zsh脚本文件位置  `~/.zshrc`

- 修改文件夹名字 `mv oldname newname`

- 移动bin下的xx文件到root下 `mv /bin/xx /root`

- 复制bin下的xx文件到root下 `cp /bin/xx /root`

- 本地ssh公钥位置 `~/.ssh`

- etc下的是配置，usr下的是可执行文件

- 从本地传到服务器三种方式，ftp ssh scp   

- scp -P xxx /Users/maximilian/Desktop/server.zip root@xxx:/server   scp -r ………………(传文件夹）

- CPU的重点是在进行运算与判断，那么要被运算与判断的数据是从哪里来的, CPU读取的数据都是从主内存来的！ 主内存内的数据则是从输入单元所传输进来！而CPU处理完毕的数据也必须要先写回主内存中，最后数据才从主内存传输到输出单元.

- 通信方式http websocket rpc(grpc) 消息队列

- 查看系统盘和数据盘硬盘实例：fdisk -l  查看硬盘的使用情况： df -h

- 修改登录服务器端口 修改默认端口 sudo vi /etc/ssh/sshd_config，Port: xxx  然后重启ssh服务 sodu service ssh restart，ssh登录ssh -p 3999 root@xxx

- 把原有的ip防火墙规则清空 sudo iptables -F  修改服务器的防火墙：sudo vi /etc/iptables.up.rules(vim /etc/sysconfig/iptables)  重启防火墙service iptables restart  关闭防火墙service iptables stop

- 建立软连接，ln -s /xx/xx/mongod /usr/local/bin/mongod

- 工具管理 apt-get
