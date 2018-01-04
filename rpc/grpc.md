## 基本数据类型
```
message SearchRequest {
  string query = 1;
  int32 page_number = 2;
  int32 result_per_page = 3;
  enum Corpus {
    UNIVERSAL = 0;
    WEB = 1;
    IMAGES = 2;
    LOCAL = 3;
    NEWS = 4;
    PRODUCTS = 5;
    VIDEO = 6;
  }
  Corpus corpus = 4;
}
```
proto类型|Java|默认值
--|--|--
string|String|""
bytes|ByteString|Empty ByteString
bool|Boolean|false
double|double|0
float|float|0
int32|int|0
int64|long|0
uint32,sint32,fixed32,sfixed32|int|0
uint64,sint64,fixed64,sfixed64|long|0
enum|enum|0

## 嵌套
```
message SearchResponse {
  repeated Result results = 1;
}

message Result {
  string url = 1;
  string title = 2;
  repeated string snippets = 3;
}
```

## gRPC通信方式

gRPC有四种通信方式: 

1、 Simple RPC 
简单rpc 
这就是一般的rpc调用，一个请求对象对应一个返回对象 
```
rpc simpleHello(Person) returns (Result) {}
```
2、 Server-side streaming RPC 
服务端流式rpc 
一个请求对象，服务端可以传回多个结果对象 
```
rpc serverStreamHello(Person) returns (stream Result) {}
```
3、 Client-side streaming RPC 
客户端流式rpc 
客户端传入多个请求对象，服务端返回一个响应结果 
```
rpc clientStreamHello(stream Person) returns (Result) {}
```
4、 Bidirectional streaming RPC 
双向流式rpc 
结合客户端流式rpc和服务端流式rpc，可以传入多个对象，返回多个响应对象 
```
rpc biStreamHello(stream Person) returns (stream Result) {}
```
## grpc坑：

来自https://news.ycombinator.com/item?id=12345223的网友： 
http2只允许单个链接传输10亿流数据。原因在于： 
htt2使用31位整形标示流，服务端使用奇数，客户端使用偶数，所以总共10亿可用。
