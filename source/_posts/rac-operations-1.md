title: RACSignal操作详解(一)
date: 2015-03-29 18:27:27
tags:
---

RACSignal 作为 函数响应式编程中的重要概念，它的相关操作非常值得深入研究

### 行为注入

```
- (RACSignal *)doNext:(void (^)(id x))block;

- (RACSignal *)doError:(void (^)(NSError *error))block;

- (RACSignal *)doCompleted:(void (^)(void))block;

- (RACSignal *)initially:(void (^)(void))block;

- (RACSignal *)finally:(void (^)(void))block;
```

### 时机操作

```
- (RACSignal *)throttle:(NSTimeInterval)interval;

- (RACSignal *)throttle:(NSTimeInterval)interval valuesPassingTest:(BOOL (^)(id next))predicate;

- (RACSignal *)delay:(NSTimeInterval)interval;

- (RACSignal *)repeat;
```

### 蓄积

```
- (RACSignal *)bufferWithTime:(NSTimeInterval)interval onScheduler:(RACScheduler *)scheduler;

- (RACSignal *)collect;

- (RACSignal *)takeLast:(NSUInteger)count;
```

### 合并

```
- (RACSignal *)combineLatestWith:(RACSignal *)signal;

+ (RACSignal *)combineLatest:(id<NSFastEnumeration>)signals;

+ (RACSignal *)combineLatest:(id<NSFastEnumeration>)signals reduce:(id (^)())reduceBlock;

- (RACSignal *)merge:(RACSignal *)signal;

+ (RACSignal *)merge:(id<NSFastEnumeration>)signals;

- (RACSignal *)flatten:(NSUInteger)maxConcurrent;
```




