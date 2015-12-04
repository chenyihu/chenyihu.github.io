title: Today 扩展实战
date: 2014-11-30 22:02:40
tags:
---

今天花了一下午，把Today扩展给集成到了轻卡项目里。

总结一下

1. Today 扩展可以使用 AutoLayout 来进行自动布局，并且布局过程是动画的。

2. 可以通过 widgetMarginInsetsForProposedMarginInsets 代理方法来设定边界距离。

3. 可以通过 preferredContentSize 来设置高度，宽度被自动忽略。

4. widgetPerformUpdateWithCompletionHandler 可以给一个后台刷新准备数据的机会，并提供系统回调。

5. 扩展和主应用之间通过共享容器来数据通信，需要注意的是，共享容器在应用删除后仍然会存在。

6. 扩展和主应用可以通过共享容器来分享CoreData数据，原理同上。

7. 可以通过 framework，或者 CocoaPods 来分享代码。

8. 如果是非 framework 方式共享代码，整个应用的体积会增大不少，因为扩展也可以认为是单独的程序。

9. 大部分的API都可以被扩展所使用，扩展不能使用的一些API也相应的被标记。

10. - (void)viewWillTransitionToSize:(CGSize)size withTransitionCoordinator:(id<UIViewControllerTransitionCoordinator>)coordinator  是一个隐藏的方法，用来动画设置扩展高度