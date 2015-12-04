title: 使用classy订制UI样式
date: 2015-04-26 19:30:27
tags:
---

苹果的 UIApprearance 给予了我们一个统一订制样式的方案，但实际上在很多情况下并不好用。

在 UIApprearance 之前实际上已经有一个机智的小伙伴写了个叫 NUI 的类库，里面的原理也很简单，用类CSS定义样式，用HOOK大法劫持住系统的 moveToWindow 等方法，然后使用Visitor（Render）去装修我们的View等元素，整个类库安全无污染，放进去就能用。

之后又有了很多类似的库，较有影响力的 Classy 就是今天的主角，把玩了一番后觉得很有意思。

Classy 定义样式的语法相当简单，但又非常强大。

支持常量

```
$main-color = #35a3ee
$background-color = #f4f4f4
$nav-background-color = #fcfcfb

$warm-gray-color = #dfdfdf
$light-gray-color = #999999
$normal-gray-color = #666666
$dark-gray-color = #333333

$h1 = 36
$h2 = 30
$h3 = 24
$h4 = 18
$h5 = 14
$h6 = 12
```

父类匹配，所有父类为 UIViewController 的 ViewController 的 View (有点绕。。)

```
^UIViewController > UIView {
	background-color: $background-color
}
```

嵌套对象样式

```
UIButton.return_current {
	background-color: clear
	layer: @{
		corner-radius: 12
		border-width: 1
		border-color: $main-color
	}
}
```

text attributes 支持，简直神了

```
UINavigationBar {
	tin-color: $main-color
	background-color: $nav-background-color
	translucent: NO
	title-text-attributes: @{
		font: $title-font
		foreground-color: $dark-gray-color
		shadow: @{
			shadow-offset: 0, -1
			shadow-color: clear
		}
	}
}

```

Live Reload

实时加载修改，给设计妹子演示的时候，妹子看的很开心啊。

一些懒人必备的语法，帮助样式的复用，不得不佩服作者的巧妙心思。

```
UICollectionView {
  background-color #a2a2a2
  
  > UICollectionViewCell {
    clips-to-bounds NO
    
    UILabel {
      text-color purple
      
      &.title {
        font 20
      }
    }
  }
}
```

当然并不局限于样式，一些订制的功能还需要深入挖掘。








