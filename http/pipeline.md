- HTTP/1.1 without pipelining: Each HTTP request over the TCP connection must be responded to before the next request can be made.

- HTTP/1.1 with pipelining: Each HTTP request over the TCP connection may be made immediately without waiting for the previous request's response to return. The responses will come back in the same order.

- HTTP/2 multiplexing: Each HTTP request over the TCP connection may be made immediately without waiting for the previous response to come back. The responses may come back in any order.
