* 查看进程 ps aux | grep xxx    或者 ps -ef    或者ps aux
* 查看npm全局安装的东西   npm ls -g —depth=1
* 解压 .tgz => tar -zxvf  .tar.xz => xz -d  .tar.gz => tar -xzvf  .tar => tar -xvf
* touch创建文件  mkdir创建文件夹
* 删除文件夹 rm -rf
* export PATH=<mongodb-install-directory>/bin:$PATH
* PATH变量决定了shell 将到哪些目录中寻找命令或程序。如果要执行的命令的目录在 $PATH 中，您就不必输入这个命令的完整路径，直接输入命令就可以了。添加路径export PATH=$PATH:/some/directory
* 进入zshrc   vi ~/.zshrc
* 查看3000进程的PID    lsof -i:3000
* 改文件夹名字 mv oldname newname
* 查看本地ssh公钥 cd .ssh  cat id_rsa.pub
* etc下的是配置，usr下的是可执行文件
* 从本地传到服务器三种方式，ftp ssh scp   
* scp /Users/maximilian/Desktop/server.zip root@xxx:/server
* scp -r ………………(传文件夹）

Vim操作
*  保存并退出 :wq
* 不保存退出 :q!
* 命令模式（默认）下进入插入模式 i  退出插入模式esc
* 在命令模式下 x 删除光标后面一个字符
*
