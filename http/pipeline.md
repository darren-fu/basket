- HTTP/1.1 without pipelining: Each HTTP request over the TCP connection must be responded to before the next request can be made.

- HTTP/1.1 with pipelining: Each HTTP request over the TCP connection may be made immediately without waiting for the previous request's response to return. The responses will come back in the same order.

- HTTP/2 multiplexing: Each HTTP request over the TCP connection may be made immediately without waiting for the previous response to come back. The responses may come back in any order.


![img](https://mdn.mozillademos.org/files/13727/HTTP1_x_Connections.png)


## HTTP pipelining is not activated by default in modern browsers:

- Buggy proxies are still common and these lead to strange and erratic behaviors that Web developers cannot foresee and diagnose easily.
- Pipelining is complex to implement correctly: the size of the resource being transferred, the effective RTT that will be used, as well as the effective bandwidth, have a direct incidence on the improvement provided by the pipeline. Without knowing these, important messages may be delayed behind unimportant ones. The notion of important even evolves during page layout! HTTP pipelining therefore brings a marginal improvement in most cases only.
- Pipelining is subject to the HOL problem.

For these reasons, pipelining has been superseded by a better algorithm, multiplexing, that is used by HTTP/2.

```
在现代浏览器中，缺省情况下HTTP流水线不会被激活：

Buggy代理仍然很常见，并且这些会导致Web开发人员无法轻易预见和诊断的奇怪且不稳定的行为。
流水线执行起来很复杂：正在传输的资源的大小，将要使用的有效RTT以及有效带宽，都直接影响到流水线所提供的改进。 如果不知道这些，重要的信息可能会被拖延到不重要的信息后面。 重要的概念甚至在页面布局中发展！ 因此，HTTP流水线在大多数情况下只会带来一些改进。
流水线服从HOL问题。
由于这些原因，流水线已经被HTTP / 2使用的更好的算法 - 多路复用所取代。
```
