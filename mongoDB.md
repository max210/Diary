- docker 安装
`docker run -d -p 27017:27017 -p 28017:28017 -e AUTH=no tutum/mongodb`

- 手动安装
etc/mongo.conf:
```
dbpath=/mongodb/data
logpath=/mongodb/logs/mongo.log
logappend=true
journal=true
quiet=true
port=32688
fork=true
mongoose
```

- 启动  mongod -f /usr/local/etc/mongod.conf

- 查看有哪些数据库 show dbs

- 创建一个名字为 demo 的数据库并进入 use db demo

- 查看当前数据库的集合 show collections

- 创建一个名字为user的集合 db.createCollection(“user”) 或者 db.user.insert({id: 123, name: ‘hello’})

- 导入集合 mongoimport -d demo -c user --file 拖进去

- 命令行导入数据包 mongoimport -h 1270.0.01 —port 27.17 -d db_demo -c users —file 文件地址

- 删除数据库 db.dropDatabase()

- 删除集合 db.user.drop()

- 查看当前集合的所有数据并格式化 db.user.find().pretty()

- 数据备份 mongodump -h dbhost -d dbname -o dbdirectory
-h：MongDB所在服务器地址，例如：127.0.0.1，当然也可以指定端口号：127.0.0.1:27017
-d：需要备份的数据库实例，例如：test
-o：备份的数据存放位置，例如：c:\data\dump，当然该目录需要提前建立，在备份完成后，系统自动在dump目录下建立一个test目录，这个目录里面存放该数据库实例的备份数据。
