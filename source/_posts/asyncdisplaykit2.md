title: AsyncDisplayKit：Paper的异步UI技术(二)
date: 2014-10-21 13:41:52
tags:
---

使用 Node 这一重新设计的类型，我们可以很方便的在后台进行界面渲染

```Objective-C

dispatch_async(_backgroundQueue, ^{
  ASTextNode *node = [[ASTextNode alloc] init];
  node.attributedString = [[NSAttributedString alloc] initWithString:@"hello!"
                                                          attributes:nil];
  [node measure:CGSizeMake(screenWidth, FLT_MAX)];
  node.frame = (CGRect){ CGPointZero, node.calculatedSize };

  // self.view isn't a node, so we can only use it on the main thread
  dispatch_sync(dispatch_get_main_queue(), ^{
    [self.view addSubview:node.view];
  });
});

```

目前可以使用的 Node 有以下几种

* ASDisplayNode. Counterpart to UIView — subclass to make custom nodes.
* ASControlNode. Analogous to UIControl — subclass to make buttons.
* ASImageNode. Like UIImageView — decodes images asynchronously.
* ASTextNode. Like UITextView — built on TextKit with full-featured rich text support.
* ASTableView. UITableView subclass that supports nodes.

我们可以在 - loadView 实现里这么写

```Objective-C
_imageView = [[UIImageView alloc] init];
_imageView.image = [UIImage imageNamed:@"hello"];
_imageView.frame = CGRectMake(10.0f, 10.0f, 40.0f, 40.0f);
[self.view addSubview:_imageView];
```

Node 版本

```Objective-C
_imageNode = [[ASImageNode alloc] init];
_imageNode.backgroundColor = [UIColor lightGrayColor];
_imageNode.image = [UIImage imageNamed:@"hello"];
_imageNode.frame = CGRectMake(10.0f, 10.0f, 40.0f, 40.0f);
[self.view addSubview:_imageNode.view];
```

表面看，上面的代码并没有用到异步 UI 渲染，但实际上速度仍然要快一点，因为 _imageNode.image = xxx 会在后台执行，所以能充分利用多核的计算能力。


此外，我们也能够使用 Node 来响应交互事件

```Objective-C
- (void)viewDidLoad
{
  [super viewDidLoad];

  // attribute a string
  NSDictionary *attrs = @{
                          NSFontAttributeName: [UIFont systemFontOfSize:12.0f],
                          NSForegroundColorAttributeName: [UIColor redColor],
                          };
  NSAttributedString *string = [[NSAttributedString alloc] initWithString:@"shuffle"
                                                               attributes:attrs];

  // create the node
  _shuffleNode = [[ASTextNode alloc] init];
  _shuffleNode.attributedString = string;

  // configure the button
  _shuffleNode.userInteractionEnabled = YES; // opt into touch handling
  [_shuffleNode addTarget:self
                   action:@selector(buttonTapped:)
         forControlEvents:ASControlNodeEventTouchUpInside];

  // size all the things
  CGRect b = self.view.bounds; // convenience
  CGSize size = [_shuffleNode measure:CGSizeMake(b.size.width, FLT_MAX)];
  CGPoint origin = CGPointMake(roundf( (b.size.width - size.width) / 2.0f ),
                               roundf( (b.size.height - size.height) / 2.0f ));
  _shuffleNode.frame = (CGRect){ origin, size };

  // add to our view
  [self.view addSubview:_shuffleNode.view];
}

- (void)buttonTapped:(id)sender
{
  NSLog(@"tapped!");
}

```

有一个问题是，按钮的大小并非有 44 那么高。所以我们这里还需要提供一个方法，让触摸的热点变的更大。

```Objective-C
 // size all the things
  /* ... */

  // make the tap target taller
  CGFloat extendY = roundf( (44.0f - size.height) / 2.0f );
  _shuffleNode.hitTestSlop = UIEdgeInsetsMake(-extendY, 0.0f, -extendY, 0.0f);
```

