> Netty 可用的 ChannelConfig
- ChannelConfig 

| Option | Method |
| --- | --- | 
ChannelOption.CONNECT_TIMEOUT_MILLIS |	setConnectTimeoutMillis(int)
ChannelOption.WRITE_SPIN_COUNT |	setWriteSpinCount(int)
ChannelOption.ALLOCATOR	| setAllocator(ByteBufAllocator)
ChannelOption.AUTO_READ	| setAutoRead(boolean)


- ServerSocketChannelConfig  extends ChannelConfig

| Option | Method |
| --- | --- | 
"backlog"	| setBacklog(int)
"reuseAddress" |	setReuseAddress(boolean)
"receiveBufferSize" |	setReceiveBufferSize(int)

- SocketChannelConfig  extends ChannelConfig

| Option | Method |
| --- | --- | 
ChannelOption.SO_KEEPALIVE	|setKeepAlive(boolean)
ChannelOption.SO_REUSEADDR|	setReuseAddress(boolean)
ChannelOption.SO_LINGER	 | setSoLinger(int)
ChannelOption.TCP_NODELAY	| setTcpNoDelay(boolean)
ChannelOption.SO_RCVBUF	| setReceiveBufferSize(int)
ChannelOption.SO_SNDBUF	| setSendBufferSize(int)
ChannelOption.IP_TOS	| setTrafficClass(int)
ChannelOption.ALLOW_HALF_CLOSURE |	setAllowHalfClosure(boolean)
