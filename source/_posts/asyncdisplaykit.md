title: AsyncDisplayKit：Paper的异步UI技术(一)
date: 2014-10-20 01:25:24
tags:
---

自从上次听了 Facebook 的技术分享之后，对它的异步 UI 技术非常的神往。
终于，在几个月后，看到 GitHub - ObjC 排行榜第一名的 AsyncDisplayKit ！

![logo](/img/asyncdisplaykit-logo.png)

AsyncDisplayKit 是 Facebook 的 Paper 团队在项目研发时的副产品
基于以下一些理由

1. 手势和物理动画相结合 (POP引擎) ，还要有非常流畅的效果。
2. 丢帧的效果令人沮丧，失败的预期会导致人焦虑。
3. 触控和动画都是至关重要的，需要60FPS的水准。
4. CA 引擎只作用于静态动画，无法处理手势和物理效果动画。
5. 用户可以在任意状态下操作界面，而静态动画往往是 fire-and-forget.
6. 当你决定不再需要渲染时Block主线程时，自然而然会想到在设计初始就用一个特殊的架构去实现。


导致丢帧的因素可能有哪些呢？

1. 拖延主线程超过5毫秒，将导致丢帧。
2. - layoutSubViews / - layoutSublayers
3. - drawRect:
4. UIView: init / addSubview: / removeFromSuperview / dealloc


异步UI设计了 Node 这样一个新的数据结构

Node 是类似于 View 的一种视图对象，但是它的渲染都不在主线程完成，所以一定不会卡住主线程，同时可以转换为 View, 借用官方的一个图来说：

![node](/img/asyncdisplaykit-node.png)

 今天就到这里，未完待续！






