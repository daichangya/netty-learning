## 微信公众号

扫码关注微信公众号，Java码界探秘。
![Java码界探秘](http://images.zthinker.com/qrcode_for_gh_1e2587cc42b1_258_1587996055777.jpg)

[https://zthinker.com/](https://zthinker.com/)

https://daichangya.github.io/netty-learning/


Netty源码解析
====

Netty是Java世界知名的网络应用框架。本系列文章是Netty的源码导读。

## 为什么要读Netty源码？

我认为，一般研究Netty源码出于两个原因：

1. 日常工作中使用到Netty，想要进一步了解；
2. 对Java网络编程感兴趣，想知道如何构建一个高性能网络应用。

另外，Netty的代码组织比较优秀，从中可以学到代码结构组织的方法。

## 这些文章讲什么？

本系列文章的介绍点包括：Netty的设计思想，网络编程的领域知识，以及Netty代码结构的骨干，可能也会包括一些具体场景的应用以及一些特性的分析。

==========

## 文章索引

## 一、Netty的架构

### [1.概述](book/ch1-overview.md)
### [2.Netty中的buffer](book/ch2-buffer.md)
### [3.Channel中的Pipeline](book/ch3-pipeline.md)
### [4.Netty与Reactor模式](book/ch4-reactor.md)
### [5.分门别类讲讲Handler](book/ch5-handler.md) *未完成*

## 二、Netty中的特性与细节

### [1.理解Netty中的异步](book/detail/ch1-async-in-netty.md) 

## 三、Netty实战

### [1.构建一个socks proxy](book/socks-proxy-by-netty.md)

=========

## 其他学习资料：

《Netty代码分析》 by 阿里中间件团队博客:
[http://jm-blog.aliapp.com/?p=423](http://jm-blog.aliapp.com/?p=423)

《netty从入门到精通》:
[http://cqupt123.iteye.com/blog/1682657](http://cqupt123.iteye.com/blog/1682657)

[@kafka0102](http://weibo.com/kafka0102)的《Netty实现原理浅析》[http://www.kafka0102.com/2010/06/167.html](http://www.kafka0102.com/2010/06/167.html)

[@OneCoder](http://weibo.com/kubicoder)的《Java NIO框架Netty教程》:
[http://www.coderli.com/category/open-source/distributed/netty](http://www.coderli.com/category/open-source/distributed/netty)

twitter关于3.0与4.0中Channel Event的说明：
[https://blog.twitter.com/2013/netty-4-at-twitter-reduced-gc-overhead](https://blog.twitter.com/2013/netty-4-at-twitter-reduced-gc-overhead)

[@章炎-友盟](http://weibo.com/dirlt)的博客：
[http://dirlt.com/netty.html](http://dirlt.com/netty.html)

[@李林锋hw](http://weibo.com/lilinfeng)的大作： [Netty5.0架构剖析和源码解读.pdf](http://vdisk.weibo.com/s/C9LV9iVqH13rW)

## 使用Netty的开源项目：

* ### [Dubbo](https://github.com/alibaba/dubbo)

	阿里巴巴的RPC中间件。支持Netty和Mina。

* ### [Panda](https://github.com/daichangya/panda)

  基于Netty的 Java Web服务器。

* ### [Finagle](https://github.com/twitter/finagle)

	Twitter的RPC中间件。使用Scala编写。
	
* ### [Norbert](https://github.com/rhavyn/norbert)

	LinkedIn的RPC中间件。使用Scala编写。

* ### [Moco](https://github.com/dreamhead/moco)

	基于Netty的HTTP MOCK服务器。

## 协议：

相关代码遵循Apache V2协议。

文档遵循CC-BYNC协议。

