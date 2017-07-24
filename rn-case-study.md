使用 React Native 重写大型 Ionic 应用后，我们想分享一下这八个经验
===

本文的内容是关于 React Native 重写的经验分享，基于 React Native 重写 Ionic 应用[Growth](https://github.com/phodal/growth-v2) 过程中遇到的一些坑。

Growth 是一款专注于Web开发者成长的应用。其 1.0 和 2.0 主要使用 Ionic 实现，Ionic 1.x 的主要问题是 Angular 1.x 已经落后了。而 Ionic 2.x 则在启动的性能上不是让人满意——其实在开源方面，我是中 HDD（热闹驱动开发）的一员。

在重写的过程中，我们错误估计了其开发效率与 Ionic 2.x 是接近的，我们以为会差上个 0.2 倍左右的差距——**上手新的框架的学习成本**。但是实际上这个差距可能是在 0.5~1.0 倍之间，毕竟要填的坑太多了，以至于在中途的时候让人想放弃。

最后，我们花了两三个月的时间才重写完这个应用。在 APP 发布的这几天里，顺便写了篇文章分享一下经验：
 
 - 你遇到的问题，别人基本到遇到过
 - 版本间差异太大，导致下游配套
 - 新的组件坑更多
 - 大部分时间，你都是在重写 UI
 - 最麻烦的地方，其实是搭建环境
 - 你看的文档可能有问题
 - 有时候真机才能反映问题
 - 尽早尝试 Release 0.0.1
 - 记得记录崩溃问题

幸运的是，作为一个开源应用，你可以看到这些坑是如何解决的。

你遇到的问题，别人基本到遇到过
---

你遇到的问题，别人基本到遇到过，要么就是你的**关键词不对**。

这一点实际上与 React Native 无关，只是在编写应用的过程中，遇到一些奇怪的问题。而尽管我第一时间使用了 Google 来搜索，但是并不能第一时间找到合适的答案。因为在这个领域里，我算是半年新手，总会错失一些关键词。

而遗憾的是，Google 不一定能第一时间帮你解决问题，有些问题在官方的 issues 里，但是没有被索引。因此，如果 Google 不到结果，请找官方的 issues，或者源码。

如果只是一般的应用，那么你遇到的问题，大部分人也都遇到过。除非，你是在写一些原生的组件，遇到一些莫名其妙地问题。

版本间差异太大，导致下游配套
---

开始编写 Growth 的时候，使用的 React Native 的版本是 0.42。在Growth 3.0 里面，使用了一些长的列表，如 awesome 列表，导致性能上不是很理解。在看到 React Native 0.43+ 之后，便升级到了 React Native 0.44。

整个升级过程中，看上去很容易：

1. 修改 package.json 中 react-native 的版本从 ^0.42.0 为 ^0.44.3
2. 修改 package.json 中的 react、react-dom 等组件版本从 15.4.2 变为 16.0.0-alpha.6

然而新版本里的类型检测 prop-types，已经变成了一个独立的组件，这就意味着我需要修改所有相关的代码。要不就会遇到这样的问题：

```
Warning: View.propTypes has been deprecated and will be removed in a future version of ReactNative. Use ViewPropTypes instead.
```

而，这意味着所有的第三方组件都需要修改。这并不是一件容易的事，这会导致遇到一系列的问题，如我的持续集成会在 Travis CI 出现问题。

幸运的是，我使用的原生组件比较少，因此也没有遇到一些组件不能支持新版本的问题。而对于那些库来说，他们可能是这样子的 README：

```
if on react-native < 0.40: npm i react-native-xx@0.4
if on 0.42 >= react-native >= 0.40 npm i react-native-xx@0.6
if on 46 >= react-native >= 0.44 npm i react-native-xx@1.0
```

除了此，对于我来说，那些使用 enzyme 写的测试也出现了问题，因为 enzyme 的开发者不想支持 alpha 版本的软件。

新的组件坑更多，如文档更新不及时
---

在 Android 版里的 WebView 可以支持 ``allowUniversalAccessFromFileURLs``，但是在使用的时候，文档并没有更新到这方面的内容。

WebView 问题

大部分时间，你都是在重写 UI
---

作为 Client 端，UI 风格一致问题

最麻烦的地方，其实是搭建环境
---

hello, world 都可能有问题 

直接使用 Node.js 的库，如 moment

你看的文档可能有问题
---

有时候真机才能反映问题
---

原生组件问题

尽早尝试 Release 0.0.1
---

记得记录崩溃问题
---
