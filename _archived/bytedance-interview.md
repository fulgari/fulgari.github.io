---
title: "一年半前端参加字节社招的复盘"
date: "2022-05-14"
---

## 一面

这次的面试机会其实十分偶然，当时我只是更新了一下简历，并表明自己只是“看看机会”，但是却收到了 HR 的一面邀请。

因为工作地点离家近，当时抱着 ~~试一试~~ “流水不腐，户枢不蠹”的心态，粗略准备两天就上了战场。几乎是“裸面”。

面试官看上去很年轻，简单让我介绍了自己的情况之后，就开始了考察。

- 项目
  - 解决过哪些难点
- JS
  - 闭包的了解
  - React 的 class 和 hooks 的区别 (没答好，生命周期、static 函数等)
  - type 和 interface 的区别 (没答好，interface 支持声明合并，能够被 class 实现；type 可以给基础类型起别名)
- CSS
  - 对 BFC 的了解，如何设置 BFC (display: flex/inline/table-cell, positon: relative/absolute/fixed, overflow: hidden/scroll, float: left/right)
  - flex 布局有哪些属性，简写 flex: 1 的含义 (没答好，1 1 0%)
  - 页面中如何隐藏一个元素 (display, visibility, opacity+point-events, scale(0), left&top: -9999999px)
- 网络
  - HTTPS 和 HTTP 的区别 (没答好，多了 SSL/TLS 层，需要 CA 颁发的电子证书避免中间人攻击，需要先进行非对称加密传输随机数，然后再使用该随机数进行对称加密传输)
  - 缓存的 header (强缓存: cache-control:max-age & expires -> 弱缓存: Etag & Last-Modified & 304)
  - 有哪些 HTTP 的状态码 (400 Bad Gateway, 401 Unauthorized, 403 Forbidden, 404 Not Found, 500 Internal Server Error)
- 算法题
  - 通过一个包含访问符的字符串取出给定对象的对应属性值，如: 给定 `a` 对象，传入`'a.b.c'` 取出 a.b.c 对象的值。通过正则解决。
  - 实现异步函数链式调用 (Promise.then)
  - 存在两个数组，判断两个数组之间的三种关系: 包含、相等、被包含 (先排序，然后双指针遍历)

这次八股文啃老本，评价基础不好，好在算法 ~~在面试官提示下~~ 都做了出来，整个面试时长达 2h。

## 二面

五一后，面试过了，约了二面。

面试官做核酸去了，临时换了个面试官。不过算是按时开始了。

这次着重问了些个人项目的问题，包括协同编辑，还有一些个人关注的前端领域。

- JS
  - 协同编辑的实现方案 (CRDT)
  - webpack 为什么比 ESBuild 慢 (Webpack 查找依赖的链路较长，ESBuild 依赖了浏览器的 ESM 模块化，局部动态加载快，go 语言的实现)
  - useState 的状态更新（通常放在 `Promise.resovle` 批量微任务队列里更新，放在如 `setTimeout` 宏任务里就同步更新）
  - eventLoop 的概念，宏任务、微任务、nextTick 队列，异步任务，异步任务的优先级，异步任务的执行时机
- 算法
  - 实现一个 Promise.all （没做好，需要用一个 `result` 数组存结果，一个 `count` 在`promises[i]` 的 `then` 里计数并把结果存到 `result[i]` 中，`count` 等于 `promises` 的长度时，就输出 `result`——也可用 `reduce`，原理一样)
  - 继承方式有哪些，没做好，假设 A 继承 B
    1. 原型继承 (`A.prototype = new B()`)
    2. 构造函数继承 (`B.call(this)`)
    3. 组合继承 = 原型继承 + 构造函数继承
    4. `class` 的 `extends` 关键字用到的继承: 寄生组合继承

面了 1h ，网络遇到了一些问题，显示面试结束了。复盘并和 HR 沟通后，希望重新面试，希望更好地表现自己。

## 三面

二面次日，意外收到面试通过的通知。约了周五晚上的面试。

先介绍了自己的富文本编辑器项目，问了些文档数据存储结构、CRDT、合并数据等相关的问题。

- JS
  - 内容多的页面如何优化滚动性能
    - 对可中断的事件监听器设置 `passive: true` 使得事件监听流程可被打断，优先处理滚动事件，提高流畅度
    - 虚拟滚动，减少多余的 DOM 节点，提高性能
  - 通过哪些指标衡量渲染性能
    - Performance 查看火焰图分析
    - JS 埋点
  - 怎么避免重复渲染
    - `useMemo` 或 `React.memo` 缓存
    - 将状态放在子组件内部进行管理
    - 使用 MobX 或者 Redux 管理状态
  - SharedWorker 之间传输大体积数据
    - 不熟悉，说了些自己的认识，我说是通过流来实现的。
    - 正确答案是 [SharedArrayBuffer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SharedArrayBuffer)，实现在主线程和 worker 线程之间的共享内存。
  - WebAssembly 和 Worker 对性能的优化
    - 前者优化了一些 JS 瓶颈部分，比如使用底层语言对内存有更充分的控制
    - 后者主要就是处理一些可被并行计算的耗性能部分
- 算法
  - 传入一个对象，key 为模块名，value 为该模块的依赖数组。以数组形式输出模块的加载顺序 (没有依赖或者依赖全已满足即可输出)。
    - 递归遍历
    - 每次递归都记录上一次加载的模块，并在下一次处理中清除掉该已加载过的模块。

面试经历了 1.5h，聊了很多 web 的优化方面。算法在提示下才 debug 出来，做了半个多小时。

不过自我感受还是 ok 的，最后我问了一些关于团队所做的 to B 业务详情以及公司的情况。

工作一年半，相比当时校招的生涩，现在对自己多了一份自信，每次面试也都在见证着自己的成长。特此总结和分享。
