#### 在node与浏览器中，JS的事件循环(Event Loop)

今天群里讨论一道题目，执行的结果是什么

```
console.log(1)

setTimeout(() => {console.log(2)})

Promise.resolve().then(() => {console.log(3)}).then(() => {console.log(4)})

console.log(5)
```
结果是 `15342`

之前只是明白JS事件循环的大概，今天想彻底把它弄懂，查了一些资料，理了一下思路。

##### 为什么JS会有事件循环

因为Javascript是单线程运行的。为什么必须是单线程？因为在JavaScript诞生之初就是为了在浏览器中运行操作DOM，如果是多线程，两个线程同时操作一个DOM，那么浏览器会灰常纠结，不知道该听谁的。

单线程会有一个问题，就是顺序执行，碰到耗时的任务，会一直等待下去，没有效率。所以有了事件循环，把异步的任务添加到一个事件队列中去，主进程继续执行下面的代码，等到异步任务处理完成，才会通知主进程执行异步任务的回调，大大提高了效率。如果部署得好，JavaScript程序是不会出现堵塞的，这就是为什么`NodeJS`平台可以用很少的资源，应付大流量访问的原因。

##### 浏览器中的事件循环

我们都知道浏览器是多进程的（一个进程中可以有多个线程，JavaScript是单线程的），其中有个进程对于我们前端工程师来说是很熟悉的，那就是渲染进程，也叫浏览器内核。渲染进程包含了几个进程，如下

* `GUI渲染线程` （负责解析 HTML CSS 渲染成页面）
* `JS引擎线程` （负责执行JS代码，也就是我们常说的V8引擎）
* `事件触发线程` （也就是我理解的触发事件队列的，一些异步的操作（setInternal、setTimeout、异步请求等）完成后的回调都放在这里，等待JS引擎线程执行完代码后就开始执行这里面的回调函数）
* `定时触发器线程` （用于setInternal与setTimeout的计时，时间到了，就会把他们的回调放入事件队列中，等待JS主进程执行完代码空闲时执行）
* `异步http请求线程` （就是我们常说的异步请求，完成后，也是把它的回调放入事件队列）

上面的代码为什么会输出 `15342` 呢，按道理来说，`setTimeout`函数是异步，会把它放入事件队列，继续执行下面代码，应该是 `13452`,
这就要说一下在JS中，还有一种叫`microtask`的东西，这个小东西是放在主任务执行完之后，事件队列中的回调函数之前执行。`Promise`就属于是`microtask`。

也就是主任务开始执行，碰到异步任务的放入事件队列中，继续执行主任务，主任务执行结束，立即执行`microtask`,然后执行事件队列中的回调函数。

##### NodeJS中的事件循环

node中的事件循环和浏览器会有不同，看下面代码，输出结果是什么

```
setTimeout(() => console.log(1))

setImmediate(() => console.log(2))

process.nextTick(() => console.log(3))

Promise.resolve().then(() => console.log(4))

(() => console.log(5))()
```
node官网上有对`Event Loop`的[介绍](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/)

![](./img/event-loop.png)

这张图中，node中事件循环包括六个阶段，当主任务执行完之后，会依次进入这六个阶段

* `timers` （处理setTimeout和setInterval的回调函数，主线程会检查一下当前时间，是否满足定时器的条件。如果满足就执行回调函数，否则就离开这个阶段。）
* `I/O callbacks` （执行一些错误的回调和上轮应该在poll执行而没有执行的回调）
* `idle prepare` （这个阶段只供 libuv 内部调用，我们可以先忽略）
* `Poll` （等待还未返回的 I/O 事件，并执行回调，等待的过程中如果有timer到时间，那就进入timer阶段,如果有setImmediate，直接进入check阶段）
* `check` （该阶段执行setImmediate()的回调函数）
* `close callbacks` （该阶段执行关闭请求的回调函数，比如socket.on('close', ...)）

上面代码 setTimeout放入事件循环队列中（虽然没有设置延迟的时间，但是它是异步函数，还是要放入事件循环队列中），接着 `setImmediate` 也是要放入事件循环队列，接着  `process.nextTick` （它y优先于`promise`执行，在主任务执行完立即会执行它），接着 `Promise` （不用多说了，但是会在 `process.nextTick` 之后执行，优先级不如`process.nextTick`高），接着是立即执行的同步函数，打印 5，紧接着执行 `process.nextTick` ，打印 3 ，紧接着执行 Promise，打印 4。

接下来就进入事件循环了，首先进入 `timers` 阶段，检查后发现已经到时间，执行回调，打印 1 ，然后是执行下面的阶段，等到了`check` 阶段，执行 `setImmediate` 的回调，打印 3 。

这只是一种情况，还有很多种情况可以自行试一试。这样就会对node的事件循环机制有更深的了解。

初步的认识，如有错误，还请指正～

###### 参考文章
`https://funteas.com/topic/5a64e9482630e6f31583701d`
`http://www.ruanyifeng.com/blog/2018/02/node-event-loop.html`
`http://www.ruanyifeng.com/blog/2014/10/event-loop.html`
`https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/`
