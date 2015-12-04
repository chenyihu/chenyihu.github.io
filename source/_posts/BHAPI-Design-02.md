title: BHAPI设计 (二)
date: 2015-03-15 23:36:10
tags:
---

#### 缓存系统的设计

BHAPI 使用了 MKNetworkKit 作为通讯框架。

MKNetworkKit 的缓存策略相当简单：
1. MKNetworkEngine 通过 useCache 作为缓存使用与否的总开关；
2. 一旦开启缓存总开关，MKNetworkOperation 还能够通过 shouldNotCacheResponse 来控制某个请求是否使用缓存；
3. 缓存激活后，每次请求的结果都会被存在本地文件中，以 UUID 命名；
4. 每次请求都会携带 Etag 访问服务器，由 304 等状态码来确定缓存的失效与否；

在薄荷的项目中，曾经使用了 MK 的这种缓存方式，但是带来了一些问题，原因在于每次请求后不论服务器返回与否都会发生第一次回调，这次回调会把预先产生的文件缓存作为返回数据，接下来进行真正的请求回调，如果不是 304 那么会再次返回一次数据，这样在一些场景下，缓存就导致了奇怪的问题，比如动态刷出了两个一模一样的。

实际上我们期望的是这样的：假如请求返回的状态码是 304，那么就只返回缓存数据，假如不是，那么就只返回新数据。
这样看起来是合理的，但是很多场景下还是需要在服务器返回前提供占位数据，这样就不难理解 MK 这样做的初衷。

所以 BHAPI 就统和这两种需求，增加了两个控制缓存的开关，useCache 和 usePlaceHolder

1. useCache = false 不用缓存
2. useCache = true & usePlaceHolder = false 使用缓存，但是需要服务器返回缓存状态后才返回
3. useCache = true & usePlaceHolder = true  服务器未返回时就使用文件缓存进行占位

Done !



