- 查看进程 `ps aux` `ps aux | grep xxx` `ps -ef`

- 解压 `.tgz => tar -zxvf`  `.tar.xz => xz -d`  `.tar.gz => tar -xzvf`  `.tar => tar -xvf`

- 创建文件 `touch xxx`  创建文件夹 `mkdir xxx`

- 删除文件夹 `rm -rf`

- 查看当前路径 `pwd`

- PATH是一个变量，它决定了 shell 将到哪些目录中寻找命令或程序。如果要执行的命令的目录在 $PATH 中，您就不必输入这个命令的完整路径，直接输入命令就可以了。添加路径export `PATH=$PATH:/some/directory`  `echo $PATH` 来看看到底有哪些目录被定义出来了 echo有『显示、印出』的意思，而 PATH 前面加的 $ 表示后面接的是变量，所以会显示出目前的 PATH 。如果当命令xx没有在PATH中，需进入xx所在的文件，执行./xx才可执行

- alias lm='ls -al' 代理命令

- zsh脚本文件位置  `~/.zshrc`

- 查看3000进程的PID `lsof -i:3000`

- 修改文件夹名字 `mv oldname newname`

- 移动bin下的xx文件到root下 `mv /bin/xx /root`

- 复制bin下的xx文件到root下 `cp /bin/xx /root`

- 本地ssh公钥位置 `~/.ssh`

- etc下的是配置，usr下的是可执行文件

- 从本地传到服务器三种方式，ftp ssh scp   

- scp /Users/maximilian/Desktop/server.zip root@xxx:/server   scp -r ………………(传文件夹）

- CPU的重点是在进行运算与判断，那么要被运算与判断的数据是从哪里来的, CPU读取的数据都是从主内存来的！ 主内存内的数据则是从输入单元所传输进来！而CPU处理完毕的数据也必须要先写回主内存中，最后数据才从主内存传输到输出单元.
