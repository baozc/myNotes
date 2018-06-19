> 使得事件流经 ChannelPipeline 是 ChannelHandler 的工作，它们是在应用程序的初 始化或者引导阶段被安装的。这些对象接收事件、执行它们所实现的处理逻辑，并将数据传递给 链中的下一个 ChannelHandler。它们的执行顺序是由它们被添加的顺序所决定的。实际上， 被我们称为 ChannelPipeline 的是这些 ChannelHandler 的编排顺序。
>
> 在Netty中，有两种发送消息的方式。你可以直接写到Channel中，也可以 写到和Channel- Handler 相关联的 ChannelHandlerContext 对象中。前一种方式将会导致消息从 Channel- Pipeline 的尾端开始流动，而后者将导致消息从 ChannelPipeline 中的下一个 Channel- Handler 开始流动。


## 更加深入地了解 ChannelHandler

### 为什么需要适配器类

有一些适配器类可以将编写自定义的 ChannelHandler 所需要的努力降到最低限度，因为它们提供了定义在对应接口中的所有方法的默认实现。

下面这些是编写自定义 ChannelHandler 时经常会用到的适配器类:
- ChannelHandlerAdapter
- ChannelInboundHandlerAdapter
- ChannelOutboundHandlerAdapter
- ChannelDuplexHandler
