title: iOS报表技术（三）
date: 2015-02-08 18:32:48
tags:
---

这次讲讲曲线的生成，曲线生成有好几种方法，这里使用贝塞尔曲线，也即谷歌的开源报表库 CorePlot 使用的方法

https://www.particleincell.com/2012/bezier-splines/

这一方法的核心是算出所有控制点

![控制点](/img/chart-5.png)

从上图可以看到控制点才是曲线的灵魂，关于贝塞尔曲线的更多内容这里不在赘述，大家可以看百科。

这样就引发了一个问题，当几个点的间距比例过大就会出现“过量”问题，画出的曲线不是很正常

![过量](/img/chart-4.jpg)

解决的方法非常简单，就是通过在比例较大的两点之间做一条直线，在这条直线上虚拟几个数据点作为辅助，如此就解决了问题

```Objective-C
CGFloat k = deltaY / deltaX;
CGFloat startX = 0;
while ( startX < deltaX ) {
    CGPoint currentPoint = CGPointMake(firstX + startX, round(firstY + k * startX));
    [interpolationPoints addObject:[NSValue valueWithCGPoint:currentPoint]];
    startX += interploation;
}
```

