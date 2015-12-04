title: 开始学习Ruby
date: 2015-05-03 20:43:25
tags:
---

一年学习一门新语言是个好习惯，去年是红的发紫的 Swift，今年则是Ruby。

为什么要学Ruby?
1. Ruby可以作为工作的自动化脚本工具，合理使用可以极大的提高工作效率。
2. Ruby有非常好的DSL能力，可以更好的实践元编程，有很强的创造力。
3. Swift 和 Ruby 有非常多的相似之处，事半功倍
4. 公司有很好的Ruby氛围。

目前整理了几本书，慢慢看
1. 《Programming Ruby》
2. 《Ruby从入门到精通》
3. 《松本行宏的程序世界》
4. 《Ruby Under a Microscope》

文档工具
Dash + Alfred

文本工具
Textmate/Sublime

看了《Programming Ruby》前两章，做了下笔记：

Ruby 有一个很好用的交互式控制台 irb

```
%ruby
puts "Hello world"
^D
Hello world
```

#### constructor
和 Swift 的 init 方法一样，Ruby的对象使用 new 构造器进行初始化。

#### Method

```ruby
def say_goodnight(name)
	result = "Good night, " + name
	return result
end
```

默认的，方法内带有一个隐藏参数 result，这个特性 object-pasical 也有

语句不需要分号结尾，懒人福音，这也是 Ruby 的哲学所在，能少写代码，就尽量少写。代码越少，意味着思维越清楚。

表达式插值 expression interpolation

```
result = "Good night, #{name}"
```

怎么样，想起 Swift 了吧 :)

#### 变量

```
$greeting = "hello" # 全局变量
@name = "Prudence"  # 实例变量
@@hello = "hello" 	# 类变量
```

#### 数组

```
a = [1, 'cat', 3.14, nil]
```

这里的 nil 和 Swift 一样，并不是空地址 （0x00000000）而是一个用来指示空的对象。

快捷创建字符串数组

```ruby
a = [ 'ant', 'bee', 'cat', 'dog', 'elk']
# 等价于
a = %w( ant bee cat dog elk )
```

#### 字典

```
inst_section = {
	'cello' => 'string',
	'oboe' 	=> 'woodwind'
}
```

如果键对应的值为空，则返回nil
如果希望默认值为0，则可以

```
histogram = Hash.new(0)
```

#### 控制结构

```ruby
if count > 10
	puts "try again"
elsif tries == 3
	puts "you lose"
else 
	puts "enter a number"
end

while weight < 100 and num_pallets <= 30
	pallet = next_pallet()
	weight += pallet.weight
	num_pallets += 1
end
```

语句修饰符 statement modifiers

```
if radiation > 3000
	puts "Danger, Will Robinson"
end

# 等价于

puts "Danger, Will Robinson" if radiation > 3000

```

#### 正则表达式

Ruby内建正则支持，所以具有强大的文本处理能力

```
if line =~ /Perl|Python/
	puts "..."
end

line.sub(/Perl/, 'Ruby')  # 替换第一个匹配的
line.gsub(/Python/, 'Ruby') # 替换所有的
```

#### Block, 迭代器

花括号活着 do..end 都可以包裹一段代码作为 Block

```
{ puts "Hello" }

do
	club.enroll(person)
	person.socialize
end
```

有点类似 Swift 的尾行闭包，最后一个参数假如是 block, 则可以写在方法执行体外面作为参数进行传递，而 Ruby 则称为 方法关联，协同例程 

```
animals = %w( ant bee cat dog elk )
animals.each { |animal| puts animal }
```
再看看 each 方法

```
def each
	for each element # 伪代码
		yield(element)
	end
end
```


