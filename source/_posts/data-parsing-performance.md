title: 使用Sqlite进行日期转换的黑科技
date: 2014-12-21 21:39:40
tags:
---
今天看到一篇文章，不由得对人民群众的创造力大为叹服。

苹果对日期的处理性能一直是让人诟病的一件事，广大机智的小伙伴也想出了很多方法去优化。比如缓存时间处理格式和对象等等。

今天这个小伙伴就利用了Sqlite的日期转换功能让日期转换提高了14倍的速度。

```
2013-09-07T23:45:00Z
```

这就是大家熟悉的 ISO 时间，我们需要转换为 NSDate

如果使用 NSFormatter 大概是这样的

```
NSDateFormatter *inputFormatter = [[NSDateFormatter alloc] init];
[inputFormatter setLocale:[[[NSLocale alloc] initWithLocaleIdentifier:@"en_US_POSIX"] autorelease]];
[inputFormatter setDateFormat:format];
NSDate *theDate = nil;
NSError *error = nil;
if (![inputFormatter getObjectValue:&theDate forString:string range:nil error:&error]) {
    NSLog(@"ERROR! Date '%@' with '%@' could not be parsed: %@", string, format,  error);
}
//NSDate *date = [inputFormatter dateFromString:string];
[inputFormatter release];

```


用 Sqlite 大概是这个过程

```
sqlite3_stmt *statement = NULL;
sqlite3_prepare_v2(db, "SELECT strftime('%s', ?);", -1, &statement, NULL);

sqlite3_bind_text(statement, 1, [dateString UTF8String], -1, SQLITE_STATIC);
sqlite3_step(statement);
sqlite3_int64 interval = sqlite3_column_int64(statement, 0);
NSDate *date = [NSDate dateWithTimeIntervalSince1970:interval];
```

很聪明的解法，比方法一快了14倍 ！。

