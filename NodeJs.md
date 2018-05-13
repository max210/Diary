
* 对db er模型的理解 mongodb mysql 对应的node模块， nginx事务，os 算法 数据结构 设计模式 编译原理   《node in action》 《深入浅出Node.js》 《更了不起的Node.js》
* promise中有process.nextTick,promise先执行完在执行process.nextTick   这两个函数都是在六个循环阶段切换时执行  执行顺序简化为三个阶段，1.settimeout 2 异步IO 3 setimmediat（）   process.nextstick  promise在这三个阶段中间执行前者优于后者执行
* session 的 store 有四个常用选项：1）内存 2）cookie 3）缓存 4）数据库  
* iview的form表单，autoComplete属性是自动填充默认值到输入框里
* process.env 是读取系统环境变量的，比如你在启动服务的时候，设置环境变量为production或者development，那么在程序里面就可以通过process.env.ENVNAME获取，因为你在node命令窗口启动时没有设置相关的环境变量，所以就没办法获取到了，你可以试一下NODE_ENV=development node来启动命令窗口，然后应该就可以获取到了！https://segmentfault.com/a/1190000011683741
* 静态文件 app.use(express.static(path.resolve(__dirname, './dist')));
* node+vue https://juejin.im/entry/597006fa51882526426625d4
* vue整合后端http://www.jianshu.com/p/a5e63c033164
* 定义过程 === 预加载
* next() 继续执行下一个中间件， next(‘msg’) 直接到错误中枢
* express中 res.json() res.end() res.render() res.send()
* nodemon用来监视node.js应用程序中的任何更改并自动重启服务,非常适合用在开发环境中。
* 相应的缺点就是，单一node实例无法利用多核cpu，程序健壮性需要控制，防止因为一个错误造成整个服务的宕机，大量占用cpu的操作可能会引起异步io无法执行。不过，这个是以前的情况了，现在通过反向代理、负载均衡、clustor、web wokers等可以最大限度的降低这些问题的发生。nodejs还提供了与web workers相同的思路的解决方案，child_process（子进程，将计算分发到子进程，可以将大量的计算分解掉。然后，再通过进程间的事件消息来传递结果，这可以很好地保持应用模型的简单和低依赖。这就是Master-Worker的管理方式，这个方式很好地管理各个工作进程，创造出更好的程序健壮性。）

node插件
* nodemailer    nodemailer-smtp-transport  （阿里企业邮箱SMTP地址和端口）
* Passport项目是一个基于Nodejs的认证中间件
* moment.js 时间
* bcrypt 加密
* marked 支持markdown
* socket.io
* json-server 模拟json数据
* http-proxy 代理
