title: Swift 构造器总结
date: 2014-12-28 21:36:03
tags:
---

Swift 有一个很强大的特性就是安全
体现在对象上就是：不允许游荡的，自由指针，如何做到的呢？构造器就可以一窥其中的思路。

## 基本规则

### Rule 1. 可以在初始化过程中修改常量值

### Rule 2. 在值类型中定义指定构造器会覆盖掉默认的逐一构造器

### Rule 3. 每一个类都必须拥有至少一个指定构造器

## 构造器链

### Rule 4. 指定构造器必须调用其直接父类的的指定构造器

### Rule 5. 便利构造器必须调用同一类中定义的其它构造器

### Rule 6. 便利构造器必须最终以调用一个指定构造器结束

## 两段式构造过程

### Rule 7. 指定构造器必须保证它所在类引入的属性在它往上代理之前先完成初始化

### Rule 8. 指定构造器必须先向上代理调用父类构造器，然后再为继承的属性设置新值

### Rule 9. 便利构造器必须先代理调用同一类中的其它构造器，然后再为任意属性赋新值

### Rule 10.  构造器在第一阶段构造完成之前，不能调用任何实例方法、不能读取任何实例属性的值，也不能引用self的值

## 构造器的继承和重载

### Rule 11. Swift 中的子类不会默认继承父类的构造器

### Rule 12. 重载构造器时你没有必要使用关键字override

### Rule 13. 如果子类没有定义任何指定构造器，它将自动继承所有父类的指定构造器

### Rule 14. 如果子类提供了所有父类指定构造器的实现，它将自动继承所有父类的便利构造器

## 举几个栗子

```
class RecipeIngredient: Food {
    var quantity: Int
    init(name: String, quantity: Int) {
    	self.quantity = quantity // Rule 7
	    super.init(name: name) // Rule 8
	}
    convenience init(name: String) {
    	self.init(name: name, quantity: 1) // Rule 6
	}
}
```

![logo](/img/init-coder.png)

自从beta 6 开始，几乎所有的 UIView 类在实现 init 方法后都会报这个错误，这是 Rule 11 的体现。








