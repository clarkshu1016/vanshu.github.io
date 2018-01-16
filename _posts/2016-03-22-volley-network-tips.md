---
layout: post
title: 关于Volley的日常使用
date: 2016-03-22
categories: [Android]
tags: [android]
description: an article about volley
---


目前项目采用的就是 volley＋gson＋OkHttp ,Okhttp相当于重写了httpClient和httpUrlConnection属于传输层协议的包


1. 如何开启Volley 日常调试 adb shell setprop log.tag.Volley VERBOSE



2. volley 日志分析
```
11-14 15:27:26.540: D/Volley(14335): [4488] BasicNetwork.logSlowRequests: HTTP response for request=<[ ] http://180.168.32.66:7000/koala/tycloud/technicalservicecmd 0x545ef8ef HIGH 105> [lifetime=635], [size=96], [rc=200], [retryCount=0]
11-14 15:27:26.610: D/Volley(14335): [1] MarkerLog.finish: (714  ms) [ ] http://180.168.32.66:7000/koala/tycloud/technicalservicecmd 0x545ef8ef HIGH 105
11-14 15:27:26.618: D/Volley(14335): [1] MarkerLog.finish: (+0   ) [4602] add-to-queue
11-14 15:27:26.618: D/Volley(14335): [1] MarkerLog.finish: (+1   ) [4485] cache-queue-take
11-14 15:27:26.618: D/Volley(14335): [1] MarkerLog.finish: (+8   ) [4485] cache-hit-expired
11-14 15:27:26.618: D/Volley(14335): [1] MarkerLog.finish: (+1   ) [4488] network-queue-take
11-14 15:27:26.626: D/Volley(14335): [1] MarkerLog.finish: (+636 ) [4488] network-http-complete
11-14 15:27:26.626: D/Volley(14335): [1] MarkerLog.finish: (+54  ) [4488] network-parse-complete
11-14 15:27:26.626: D/Volley(14335): [1] MarkerLog.finish: (+13  ) [4488] network-cache-written
11-14 15:27:26.626: D/Volley(14335): [1] MarkerLog.finish: (+0   ) [4488] post-response
```

logSlowRequests  的出现表示 开启了volley调试或者一个网络请求到返回超过3秒.HIGH   表示优先级高， 可以通过自定义Request实现，默认为Normal，但是请求图片的优先级默认为LOW
105 表示 第105个线程.

[lifetime=635], [size=96], [rc=200], [retryCount=0]  分别表示：
lifetime : 网络请求耗时（ms），
size :请求返回的大小(byte),
rc: (response code ) http header 返回status code  ，200表示 正常完成
retryCount 表示重试次数 ，自定义request可以设置 重发策略，和超时时间，以及重试的次数

* add-to-queue 加入队列

* cache-queue-take缓存队列接手

* cache-hit-expired没有发现本地缓存

* network-queue-take 网络队列接收 (如果此时 有很多网络在排队，可能这个点会耗时很久)

* network-http-complete  +636 表示 网络耗时的增量网络从发起，到结束的耗时

* network-parse-complete 解析耗时

* post-response 
回调个response listener 的耗时.


3.关于网络请求异常处理的情况(Volley框架)
 * 无效的请求链接  即不是 基于http 或者https 协议的
 * 无网络，NoConnectionError
 * 有网络，但是网络状况不好，导致请求超时   TimeoutError
 * 授权失败  服务器端报 401,403 错误等 AuthFailureError
 * 有网络，服务器也得到响应了，但是服务器报异常错误，如404,503等，返回的页面也不是json格式的 ServerError
 * 有网络，服务器也得到相应了，返回的格式也是json格式，但是可能由于插入属于失败或者其他原因，导致业务上没有返回要的东西，如
 果客户端这时候又没有写相应的解析，那么就会报解析失败异常ParseError错误
 * 有网络，服务器也得到相应了，返回格式也是json，而且客户端也正常解析了，但是解析的东西属于业务上失败的东西。




