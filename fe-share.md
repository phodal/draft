在我的 Backbone、Vue、Angular、React 经验里，为什么 Angular 更吸引我
===

上周，知乎上有几篇关于 Angular 和 Vue 对比的文章。本来想着的是，这些文章倒是可以指导下新手，作一些技术选型。可遗憾的是，开始的文章失去了一些偏颇，后面的文章则开始了一些攻击性行为。慢慢的，整个知乎上便是充满了一些戾气，开始了无尽的网络暴力。

在讨论 Angular 之前吧，让我先分享一下之前使用这些 MV* 框架的经验。

在编写后台应用的时候，与可维护性相比，发现我们根本不在乎性能。

近来，在前端应用上，同意也有如此的事情发生了。

前端圈的更新速度
---

同样吧，在上周结束了《Expert Angular》的审校，这是第三本为 Packt 出版社审校的 Angular 的书。然后，先让我来讲个故事：一年前我开始审校的这本书，当时是基于 Angular 2 beta.4 写的，当时的书名叫 Mastering Angular 2。后来 Angular 2.0 正式版大改，作者们就重新改写代码，书名就改成了 Mastering Angular。完了 Angular 4 出来了，而 Angular 5 也进入了 Beta 版本，因此书名改叫成了《Expert Angular》。 ​​​​

Backbone
---

刚开始编写大型前端应用的时候，学习的是 Backbone。因为并没有一个好的 MVC 框架，在当时的情况下，仍然是最适合的选择。

对于我们而言，采用 Backbone + jQuery + Spring 有几个明显的问题：

### jQuery 的**散弹性架构**问题。

一个 Backbone.View 则需要“继承”（extend）从

对于一个复杂的应用，他则会出现一个 PageView，ListPageView 这样的页面，而 ListPageView 则“继承”自 PageView，PageView 则 “继承”自  Backbone.View。在一些复杂的情况下，还会有 SubListPageView 这样的情况。

而在新的 MV* 框架里，则可以使用模块化来解决问题。

### **前后端两次渲染的复杂度**。

Java 在后台渲染 Mustache，而 Mustache.js 则也使用同一个模板。我们所需要做的，便是在构建的时候，只需要用 require.js 将 Mustache 模板文件打包。

与今天的 React 后台渲染类似，API 以 JSON 的形式嵌入在 HTML 中。可与 React 的同构不一样的是，在 Mustache 和 Java 之间同步状态，并不是一件容易的事。

每当新加一个状态，便需要使用 Java 启用一个新的 API，这个时候即要修改前端的框架，又要修改大量的后台测试。

除了此， 我们还需要考虑到，用户刷新页面的情况。当用户由在产品详情页，刷新页面时，我们需要将一些数据，通过 URL hash 传递到后台，然后解析 blabla。完了，还要考虑将这个状态再传到前端。

React
---

我们是在 React 初期采用这个框架的，所以操作起来并不会像今天这么顺利。我们在实现原型引入 系统的时候，需要自己去实现一个又一个的组件，

Angular 
---

在新的项目里，采用 Angular 的原因是。

在移动端里，Cordova + Ionic 是一种最好的选择。而 Ionic 是基于 Angular 1.x，同样的我们也可以在 Web 端采用 Angular，这样做不仅从某种统一了技术栈，还实现了某一部分的代码复用。

我们的项目也从 Angular 1.x 迁移（重写一部分）到了 Angular 4.x，旧的应用还运行在旧有的 Angular 1.x 代码上，而新的应用则运行在新的系统上。

### 为什么 Angular 在选型里失去优势？

Vue
---


