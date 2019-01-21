[TOC]

# Netty

## 基础

### 一，什么是Netty？

Netty 是一个基于NIO的客户、服务器端编程框架

### 二，Netty的基础组件

Channel：通道

EventLoop：线程组

ChannelFuture： 异步Channel

ChannelHandler：它充当了所有 处理入站和出站数据的应用程序逻辑的容器

ChannelPipeline ：ChannelPipeline 提供了 ChannelHandler 链的容器，并定义了用于在该链上传播入站 和出站事件流的 API

### 三，Netty中的Handler如果不实现exceptionCaught会发生什么？

每个 Channel 都拥有一个与之相关联的 ChannelPipeline，其持有一个 ChannelHandler 的 实例链。在默认的情况下，ChannelHandler 会把对它的方法的调用转发给链中的下一个 Channel- Handler。因此，如果exceptionCaught()方法没有被该链中的某处实现，那么所接收的异常将会被 传递到 ChannelPipeline 的尾端并被记录。为此，应用程序应该提供至少有一个实现了 exceptionCaught()方法的ChannelHandler。

### 四，SimpleChannelInboundHandler和ChannelInboundHandler 的区别？

在SimpleChannelInboundHandler中的channelRead0方法执行完毕以后，会释放掉接受消息的ByteBuf

在ChannelInboundHandler中的channelRead方法执行完毕后，不会释放传输消息的ByteBuf，因为write操作时异步的，并不知道其是否执行完成。其ByteBuf的释放实际为channelReadComplete()方法中，当 writeAndFlush()方 法被调用时被释放。

### 五，ChannelHandlerContext 和Channel和ChannelPipeline之间的关系

当Channel被绑定到ChannelPipeline上的时候会创建ChannelHandlerContext。ChannelPipeline包括了所有绑定的ChannelHandler