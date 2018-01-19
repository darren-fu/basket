# Netty
- [Netty 4.0中的新变化和注意点](http://colobu.com/2015/08/17/netty-new-and-noteworthy-in-4-0/)
- [Netty 4.1中的新变化和注意点](http://colobu.com/2015/08/18/netty-new-and-noteworthy-in-4-1/)
- [Netty 5.0 中的新变化和注意点](http://colobu.com/2015/08/18/netty-new-and-noteworthy-in-5-0/)
## 4.0优点
- Netty 绝不会并发地调用 ChannelHandler'的方法, 除非 ChannelHandler 被标记为 @Sharable. 这与handler方法的类型无关 - inbound, outbound, life cycle event handler method.
- 用户不必同步inbound or outbound event handler方法.
- 4.0 不允许增加 ChannelHandler 多次， 除非它被标记为 @Sharable.
- Netty 调用的ChannelHandler之间总是遵循 happens-before .用户不必定义volatile字段来保存 handler 的状态.
- 用户往ChannelPipeline中增加handler时可以指定 EventExecutor. 如果指定了一个EventExecutor, ChannelHandler的handler方法总是被这个指定的 EventExecutor执行.如果没有指定, handler 方法总是被它的Channel注册的 EventLoop 执行.
- 指派给handler或者channle的EventExecutor 和 EventLoop 总是单线程的.handler 方法总是被同一个线程调用.
- 如果使用多线程的EventExecutor 或 EventLoop, 首先会选择其中一个线程， 然后一直使用这个线程，直到取消注册（deregistration）.
- 如果在同意管道pipeline中的两个handler 被指派给不同的 EventExecutor, 它们可以被并发地调用. 如果它们访问共享数据， 用户必须小心线程安全 .
- 增加到ChannelFuture 的ChannelFutureListeners 总是此Channel相关的EventLoop 执行.
