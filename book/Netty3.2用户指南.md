# Netty Project 3.2用户指南

## 经过验证的快速网络应用程序开发方法

3.2.6.Final

## 前言

## 1.问题

如今,我们使用通用应用程序或库相互通信.例如,我们经常使用HTTP客户端库从Web服务器检索信息,并通过Web服务调用远程过程调用.

但是,通用协议或其实现有时不能很好地扩展.就像我们不使用通用的HTTP服务器来交换巨大的文件,电子邮件和近乎实时的消息,例如财务信息和多人游戏数据.所需要的是专门用于特殊目的的高度优化的协议实现.例如,您可能想要实现一个HTTP服务器,该服务器针对基于AJAX的聊天应用程序,媒体流或大文件传输进行了优化.您甚至可能想要设计和实现完全适合您的需求的全新协议.

另一个不可避免的情况是,您必须处理旧的专有协议以确保与旧系统的互操作性.在这种情况下,重要的是我们能够在不牺牲最终应用程序的稳定性和性能的情况下,以多快的速度实现该协议.

## 2.解决方案

[Netty项目](http://www.jboss.org/netty/)致力于提供一个异步事件驱动的网络应用程序框架和工具,以快速开发可维护的高性能,高扩展性协议服务器和客户端.

换句话说,Netty是一个NIO客户端服务器框架,可以快速轻松地开发网络应用程序,例如协议服务器和客户端.它极大地简化和简化了网络编程,例如TCP和UDP套接字服务器开发.

“快速简便”并不意味着最终的应用程序将遭受可维护性或性能问题的困扰.Netty经过精心设计,结合了许多协议(例如FTP,SMTP,HTTP以及各种基于二进制和文本的旧式协议)的实施经验.结果,Netty成功地找到了一种无需妥协即可轻松实现开发,性能,稳定性和灵活性的方法.

有些用户可能已经发现其他声称具有相同优势的网络应用程序框架,并且您可能想问一下Netty与他们有何不同.答案是建立在其基础上的哲学.Netty旨在从第一天开始就API和实施方面为您提供最舒适的体验.这不是有形的东西,但是您会意识到,当您阅读本指南并与Netty一起玩时,这种哲学将使您的生活更加轻松.

## 第1章入门

本章将通过简单的示例介绍Netty的核心构造,以使您快速入门.在本章结束时,您将可以立即在Netty之上编写客户端和服务器.

如果您喜欢自上而下的学习方法,则可能要从[第2章.体系结构](#第2章架构概述)开始,然后回到此处.

## 1.1.开始之前

运行本章介绍的示例的最低要求只有两个;Netty和JDK 1.5或更高版本的最新版本.Netty的最新版本可在 [项目下载页面中找到](http://www.jboss.org/netty/downloads.html).要下载正确版本的JDK,请访问您首选的JDK供应商的网站.

在阅读时,您可能对本章介绍的类有更多疑问.如果您想进一步了解它们,请参考API参考.为了方便起见,本文档中的所有类名都链接到在线API参考.另外,请不要犹豫, [与Netty项目社区联系,](http://www.jboss.org/netty/community.html)并让我们知道是否有任何不正确的信息,语法和错字错误,以及您是否有改进文档的好主意.

## 1.2.编写丢弃服务器

世界上最简单的协议不是“ Hello,World!”.但是 [DISCARD](http://tools.ietf.org/html/rfc863).这是一个协议,它丢弃任何接收到的数据而没有任何响应.

要实现DISCARD协议,您唯一需要做的就是忽略所有接收到的数据.让我们直接从处理程序实现开始,该实现处理Netty生成的I / O事件.
```
package org.jboss.netty.example.discard;

public class DiscardServerHandler extends SimpleChannelHandler {

    @Override
    public void messageReceived(ChannelHandlerContext ctx, MessageEvent e) {
    }

    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, ExceptionEvent e) {
        e.getCause().printStackTrace();
        
        Channel ch = e.getChannel();
        ch.close();
    }
}
```
<table border="0" summary="Callout list"><tbody><tr><td width="5%" valign="top" align="left"><p>1</p></td><td valign="top" align="left"><p><code class="classname">DiscardServerHandler</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">扩展 </font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/SimpleChannelHandler.html"><code class="classname">SimpleChannelHandler</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">,这是的实现 </font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelHandler.html"><code class="interfacename">ChannelHandler</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">. </font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/SimpleChannelHandler.html"><code class="classname">SimpleChannelHandler</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">提供了可以覆盖的各种事件处理程序方法.</font><font style="vertical-align: inherit;">到目前为止,仅扩展</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/SimpleChannelHandler.html"><code class="classname">SimpleChannelHandler</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">而不是自己实现处理程序接口</font><font style="vertical-align: inherit;">就足够了</font><font style="vertical-align: inherit;">.</font></font></p></td></tr><tr><td width="5%" valign="top" align="left"><p>2</p></td><td valign="top" align="left"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">我们在</font></font><code class="methodname">messageReceived</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">这里</font><font style="vertical-align: inherit;">重写</font><font style="vertical-align: inherit;">事件处理程序方法.</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/MessageEvent.html"><code class="interfacename">MessageEvent</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">每当从客户端接收到新数据时,都会使用</font><font style="vertical-align: inherit;">调用此方法</font><font style="vertical-align: inherit;">,该方法包含接收到的数据.</font><font style="vertical-align: inherit;">在此示例中,我们通过不执行DISCARD协议来忽略接收到的数据.</font></font></p></td></tr><tr><td width="5%" valign="top" align="left"><p>3</p></td><td valign="top" align="left"><p><code class="methodname">exceptionCaught</code><font style="vertical-align: inherit;"></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ExceptionEvent.html"><code class="interfacename">ExceptionEvent</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">当Netty因I / O错误而引发异常,或者由于处理事件时引发异常而由处理程序实现引发</font><font style="vertical-align: inherit;">事件处理程序</font><font style="vertical-align: inherit;">时,将使用</font><font style="vertical-align: inherit;">调用事件处理程序方法</font><font style="vertical-align: inherit;">.</font><font style="vertical-align: inherit;">在大多数情况下,应记录捕获的异常并在此处关闭其关联的通道,尽管此方法的实现可能会有所不同,具体取决于您要处理特殊情况时要采取的措施.</font><font style="vertical-align: inherit;">例如,您可能想在关闭连接之前发送带有错误代码的响应消息.</font></font></p></td></tr></tbody></table>

到目前为止,一切都很好.我们已经实现了DISCARD服务器的前半部分.现在剩下的就是编写使用`main`来启动服务器的方法`DiscardServerHandler`.

```
package org.jboss.netty.example.discard;

import java.net.InetSocketAddress;
import java.util.concurrent.Executors;

public class DiscardServer {

    public static void main(String[] args) throws Exception {
        ChannelFactory factory =
            new NioServerSocketChannelFactory(
                    Executors.newCachedThreadPool(),
                    Executors.newCachedThreadPool());

        ServerBootstrap bootstrap = new ServerBootstrap(factory);

        bootstrap.setPipelineFactory(new ChannelPipelineFactory() {
            public ChannelPipeline getPipeline() {
                return Channels.pipeline(new DiscardServerHandler());
            }
        });

        bootstrap.setOption("child.tcpNoDelay", true);
        bootstrap.setOption("child.keepAlive", true);

        bootstrap.bind(new InetSocketAddress(8080));5
    }
}
```

<table border="0" summary="Callout list"><tbody><tr><td width="5%" valign="top" align="left"><p>1</p></td><td valign="top" align="left"><p><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelFactory.html"><code class="interfacename">ChannelFactory</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">是创建和管理</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html"><code class="interfacename">Channel</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">S及其相关资源</font><font style="vertical-align: inherit;">的工厂</font><font style="vertical-align: inherit;">.</font><font style="vertical-align: inherit;">它处理所有I / O请求并执行I / O生成</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelEvent.html"><code class="interfacename">ChannelEvent</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">.</font><font style="vertical-align: inherit;">Netty提供了各种 </font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelFactory.html"><code class="interfacename">ChannelFactory</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">实现.</font><font style="vertical-align: inherit;">在此示例中,我们正在实现服务器端应用程序,因此 </font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/socket/nio/NioServerSocketChannelFactory.html"><code class="classname">NioServerSocketChannelFactory</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">已被使用.</font><font style="vertical-align: inherit;">要注意的另一件事是,它不会自行创建I / O线程.</font><font style="vertical-align: inherit;">它应该从您在构造函数中指定的线程池中获取线程,并且它使您可以更好地控制应如何在应用程序运行的环境中管理线程,例如带有安全管理器的应用程序服务器.</font></font></p></td></tr><tr><td width="5%" valign="top" align="left"><p>2</p></td><td valign="top" align="left"><p><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/bootstrap/ServerBootstrap.html"><code class="classname">ServerBootstrap</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">是设置服务器的帮助程序类.</font><font style="vertical-align: inherit;">您可以</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html"><code class="interfacename">Channel</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">直接</font><font style="vertical-align: inherit;">使用来设置服务器</font><font style="vertical-align: inherit;">.</font><font style="vertical-align: inherit;">但是,请注意,这是一个繁琐的过程,在大多数情况下您不需要这样做.</font></font></p></td></tr><tr><td width="5%" valign="top" align="left"><p>3</p></td><td valign="top" align="left"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">在这里,我们配置</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelPipelineFactory.html"><code class="interfacename">ChannelPipelineFactory</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">.</font><font style="vertical-align: inherit;">每当服务器接受新的连接时,</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelPipeline.html"><code class="interfacename">ChannelPipeline</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">指定的都会创建</font><font style="vertical-align: inherit;">一个新的连接</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelPipelineFactory.html"><code class="interfacename">ChannelPipelineFactory</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">.</font><font style="vertical-align: inherit;">新管道包含</font></font><code class="classname">DiscardServerHandler</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">.</font><font style="vertical-align: inherit;">随着应用程序变得复杂,您可能会向管道添加更多处理程序,并最终将此匿名类提取到顶级类中.</font></font></p></td></tr><tr><td width="5%" valign="top" align="left"><p>4</p></td><td valign="top" align="left"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">您还可以设置特定于</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html"><code class="interfacename">Channel</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;"> 实现</font><font style="vertical-align: inherit;">的参数</font><font style="vertical-align: inherit;">.</font><font style="vertical-align: inherit;">我们正在编写一个TCP / IP服务器,因此我们可以设置套接字选项,例如</font></font><code class="literal">tcpNoDelay</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">和 </font></font><code class="literal">keepAlive</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">.</font><font style="vertical-align: inherit;">请注意, </font></font><code class="literal">"child."</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">前缀已添加到所有选项.</font><font style="vertical-align: inherit;">这意味着这些选项将应用于</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html"><code class="interfacename">Channel</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">而不是的选项</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/socket/ServerSocketChannel.html"><code class="interfacename">ServerSocketChannel</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">.</font><font style="vertical-align: inherit;">您可以执行以下操作来设置的选项</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/socket/ServerSocketChannel.html"><code class="interfacename">ServerSocketChannel</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">:</font></font></p><pre class="programlisting notranslate">bootstrap.setOption("reuseAddress", true);</pre><p></p></td></tr><tr><td width="5%" valign="top" align="left"><p>5</p></td><td valign="top" align="left"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">我们现在准备出发.</font><font style="vertical-align: inherit;">剩下的就是绑定到端口并启动服务器.</font><font style="vertical-align: inherit;">在这里,我们绑定到计算机</font></font><code class="literal">8080</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;"> 中所有NIC(网络接口卡)</font><font style="vertical-align: inherit;">的端口</font><font style="vertical-align: inherit;">.</font><font style="vertical-align: inherit;">现在,您可以</font></font><code class="methodname">bind</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">根据需要多次</font><font style="vertical-align: inherit;">调用该</font><font style="vertical-align: inherit;">方法(使用不同的绑定地址.)</font></font></p></td></tr></tbody></table>

恭喜你!您刚刚在Netty上完成了第一台服务器.

## 1.3.查看接收到的数据

现在,我们已经编写了第一台服务器,我们需要测试它是否确实有效.测试它的最简单方法是使用`telnet` 命令.例如,您可以`telnet localhost 8080`在命令行中输入“ ”并输入一些内容.

但是,我们可以说服务器工作正常吗?我们真的不知道这是因为它是一个废弃服务器.您根本不会得到任何回应.为了证明它确实有效,让我们修改服务器以打印其收到的内容.

我们已经知道,[`MessageEvent`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/MessageEvent.html)只要接收到数据并且`messageReceived`将调用处理程序方法,就会生成该函数.让我们将一些代码放入 `messageReceived`方法中 `DiscardServerHandler`:

```
@Override
public void messageReceived(ChannelHandlerContext ctx, MessageEvent e) {
    ChannelBuffer1 buf = (ChannelBuffer) e.getMessage();
    while(buf.readable()) {
        System.out.println((char) buf.readByte());
        System.out.flush();
    }
}
```

<table border="0" summary="Callout list"><tbody><tr><td width="5%" valign="top" align="left"><p>1</p></td><td valign="top" align="left"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">可以肯定的是,套接字传输中的消息类型始终为 </font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/buffer/ChannelBuffer.html"><code class="interfacename">ChannelBuffer</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">. </font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/buffer/ChannelBuffer.html"><code class="interfacename">ChannelBuffer</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">是一个基本的数据结构,在Netty中存储字节序列.</font><font style="vertical-align: inherit;">它与NIO相似 </font></font><code class="classname">ByteBuffer</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">,但更易于使用且更灵活.</font><font style="vertical-align: inherit;">例如,Netty允许您创建一个组合</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/buffer/ChannelBuffer.html"><code class="interfacename">ChannelBuffer</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">多个</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/buffer/ChannelBuffer.html"><code class="interfacename">ChannelBuffer</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">s </font><font style="vertical-align: inherit;">的组合 </font><font style="vertical-align: inherit;">,</font><font style="vertical-align: inherit;">从而减少了不必要的内存复制数量.</font></font></p><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">尽管它与NIO </font></font><code class="classname">ByteBuffer</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">非常</font><font style="vertical-align: inherit;">相似</font><font style="vertical-align: inherit;">,但强烈建议您参考API参考.</font><font style="vertical-align: inherit;">学习</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/buffer/ChannelBuffer.html"><code class="interfacename">ChannelBuffer</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">正确使用方法是轻松使用Netty的关键步骤.</font></font></p></td></tr></tbody></table>

如果`telnet`再次运行该命令,您将看到服务器打印收到的内容.

丢弃服务器的完整源代码位于 `org.jboss.netty.example.discard`分发包中.

## 1.4.编写一个Echo服务器

到目前为止,我们一直在使用数据而没有任何响应.但是,通常应假定服务器对请求作出响应.让我们学习如何通过实现[ECHO](http://tools.ietf.org/html/rfc862)协议将响应消息写到客户端,在 [ECHO](http://tools.ietf.org/html/rfc862)协议中,任何接收到的数据都将被发回.

与前面几节中实现的丢弃服务器的唯一区别在于,它会将接收到的数据发回,而不是将接收到的数据打印到控制台.因此,再次修改该`messageReceived`方法就足够了:

```
@Override
public void messageReceived(ChannelHandlerContext ctx, MessageEvent e) {
    Channel1 ch = e.getChannel();
    ch.write(e.getMessage());
}
```

<table border="0" summary="Callout list"><tbody><tr><td width="5%" valign="top" align="left"><p>1</p></td><td valign="top" align="left"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">甲</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelEvent.html"><code class="interfacename">ChannelEvent</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">对象具有到其相关联的参考</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html"><code class="interfacename">Channel</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">.</font><font style="vertical-align: inherit;">在此,返回</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html"><code class="interfacename">Channel</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">代表接收到的连接</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/MessageEvent.html"><code class="interfacename">MessageEvent</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">.</font><font style="vertical-align: inherit;">我们可以获取</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html"><code class="interfacename">Channel</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">和调用该 </font></font><code class="methodname">write</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">方法以将一些内容写回到远程对等方.</font></font></p></td></tr></tbody></table>

如果`telnet`再次运行该命令,您将看到服务器发回发送给它的所有内容.

回显服务器的完整源代码位于 `org.jboss.netty.example.echo`分发包中.

## 1.5.编写时间服务器

本节中要实现的协议是 [TIME](http://tools.ietf.org/html/rfc868)协议.它与前面的示例不同之处在于,它发送一条包含32位整数的消息,而没有接收到任何请求,并且一旦发送了消息,便失去了连接.在此示例中,您将学习如何构造和发送消息以及如何在完成时关闭连接.

因为我们将忽略任何接收到的数据,而是在建立连接后立即发送消息,所以`messageReceived`这次我们不能使用该 方法.相反,我们应该重写该`channelConnected`方法.以下是实现:
```
package org.jboss.netty.example.time;

public class TimeServerHandler extends SimpleChannelHandler {

    @Override
    public void channelConnected(ChannelHandlerContext ctx, ChannelStateEvent e) {1
        Channel ch = e.getChannel();
        
        ChannelBuffer time = ChannelBuffers.buffer(4);2
        time.writeInt((int) (System.currentTimeMillis() / 1000));
        
        ChannelFuture f = ch.write(time);3
        
        f.addListener(new ChannelFutureListener() {4
            public void operationComplete(ChannelFuture future) {
                Channel ch = future.getChannel();
                ch.close();
            }
        });
    }

    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, ExceptionEvent e) {
        e.getCause().printStackTrace();
        e.getChannel().close();
    }
}
```

<table border="0" summary="Callout list"><tbody><tr><td width="5%" valign="top" align="left"><p>1</p></td><td valign="top" align="left"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">如前所述,</font></font><code class="methodname">channelConnected</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">建立连接后将调用方法.</font><font style="vertical-align: inherit;">让我们在此处编写一个以秒为单位表示当前时间的32位整数.</font></font></p></td></tr><tr><td width="5%" valign="top" align="left"><p>2</p></td><td valign="top" align="left"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">要发送新消息,我们需要分配一个包含消息的新缓冲区.</font><font style="vertical-align: inherit;">我们将要写一个32位整数,因此我们需要一个</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/buffer/ChannelBuffer.html"><code class="interfacename">ChannelBuffer</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">容量为 </font></font><code class="literal">4</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">字节的字节.</font><font style="vertical-align: inherit;">该</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/buffer/ChannelBuffers.html"><code class="classname">ChannelBuffers</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">辅助类用于分配一个新的缓冲区.</font><font style="vertical-align: inherit;">除了 </font></font><code class="methodname">buffer</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">方法外,</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/buffer/ChannelBuffers.html"><code class="classname">ChannelBuffers</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">还提供了许多与相关的有用方法</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/buffer/ChannelBuffer.html"><code class="interfacename">ChannelBuffer</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">.</font><font style="vertical-align: inherit;">有关更多信息,请参考API参考.</font></font></p><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">另一方面,对以下情况使用静态导入是个好主意 </font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/buffer/ChannelBuffers.html"><code class="classname">ChannelBuffers</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">:</font></font></p><pre class="programlisting notranslate">import static org.jboss.netty.buffer.<a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/buffer/ChannelBuffers.html"><code class="classname">ChannelBuffers</code></a>.*;
...
<a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/buffer/ChannelBuffer.html"><code class="interfacename">ChannelBuffer</code></a>  dynamicBuf = dynamicBuffer(256);
<a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/buffer/ChannelBuffer.html"><code class="interfacename">ChannelBuffer</code></a> ordinaryBuf = buffer(1024);</pre><p></p></td></tr><tr><td width="5%" valign="top" align="left"><p>3</p></td><td valign="top" align="left"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">和往常一样,我们编写构造的消息.</font></font></p><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">但是,等等,在哪里</font></font><code class="methodname">flip</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">?</font><font style="vertical-align: inherit;">我们不是曾经</font></font><code class="methodname">ByteBuffer.flip()</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">在NIO中发送消息之前</font><font style="vertical-align: inherit;">打电话</font><font style="vertical-align: inherit;">吗? </font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/buffer/ChannelBuffer.html"><code class="interfacename">ChannelBuffer</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">没有这样的方法,因为它有两个指针;</font><font style="vertical-align: inherit;">一个用于读取操作,另一个用于写入操作.</font><font style="vertical-align: inherit;">当您向某处写入内容时,作家索引会增加,</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/buffer/ChannelBuffer.html"><code class="interfacename">ChannelBuffer</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">而读者索引不会改变.</font><font style="vertical-align: inherit;">读者索引和作者索引分别表示消息的开始和结束位置.</font></font></p><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">相反,NIO缓冲区没有提供一种干净的方法来确定消息内容的开始和结束位置,而无需调用该 </font></font><code class="methodname">flip</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">方法.</font><font style="vertical-align: inherit;">当您忘记翻转缓冲区时会遇到麻烦,因为将不会发送任何内容或发送不正确的数据.</font><font style="vertical-align: inherit;">在Netty中不会发生这样的错误,因为我们对不同的操作类型有不同的指针.</font><font style="vertical-align: inherit;">您会发现它使您适应它的生活变得轻松多了-无需翻身的生活!</font></font></p><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">要注意的另一点是该</font></font><code class="methodname">write</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;"> 方法返回一个</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelFuture.html"><code class="interfacename">ChannelFuture</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">.</font><font style="vertical-align: inherit;">A </font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelFuture.html"><code class="interfacename">ChannelFuture</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">表示尚未发生的I / O操作.</font><font style="vertical-align: inherit;">这意味着,由于Netty中的所有操作都是异步的,因此可能尚未执行任何请求的操作.</font><font style="vertical-align: inherit;">例如,以下代码甚至在发送消息之前就可能关闭连接:</font></font></p><pre class="programlisting notranslate"><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html"><code class="interfacename">Channel</code></a> ch = ...;
ch.write(message);
ch.close();</pre><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">因此,您需要在</font></font><code class="methodname">close</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;"> 方法</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelFuture.html"><code class="interfacename">ChannelFuture</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">返回的 </font><font style="vertical-align: inherit;">之后</font><font style="vertical-align: inherit;">调用该</font><font style="vertical-align: inherit;">方法,以</font></font><code class="methodname">write</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">在完成写操作后通知您.</font><font style="vertical-align: inherit;">请注意,</font></font><code class="methodname">close</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;"> 也可能不会立即关闭连接,并且返回 </font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelFuture.html"><code class="interfacename">ChannelFuture</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">.</font></font></p></td></tr><tr><td width="5%" valign="top" align="left"><p>4</p></td><td valign="top" align="left"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">写入请求完成后如何通知我们?</font><font style="vertical-align: inherit;">这就像将a添加</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelFutureListener.html"><code class="interfacename">ChannelFutureListener</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">到return </font><font style="vertical-align: inherit;">一样简单</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelFuture.html"><code class="interfacename">ChannelFuture</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">.</font><font style="vertical-align: inherit;">在这里,我们创建了一个新的匿名操作</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelFutureListener.html"><code class="interfacename">ChannelFutureListener</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;"> ,该</font><font style="vertical-align: inherit;">匿名</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html"><code class="interfacename">Channel</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">操作在操作完成后会</font><font style="vertical-align: inherit;">关闭</font><font style="vertical-align: inherit;">.</font></font></p><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">另外,您可以使用预定义的侦听器简化代码:</font></font></p><pre class="programlisting notranslate">f.addListener(<a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelFutureListener.html"><code class="interfacename">ChannelFutureListener</code></a>.CLOSE);</pre><p></p></td></tr></tbody></table>

## 1.6.编写时间客户端

与DISCARD和ECHO服务器不同,我们需要一个用于TIME协议的客户端,因为人类无法将32位二进制数据转换为日历中的日期.在本节中,我们讨论如何确保服务器正常工作,并学习如何使用Netty编写客户端.

服务器和Netty中的客户机之间的最大和唯一的区别在于不同的[`Bootstrap`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/bootstrap/Bootstrap.html)和[`ChannelFactory`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelFactory.html)是必需的.请看下面的代码:

```
package org.jboss.netty.example.time;

import java.net.InetSocketAddress;
import java.util.concurrent.Executors;

public class TimeClient {

    public static void main(String[] args) throws Exception {
        String host = args[0];
        int port = Integer.parseInt(args[1]);

        ChannelFactory factory =
            new NioClientSocketChannelFactory1(
                    Executors.newCachedThreadPool(),
                    Executors.newCachedThreadPool());

        ClientBootstrap bootstrap = new ClientBootstrap2(factory);

        bootstrap.setPipelineFactory(new ChannelPipelineFactory() {
            public ChannelPipeline getPipeline() {
                return Channels.pipeline(new TimeClientHandler());
            }
        });
        
        bootstrap.setOption("tcpNoDelay"3, true);
        bootstrap.setOption("keepAlive", true);

        bootstrap.connect4(new InetSocketAddress(host, port));
    }
}
```

<table border="0" summary="Callout list"><tbody><tr><td width="5%" valign="top" align="left"><p>1</p></td><td valign="top" align="left"><p><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/socket/nio/NioClientSocketChannelFactory.html"><code class="classname">NioClientSocketChannelFactory</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">,而不是</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/socket/nio/NioServerSocketChannelFactory.html"><code class="classname">NioServerSocketChannelFactory</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;"> 用于创建客户端</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html"><code class="interfacename">Channel</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">.</font></font></p></td></tr><tr><td width="5%" valign="top" align="left"><p>2</p></td><td valign="top" align="left"><p><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/bootstrap/ClientBootstrap.html"><code class="classname">ClientBootstrap</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">是的客户端对应项</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/bootstrap/ServerBootstrap.html"><code class="classname">ServerBootstrap</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">.</font></font></p></td></tr><tr><td width="5%" valign="top" align="left"><p>3</p></td><td valign="top" align="left"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">请注意,没有</font></font><code class="literal">"child."</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">前缀.</font><font style="vertical-align: inherit;">客户端</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/socket/SocketChannel.html"><code class="interfacename">SocketChannel</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">没有父级.</font></font></p></td></tr><tr><td width="5%" valign="top" align="left"><p>4</p></td><td valign="top" align="left"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">我们应该调用</font></font><code class="methodname">connect</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">方法而不是</font></font><code class="methodname">bind</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">方法.</font></font></p></td></tr></tbody></table>

如您所见,它与服务器端启动并没有真正的区别.怎么样[`ChannelHandler`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelHandler.html)实现?它应该从服务器接收一个32位整数,将其转换为人类可读的格式,打印转换后的时间,然后关闭连接:

```
package org.jboss.netty.example.time;

import java.util.Date;

public class TimeClientHandler extends SimpleChannelHandler {

    @Override
    public void messageReceived(ChannelHandlerContext ctx, MessageEvent e) {
        ChannelBuffer buf = (ChannelBuffer) e.getMessage();
        long currentTimeMillis = buf.readInt() * 1000L;
        System.out.println(new Date(currentTimeMillis));
        e.getChannel().close();
    }

    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, ExceptionEvent e) {
        e.getCause().printStackTrace();
        e.getChannel().close();
    }
}
```

它看起来非常简单,并且与服务器端示例没有任何不同.但是,此处理程序有时会拒绝工作 `IndexOutOfBoundsException`.我们将在下一节讨论为什么会发生这种情况.

## 1.7.处理基于流的传输

### 1.7.1.套接字缓冲区的一个小警告

在基于流的传输(例如TCP / IP)中,将接收到的数据存储到套接字接收缓冲区中.不幸的是,基于流的传输的缓冲区不是数据包队列而是字节队列.这意味着,即使您将两个消息作为两个独立的数据包发送,操作系统也不会将它们视为两个消息,而只是一堆字节.因此,不能保证您阅读的内容与您的远程对等方写的完全一样.例如,让我们假设操作系统的TCP / IP堆栈已收到三个数据包:

```
+-----+-----+-----+
| ABC | DEF | GHI |
+-----+-----+-----+
```
由于基于流的协议具有此一般属性,因此很有可能在您的应用程序中以以下分段形式读取它们:
```
+----+-------+---+---+
| AB | CDEFG | H | I |
+----+-------+---+---+
```
因此,无论是服务器端还是客户端,接收方都应将接收到的数据整理到一个或多个有意义的 *帧中*,以使应用程序逻辑易于理解.在上面的示例中,接收到的数据应采用以下格式:
```
+-----+-----+-----+
| ABC | DEF | GHI |
+-----+-----+-----+
```
### 1.7.2.第一个解决方案

现在让我们回到TIME客户端示例.我们在这里有同样的问题.32位整数是非常少量的数据,并且不太可能经常碎片化.但是,问题在于它 *可以*被碎片化,并且碎片化的可能性会随着流量的增加而增加.

一种简单的解决方案是创建一个内部累积缓冲区,然后等待直到所有4个字节都被接收到内部缓冲区中为止.以下是修改后的`TimeClientHandler` 实现,可以解决此问题:

```
package org.jboss.netty.example.time;

import static org.jboss.netty.buffer.ChannelBuffers.*;

import java.util.Date;

public class TimeClientHandler extends SimpleChannelHandler {

    private final ChannelBuffer buf = dynamicBuffer();1

    @Override
    public void messageReceived(ChannelHandlerContext ctx, MessageEvent e) {
        ChannelBuffer m = (ChannelBuffer) e.getMessage();
        buf.writeBytes(m);2
        
        if (buf.readableBytes() >= 4) {3
            long currentTimeMillis = buf.readInt() * 1000L;
            System.out.println(new Date(currentTimeMillis));
            e.getChannel().close();
        }
    }

    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, ExceptionEvent e) {
        e.getCause().printStackTrace();
        e.getChannel().close();
    }
}
```

<table border="0" summary="Callout list"><tbody><tr><td width="5%" valign="top" align="left"><p>1</p></td><td valign="top" align="left"><p><em class="firstterm"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">动态缓冲器</font></font></em><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">是一个</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/buffer/ChannelBuffer.html"><code class="interfacename">ChannelBuffer</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">增加其上的需求的能力.</font><font style="vertical-align: inherit;">当您不知道消息的长度时,它非常有用.</font></font></p></td></tr><tr><td width="5%" valign="top" align="left"><p>2</p></td><td valign="top" align="left"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">首先,应将所有接收到的数据累加到中 </font></font><code class="varname">buf</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">.</font></font></p></td></tr><tr><td width="5%" valign="top" align="left"><p>3</p></td><td valign="top" align="left"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">然后,处理程序必须检查是否</font></font><code class="varname">buf</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">有足够的数据(在此示例中为4个字节),然后继续进行实际的业务逻辑.</font><font style="vertical-align: inherit;">否则,Netty将</font></font><code class="methodname">messageReceived</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">在有更多数据到达时再次</font><font style="vertical-align: inherit;">调用该 </font><font style="vertical-align: inherit;">方法,最终将累加所有4个字节.</font></font></p></td></tr></tbody></table>

### 1.7.3.第二种解决方案

尽管第一个解决方案已经解决了TIME客户端的问题,但修改后的处理程序看起来并不干净.想象一个更复杂的协议,它由多个字段(例如可变长度字段)组成.您的[`ChannelHandler`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelHandler.html)实施将很快变得难以维护.


正如你已注意到的，[`ChannelPipeline`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelPipeline.html)[`ChannelHandler`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelHandler.html)，因此，为了减小应用的复杂性，你可以把这个臃肿的ChannelHandler切分为多个独立的模块单元。例如，你可以把TimeClientHandler切分为两个独立的处理器：

*   `TimeDecoder` 解决数据分段的问题
    
*   `TimeClientHandler` 原始版本的实现.
    

幸运的是,Netty提供了一个可扩展的类,可以帮助您开箱即用地编写第一个类:

```
package org.jboss.netty.example.time;

public class TimeDecoder extends FrameDecoder1 {

    @Override
    protected Object decode(
            ChannelHandlerContext ctx, Channel channel, ChannelBuffer buffer)2 {
            
        if (buffer.readableBytes() < 4) {
            return null; 3
        }
        
        return buffer.readBytes(4);4
    }
}
```

<table border="0" summary="Callout list"><tbody><tr><td width="5%" valign="top" align="left"><p>1</p></td><td valign="top" align="left"><p><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/handler/codec/frame/FrameDecoder.html"><code class="classname">FrameDecoder</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">是一个实现</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelHandler.html"><code class="interfacename">ChannelHandler</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">,可以轻松处理碎片问题.</font></font></p></td></tr><tr><td width="5%" valign="top" align="left"><p>2</p></td><td valign="top" align="left"><p><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/handler/codec/frame/FrameDecoder.html"><code class="classname">FrameDecoder</code></a><font style="vertical-align: inherit;"></font><code class="methodname">decode</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">每当接收到新数据时,将使用内部维护的累积缓冲区</font><font style="vertical-align: inherit;">调用</font><font style="vertical-align: inherit;">方法.</font></font></p></td></tr><tr><td width="5%" valign="top" align="left"><p>3</p></td><td valign="top" align="left"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">如果</font></font><code class="literal">null</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">返回,则表示数据不足. </font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/handler/codec/frame/FrameDecoder.html"><code class="classname">FrameDecoder</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">当有足够的数据量时将再次调用.</font></font></p></td></tr><tr><td width="5%" valign="top" align="left"><p>4</p></td><td valign="top" align="left"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">如果</font></font><code class="literal">null</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">返回</font><font style="vertical-align: inherit;">non- </font><font style="vertical-align: inherit;">,则表示该 </font></font><code class="methodname">decode</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">方法已成功解码消息. </font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/handler/codec/frame/FrameDecoder.html"><code class="classname">FrameDecoder</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">将丢弃其内部累积缓冲区的读取部分.</font><font style="vertical-align: inherit;">请记住,您不需要解码多条消息. </font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/handler/codec/frame/FrameDecoder.html"><code class="classname">FrameDecoder</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">将继续调用该</font></font><code class="methodname">decoder</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">方法,直到返回为止 </font></font><code class="literal">null</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">.</font></font></p></td></tr></tbody></table>

现在我们要在中插入另一个处理程序[`ChannelPipeline`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelPipeline.html),我们应该在中修改[`ChannelPipelineFactory`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelPipelineFactory.html)实现 `TimeClient`:

```
      bootstrap.setPipelineFactory(new ChannelPipelineFactory() {
            public ChannelPipeline getPipeline() {
                return Channels.pipeline(
                        new TimeDecoder(),
                        new TimeClientHandler());
            }
        });
```

如果您是一个冒险的人,则可能需要尝试, [`ReplayingDecoder`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/handler/codec/replay/ReplayingDecoder.html)它可以进一步简化解码器.不过,您将需要查阅API参考以获取更多信息.

```
package org.jboss.netty.example.time;

public class TimeDecoder extends ReplayingDecoder<VoidEnum> {

    @Override
    protected Object decode(
            ChannelHandlerContext ctx, Channel channel,
            ChannelBuffer buffer, VoidEnum state) {
            
        return buffer.readBytes(4);
    }
}
```

此外,Netty提供了开箱即用的解码器,使您能够非常轻松地实现大多数协议,并避免最终以单一的,不可维护的处理程序实现而告终.请参考以下软件包以获取更多详细示例:

*   `org.jboss.netty.example.factorial` 对于二进制协议,以及
    
*   `org.jboss.netty.example.telnet` 用于基于文本行的协议.
    

## 1.8.用POJO而不是ChannelBuffer说话

到目前为止,我们已经回顾的所有示例都使用a[`ChannelBuffer`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/buffer/ChannelBuffer.html)作为协议消息的主要数据结构.在本节中,我们将改进TIME协议客户端和服务器示例,以使用 [POJO](http://en.wikipedia.org/wiki/POJO)而不是 [`ChannelBuffer`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/buffer/ChannelBuffer.html).

使用POJO的优势[`ChannelHandler`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelHandler.html)显而易见.通过分离[`ChannelBuffer`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/buffer/ChannelBuffer.html)从处理程序中提取信息的代码,您的处理程序将变得更加可维护和可重用.在TIME客户端和服务器示例中,我们仅读取一个32位整数,并且[`ChannelBuffer`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/buffer/ChannelBuffer.html)直接使用不是主要问题.但是,您会发现在实现实际协议时有必要进行分离.

首先,让我们定义一个名为的新类型`UnixTime`.

```
package org.jboss.netty.example.time;

import java.util.Date;

public class UnixTime {
    private final int value;
    
    public UnixTime(int value) {
        this.value = value;
    }
    
    public int getValue() {
        return value;
    }
    
    @Override
    public String toString() {
        return new Date(value * 1000L).toString();
    }
}
```

现在,我们可以修改,`TimeDecoder`以返回`UnixTime`而不是[`ChannelBuffer`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/buffer/ChannelBuffer.html).

```
@Override
protected Object decode(
        ChannelHandlerContext ctx, Channel channel, ChannelBuffer buffer) {
    if (buffer.readableBytes() < 4) {
        return null;
    }

    return new UnixTime(buffer.readInt());1
}
```

<table border="0" summary="Callout list"><tbody><tr><td width="5%" valign="top" align="left"><p>1</p></td><td valign="top" align="left"><p><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/handler/codec/frame/FrameDecoder.html"><code class="classname">FrameDecoder</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">并</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/handler/codec/replay/ReplayingDecoder.html"><code class="classname">ReplayingDecoder</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">允许您返回任何类型的对象.</font><font style="vertical-align: inherit;">如果将它们限制为仅返回a </font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/buffer/ChannelBuffer.html"><code class="interfacename">ChannelBuffer</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">,则必须插入另一个</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelHandler.html"><code class="interfacename">ChannelHandler</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;"> 将a </font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/buffer/ChannelBuffer.html"><code class="interfacename">ChannelBuffer</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">转换为a </font></font><code class="classname">UnixTime</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">.</font></font></p></td></tr></tbody></table>

使用更新的解码器,`TimeClientHandler` 不再使用[`ChannelBuffer`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/buffer/ChannelBuffer.html):

```
@Override
public void messageReceived(ChannelHandlerContext ctx, MessageEvent e) {
    UnixTime m = (UnixTime) e.getMessage();
    System.out.println(m);
    e.getChannel().close();
}
```

更简单,更优雅,对吗?可以在服务器端应用相同的技术.让我们`TimeServerHandler`这次更新第 一个:

```
@Override
public void channelConnected(ChannelHandlerContext ctx, ChannelStateEvent e) {
    UnixTime time = new UnixTime(System.currentTimeMillis() / 1000);
    ChannelFuture f = e.getChannel().write(time);
    f.addListener(ChannelFutureListener.CLOSE);
}
```

现在,唯一缺少的一块是一个编码器,它是一个实现 [`ChannelHandler`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelHandler.html)的是一个转换`UnixTime`回成[`ChannelBuffer`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/buffer/ChannelBuffer.html).它比编写解码器要简单得多,因为在编码消息时无需处理数据包的碎片和汇编.

```
package org.jboss.netty.example.time;
    
import static org.jboss.netty.buffer.ChannelBuffers.*;

public class TimeEncoder extends SimpleChannelHandler {

    public void writeRequested(ChannelHandlerContext ctx, MessageEvent1 e) {
        UnixTime time = (UnixTime) e.getMessage();
        
        ChannelBuffer buf = buffer(4);
        buf.writeInt(time.getValue());
        
        Channels.write(ctx, e.getFuture(), buf);2
    }
}
```

<table border="0" summary="Callout list"><tbody><tr><td width="5%" valign="top" align="left"><p>1</p></td><td valign="top" align="left"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">编码器会覆盖此</font></font><code class="methodname">writeRequested</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;"> 方法以拦截写入请求.</font><font style="vertical-align: inherit;">请注意,</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/MessageEvent.html"><code class="interfacename">MessageEvent</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">此处</font><font style="vertical-align: inherit;">的 </font><font style="vertical-align: inherit;">参数与指定的类型相同,</font></font><code class="methodname">messageReceived</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">但它们的解释有所不同.</font><font style="vertical-align: inherit;">阿</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelEvent.html"><code class="interfacename">ChannelEvent</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">可以是一个 </font></font><em class="firstterm"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">上游</font></font></em><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">或</font></font><em class="firstterm"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">下游</font></font></em><font style="vertical-align: inherit;"><font style="vertical-align: inherit;"> 事件取决于该事件的流动方向.</font><font style="vertical-align: inherit;">例如,a </font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/MessageEvent.html"><code class="interfacename">MessageEvent</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">在被调用时可以是上游事件,在被调用时可以是</font></font><code class="methodname">messageReceived</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">下游事件</font></font><code class="methodname">writeRequested</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">.</font><font style="vertical-align: inherit;">请参考API参考以详细了解上游事件和下游事件之间的区别.</font></font></p></td></tr><tr><td width="5%" valign="top" align="left"><p>2</p></td><td valign="top" align="left"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">一旦有一个转换POJO到完成</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/buffer/ChannelBuffer.html"><code class="interfacename">ChannelBuffer</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">,你应该转发新的缓冲区到以前</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelDownstreamHandler.html"><code class="interfacename">ChannelDownstreamHandler</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">的</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelPipeline.html"><code class="interfacename">ChannelPipeline</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">. </font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channels.html"><code class="classname">Channels</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">提供了各种辅助方法来生成和发送一个</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelEvent.html"><code class="interfacename">ChannelEvent</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">.</font><font style="vertical-align: inherit;">在此示例中, </font><font style="vertical-align: inherit;">method创建一个新的 </font><font style="vertical-align: inherit;">并将其发送到中的上</font><font style="vertical-align: inherit;"> 一个</font><font style="vertical-align: inherit;">. </font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channels.html"><code class="classname">Channels</code></a><code class="literal">.write(...)</code><font style="vertical-align: inherit;"></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/MessageEvent.html"><code class="interfacename">MessageEvent</code></a><font style="vertical-align: inherit;"></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelDownstreamHandler.html"><code class="interfacename">ChannelDownstreamHandler</code></a><font style="vertical-align: inherit;"></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelPipeline.html"><code class="interfacename">ChannelPipeline</code></a><font style="vertical-align: inherit;"></font></p><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">另一方面,对以下情况使用静态导入是个好主意 </font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channels.html"><code class="classname">Channels</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">:</font></font></p><pre class="programlisting notranslate">import static org.jboss.netty.channel.<a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channels.html"><code class="classname">Channels</code></a>.*;
...
<a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelPipeline.html"><code class="interfacename">ChannelPipeline</code></a> pipeline = pipeline();
write(ctx, e.getFuture(), buf);
fireChannelDisconnected(ctx);</pre><p></p></td></tr></tbody></table>

剩下的最后一项任务是将a `TimeEncoder` 插入[`ChannelPipeline`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelPipeline.html)服务器端,这是一项简单的练习.

## 1.9.关闭您的应用程序

如果您运行了`TimeClient`,您一定已经注意到该应用程序不会退出,而只会继续运行而什么也不做.从完整的堆栈跟踪看,您还将发现有几个I / O线程正在运行.要关闭I / O线程并让应用程序正常退出,您需要释放分配的资源[`ChannelFactory`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelFactory.html).

典型的网络应用程序的关闭过程包括以下三个步骤:

1.  关闭所有服务器插槽(如果有),
    
2.  如果有,请关闭所有非服务器套接字(即客户端套接字和可接受的套接字),并且
    
3.  释放占用的所有资源[`ChannelFactory`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelFactory.html).
    

要将上述三个步骤应用到`TimeClient`, `TimeClient.main()`可以通过关闭唯一的一个客户端连接并释放以下所有资源来正常关闭自身[`ChannelFactory`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelFactory.html):

```
package org.jboss.netty.example.time;

public class TimeClient {
    public static void main(String[] args) throws Exception {
        ...
        ChannelFactory factory = ...;
        ClientBootstrap bootstrap = ...;
        ...
        ChannelFuture future1 = bootstrap.connect(...);
        future.awaitUninterruptibly();2
        if (!future.isSuccess()) {
            future.getCause().printStackTrace();3
        }
        future.getChannel().getCloseFuture().awaitUninterruptibly();4
        factory.releaseExternalResources();5
    }
}
```

<table border="0" summary="Callout list"><tbody><tr><td width="5%" valign="top" align="left"><p>1</p></td><td valign="top" align="left"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">所述</font></font><code class="methodname">connect</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">的方法</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/bootstrap/ClientBootstrap.html"><code class="classname">ClientBootstrap</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;"> 返回一个</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelFuture.html"><code class="interfacename">ChannelFuture</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">当连接尝试成功或失败,其通知.</font><font style="vertical-align: inherit;">它还具有</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html"><code class="interfacename">Channel</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">与连接尝试关联</font><font style="vertical-align: inherit;">的的引用</font><font style="vertical-align: inherit;">.</font></font></p></td></tr><tr><td width="5%" valign="top" align="left"><p>2</p></td><td valign="top" align="left"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">等待返回</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelFuture.html"><code class="interfacename">ChannelFuture</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">以确定连接尝试是否成功.</font></font></p></td></tr><tr><td width="5%" valign="top" align="left"><p>3</p></td><td valign="top" align="left"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">如果失败,我们将打印失败原因,以了解失败原因.</font><font style="vertical-align: inherit;">如果连接尝试既没有成功也没有取消,</font><font style="vertical-align: inherit;">的</font></font><code class="methodname">getCause()</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">方法</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelFuture.html"><code class="interfacename">ChannelFuture</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">将返回失败的原因.</font></font></p></td></tr><tr><td width="5%" valign="top" align="left"><p>4</p></td><td valign="top" align="left"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">现在,连接尝试已经结束,我们需要等待,直到连接是通过等待关闭</font></font><code class="varname">closeFuture</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;"> 的</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html"><code class="interfacename">Channel</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">.</font><font style="vertical-align: inherit;">每个</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html"><code class="interfacename">Channel</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">都有自己的位置,</font></font><code class="varname">closeFuture</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;"> 以便您得到通知,并可以在关闭时执行某些操作.</font></font></p><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">即使连接尝试失败,</font></font><code class="varname">closeFuture</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;"> 也会通知您,因为</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html"><code class="interfacename">Channel</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">连接尝试失败时,它将自动关闭.</font></font></p></td></tr><tr><td width="5%" valign="top" align="left"><p>5</p></td><td valign="top" align="left"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">此时所有连接均已关闭.</font><font style="vertical-align: inherit;">剩下的唯一任务是释放占用的资源</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelFactory.html"><code class="interfacename">ChannelFactory</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">.</font><font style="vertical-align: inherit;">就像调用其</font></font><code class="methodname">releaseExternalResources()</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;"> 方法</font><font style="vertical-align: inherit;">一样简单</font><font style="vertical-align: inherit;">.</font><font style="vertical-align: inherit;">包括NIO </font></font><code class="classname">Selector</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">和线程池</font><font style="vertical-align: inherit;">在内的所有资源</font><font style="vertical-align: inherit;">都将被关闭并自动终止.</font></font></p></td></tr></tbody></table>

关闭客户端非常容易,但是关闭服务器又如何呢?您需要从端口解除绑定并关闭所有打开的接受的连接.为此,您需要一个跟踪活动连接列表的数据结构,这并不是一件容易的事.幸运的是,有一个解决方案[`ChannelGroup`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/group/ChannelGroup.html).

[`ChannelGroup`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/group/ChannelGroup.html)是Java集合API的特殊扩展,它表示一组open [`Channel`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html).如果将a [`Channel`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html)添加到a [`ChannelGroup`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/group/ChannelGroup.html)并且[`Channel`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html)关闭了添加的对象,则关闭的对象[`Channel`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html) 会[`ChannelGroup`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/group/ChannelGroup.html)自动从中删除.您还可以[`Channel`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html)对同一组中的所有进行操作.举例来说,你可以关闭所有[`Channel`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html)在A S [`ChannelGroup`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/group/ChannelGroup.html)当您关闭服务器.

为了跟踪开放式插座,你需要修改 `TimeServerHandler`一个新的开放加入[`Channel`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html)到全球[`ChannelGroup`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/group/ChannelGroup.html),`TimeServer.allChannels`:

```
@Override
public void channelOpen(ChannelHandlerContext ctx, ChannelStateEvent e) {
    TimeServer.allChannels.add(e.getChannel());1
}
```

<table border="0" summary="Callout list"><tbody><tr><td width="5%" valign="top" align="left"><p>1</p></td><td valign="top" align="left"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">是的,</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/group/ChannelGroup.html"><code class="interfacename">ChannelGroup</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">是线程安全的.</font></font></p></td></tr></tbody></table>

现在,所有active[`Channel`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html)的列表都将自动维护,因此关闭服务器就像关闭客户端一样容易:

```
package org.jboss.netty.example.time;

public class TimeServer {

    static final ChannelGroup allChannels = new DefaultChannelGroup("time-server"1);

    public static void main(String[] args) throws Exception {
        ...
        ChannelFactory factory = ...;
        ServerBootstrap bootstrap = ...;
        ...
        Channel channel2 = bootstrap.bind(...);
        allChannels.add(channel);3
        waitForShutdownCommand();4
        ChannelGroupFuture future = allChannels.close();5
        future.awaitUninterruptibly();
        factory.releaseExternalResources();
    }
}
```

<table border="0" summary="Callout list"><tbody><tr><td width="5%" valign="top" align="left"><p>1</p></td><td valign="top" align="left"><p><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/group/DefaultChannelGroup.html"><code class="classname">DefaultChannelGroup</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">需要该组的名称作为构造函数参数.</font><font style="vertical-align: inherit;">组名仅用于区分一个组和另一个组.</font></font></p></td></tr><tr><td width="5%" valign="top" align="left"><p>2</p></td><td valign="top" align="left"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">所述</font></font><code class="methodname">bind</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">的方法</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/bootstrap/ServerBootstrap.html"><code class="classname">ServerBootstrap</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;"> 返回服务器端</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html"><code class="interfacename">Channel</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">被绑定到指定的本地地址.</font><font style="vertical-align: inherit;">调用</font></font><code class="methodname">close()</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">返回</font><font style="vertical-align: inherit;">的</font><font style="vertical-align: inherit;">方法</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html"><code class="interfacename">Channel</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">将使</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html"><code class="interfacename">Channel</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">绑定的本地地址取消绑定.</font></font></p></td></tr><tr><td width="5%" valign="top" align="left"><p>3</p></td><td valign="top" align="left"><p><font style="vertical-align: inherit;"></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html"><code class="interfacename">Channel</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">可以将</font><font style="vertical-align: inherit;"> 任何类型的</font><font style="vertical-align: inherit;">s添加到a,</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/group/ChannelGroup.html"><code class="interfacename">ChannelGroup</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">无论是服务器端,客户端还是接受的.</font><font style="vertical-align: inherit;">因此,</font><font style="vertical-align: inherit;">当服务器关闭时</font><font style="vertical-align: inherit;">,您可以</font><font style="vertical-align: inherit;">一次性</font><font style="vertical-align: inherit;">关闭边界</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html"><code class="interfacename">Channel</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">和接受</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html"><code class="interfacename">Channel</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">的.</font></font></p></td></tr><tr><td width="5%" valign="top" align="left"><p>4</p></td><td valign="top" align="left"><p><code class="methodname">waitForShutdownCommand()</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">是一种等待关机信号的虚构方法.</font><font style="vertical-align: inherit;">您可以等待来自特权客户机或JVM关闭挂钩的消息.</font></font></p></td></tr><tr><td width="5%" valign="top" align="left"><p>5</p></td><td valign="top" align="left"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">您可以在同一通道的所有通道上执行相同的操作 </font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/group/ChannelGroup.html"><code class="interfacename">ChannelGroup</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">.</font><font style="vertical-align: inherit;">在这种情况下,我们关闭所有通道,这意味着绑定的服务器端</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html"><code class="interfacename">Channel</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">将是未绑定的,所有接受的连接将被异步关闭.</font><font style="vertical-align: inherit;">要通知所有连接成功关闭的时间,它将返回</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/group/ChannelGroupFuture.html"><code class="interfacename">ChannelGroupFuture</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;"> 与相似的作用</font></font><a class="ulink" href="https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelFuture.html"><code class="interfacename">ChannelFuture</code></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">.</font></font></p></td></tr></tbody></table>

## 1.10.摘要

在本章中,我们快速浏览了Netty,并演示了如何在Netty之上编写功能全面的网络应用程序.在接下来的各章和本章的修订版中将涵盖您可能遇到的更多问题.另请注意, [社区](http://www.jboss.org/netty/community.html)一直在等待您的问题和想法,以帮助您并根据您的反馈不断改进Netty.

## 第2章.架构概述


![网络的架构图](https://docs.jboss.org/netty/3.2/guide/html_single/images/architecture.png)

在本章中,我们将研究Netty提供了哪些核心功能,以及它们如何构成核心顶部的完整网络应用程序开发堆栈.阅读本章时,请牢记此图.

## 2.1.丰富的缓冲区数据结构

Netty使用其自己的缓冲区API而不是NIO `ByteBuffer` 来表示字节序列.与使用相比,此方法具有明显的优势`ByteBuffer`.Netty的新缓冲区类型 [`ChannelBuffer`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/buffer/ChannelBuffer.html)是从头开始设计的,旨在解决`ByteBuffer`网络应用程序开发人员的问题并满足其日常需求.列出一些很酷的功能:

*   您可以根据需要定义自己的缓冲区类型.
    
*   透明的零复制是通过内置的复合缓冲区类型实现的.
    
*   提供了一个现成的动态缓冲区类型,其容量可以按需扩展,就像`StringBuffer`.
    
*   无需再打电话`flip()`了.
    
*   它通常比`ByteBuffer`.
    

有关更多信息,请参阅 [`org.jboss.netty.buffer`包装说明](https://daichangya.github.io/netty3.2-api/org/jboss/netty/buffer/package-summary.html#package_description).

## 2.2.通用异步I / O API

Java中的传统I / O API为不同的传输类型提供了不同的类型和方法.例如, `java.net.Socket`并且 `java.net.DatagramSocket`没有任何共同的超类型,因此他们有很不同的方式来执行套接字I / O.

这种不匹配使得将网络应用程序从一种传输移植到另一种繁琐且困难的工作.当您需要支持其他传输时,传输之间缺乏可移植性将成为一个问题,因为这通常需要重写应用程序的网络层.从逻辑上讲,许多协议可以在一种以上的传输上运行,例如TCP / IP,UDP / IP,SCTP和串行端口通信.

更糟的是,Java的新I / O(NIO)API引入了与旧的阻塞I / O(OIO)API的不兼容性,并将在下一发行版NIO.2(AIO)中继续这样做.由于所有这些API在设计和性能特征上都互不相同,因此您甚至不得不在开始实施阶段之前就确定应用程序将依赖哪个API.

例如,您可能要从OIO开始,因为要服务的客户端数量很少,并且使用OIO编写套接字服务器比使用NIO容易得多.但是,当您的业务成倍增长且服务器需要同时为成千上万的客户提供服务时,您将遇到麻烦.您可以从NIO开始,但是由于NIO Selector API的复杂性,这样做可能会通过大大增加开发时间来阻碍快速开发.

Netty有一个通用的异步I / O接口,称为a[`Channel`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/Channel.html),它抽象了点对点通信所需的所有操作.也就是说,一旦您在一个Netty传输上编写了应用程序,您的应用程序便可以在其他Netty传输上运行.Netty通过一个通用API提供了许多基本传输:

*   基于NIO的TCP / IP传输(请参阅参考资料`org.jboss.netty.channel.socket.nio`),
    
*   基于OIO的TCP / IP传输(请参阅参考资料`org.jboss.netty.channel.socket.oio`),
    
*   基于OIO的UDP / IP传输,以及
    
*   当地运输(请参阅参考资料`org.jboss.netty.channel.local`).
    

从一种传输方式切换到另一种传输方式通常仅需进行几行更改,例如选择其他[`ChannelFactory`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelFactory.html) 实现.

同样,您甚至还可以通过替换几行构造函数调用来利用尚未编写的新传输(例如,串行端口通信传输).此外,您可以通过扩展核心API来编写自己的传输.

## 2.3.基于拦截器链模式的事件模型

对于事件驱动的应用程序,必须具有定义明确且可扩展的事件模型.Netty有一个针对I / O的定义明确的事件模型.它还允许您在不破坏现有代码的情况下实现自己的事件类型,因为每种事件类型都通过严格的类型层次结构进行区分.这是与其他框架的另一个区别.许多NIO框架没有事件模型或事件模型的概念非常有限.如果它们提供扩展名,那么当您尝试添加自定义事件类型时,它们通常会破坏现有代码

A[`ChannelEvent`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelEvent.html)由中的[`ChannelHandler`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelHandler.html)s 列表处理[`ChannelPipeline`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelPipeline.html).管道实现了“ [拦截过滤器”](http://java.sun.com/blueprints/corej2eepatterns/Patterns/InterceptingFilter.html) 模式的高级形式, 以使用户可以完全控制事件的处理方式以及管道中的处理程序之间的交互方式.例如,您可以定义从套接字读取数据时的操作:

```
public class MyReadHandler implements SimpleChannelHandler {
    public void messageReceived(ChannelHandlerContext ctx, MessageEvent evt) {
        Object message = evt.getMessage();
        // Do something with the received message.
        ...

        // And forward the event to the next handler.
        ctx.sendUpstream(evt);
    }
}

```

您还可以定义处理程序收到写请求时的处理方式:

```
public class MyWriteHandler implements SimpleChannelHandler {
    public void writeRequested(ChannelHandlerContext ctx, MessageEvent evt) {
        Object message = evt.getMessage();
        // Do something with the message to be written.
        ...

        // And forward the event to the next handler.
        ctx.sendDownstream(evt);
    }
}
```

有关事件模型的详细信息,请参阅的API文档[`ChannelEvent`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelEvent.html)和[`ChannelPipeline`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelPipeline.html).

## 2.4.先进组件,实现更快的发展

除了上面提到的已经可以实现所有类型的网络应用程序的核心组件之外,Netty还提供了一组高级功能来进一步加快开发速度.

### 2.4.1.编解码器框架

如[第1.8节 用POJO代替ChannelBuffer进行演讲](#_18用pojo而不是channelbuffer说话)所示,将协议编解码器与业务逻辑分开总是一个好主意.但是,从头开始实施此想法时会遇到一些麻烦.您必须处理邮件的碎片.一些协议是多层的(即建立在其他较低级别协议之上).有些过于复杂,无法在单个状态机中实现.

因此,良好的网络应用程序框架应提供可生成,可重用,可单元测试的多层编解码器框架,以生成可维护的用户编解码器.

Netty提供了许多基本和高级编解码器,可解决您编写协议编解码器时遇到的大多数问题,无论它是简单的,二进制的还是文本的(无论如何).

### 2.4.2.SSL / TLS支持

与旧的阻塞I / O不同,在NIO中支持SSL是一项艰巨的任务.您不能简单地包装一个流来加密或解密数据,而必须使用`javax.net.ssl.SSLEngine`. `SSLEngine`是一个与SSL本身一样复杂的状态机.您必须管理所有可能的状态,例如密码套件和加密密钥协商(或重新协商),证书交换和验证.而且,`SSLEngine`正如人们所期望的那样,它甚至不是完全线程安全的.

在Netty中,请[`SslHandler`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/handler/ssl/SslHandler.html)注意的所有细节和陷阱`SSLEngine`.您所需要做的就是配置[`SslHandler`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/handler/ssl/SslHandler.html)并将其插入[`ChannelPipeline`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/channel/ChannelPipeline.html).它还使您可以 非常轻松地实现高级功能,例如 [StartTLS](http://en.wikipedia.org/wiki/Starttls).

### 2.4.3.HTTP实现

HTTP无疑是Internet中最流行的协议.已经有许多HTTP实现,例如Servlet容器.那么,为什么Netty在其核心之上具有HTTP?

Netty的HTTP支持与现有HTTP库有很大不同.它使您可以完全控制如何在低级别交换HTTP消息.因为它基本上是HTTP编解码器和HTTP消息类的组合,所以没有诸如强制线程模型之类的限制.也就是说,您可以编写自己的HTTP客户端或服务器,该客户端或服务器可以完全按照所需的方式工作.您可以完全控制HTTP规范中的所有内容,包括线程模型,连接生命周期和分块编码.

由于其高度可定制的特性,您可以编写非常高效的HTTP服务器,例如:

*   需要持久连接和服务器推送技术的聊天服务器(例如[Comet](http://en.wikipedia.org/wiki/Comet_%28programming%29) 和[WebSockets](http://en.wikipedia.org/wiki/WebSockets))
    
*   媒体流服务器,需要保持连接打开,直到整个媒体流化为止(例如2小时的视频)
    
*   允许上传大文件而没有内存压力的文件服务器(例如,每个请求上传1GB)
    
*   可扩展的混搭客户端,可异步连接到成千上万的第三方网络服务
    

### 2.4.4.Google协议缓冲区集成

[Google协议缓冲区](http://code.google.com/apis/protocolbuffers/docs/overview.html) 是快速实施随时间演变的高效二进制协议的理想解决方案.使用[`ProtobufEncoder`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/handler/codec/protobuf/ProtobufEncoder.html)和 [`ProtobufDecoder`](https://daichangya.github.io/netty3.2-api/org/jboss/netty/handler/codec/protobuf/ProtobufDecoder.html),您可以将Google协议缓冲区编译器(protoc)生成的消息类转换为Netty编解码器.请看一下 [“ LocalTime”示例](http://docs.jboss.org/netty/3.2/xref/org/jboss/netty/example/localtime/package-summary.html) ,该[示例](http://docs.jboss.org/netty/3.2/xref/org/jboss/netty/example/localtime/package-summary.html)说明您可以从[示例协议定义中](http://anonsvn.jboss.org/repos/netty/trunk/src/main/java/org/jboss/netty/example/localtime/LocalTimeProtocol.proto)轻松创建高性能的二进制协议客户端和服务器 .

## 2.5.摘要

在本章中,我们从功能角度回顾了Netty的总体体系结构.Netty具有简单但功能强大的体系结构.它由三个组件组成-缓冲区,通道和事件模型-所有高级功能都建立在这三个核心组件之上.一旦了解了这三种方式如何协同工作,就不难理解本章简要介绍的更高级的功能.

您可能仍未回答有关总体体系结构到底是什么样子以及每个功能如何协同工作的问题.如果是这样,最好 [与我们联系](http://www.jboss.org/netty/community.html)以改进本指南.