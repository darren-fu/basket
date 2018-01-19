# Http1.1 状态码
```
100-199 用于指定客户端应相应的某些动作。 
200-299 用于表示请求成功。 
300-399 用于已经移动的文件并且常被包含在定位头信息中指定新的地址信息。 
400-499 用于指出客户端的错误。 
500-599 用于支持服务器错误。 
```

## 100-199
- 100 (Continue/继续)
- 101 (Switching Protocols/转换协议)

## 200-299
- 200 (OK/正常)
- 201 (Created/已创建)
- 202 (Accepted/接受)
- 203 (Non-Authoritative Information/非官方信息)
- 204 (No Content/无内容)
- 205 (Reset Content/重置内容)
- 206 (Partial Content/局部内容)

## 300-399
- 300 (Multiple Choices/多重选择)
- 301 (Moved Permanently)(SC_MOVED_PERMANENTLY)状态是指所请求的文档在别的地方；文档新的URL会在定位响应头信息中给出。浏览器会自动连接到新的URL。 
- 302 (Found/找到)与301有些类似，只是定位头信息中所给的URL应被理解为临时交换地址而不是永久的
- 303 (See Other/参见其他信息)
- 304 (Not Modified/为修正)
- 305 (Use Proxy/使用代理)
- 307 (Temporary Redirect/临时重定向)

## 400-499
- 400 (Bad Request/错误请求)  客户端请求中的语法错误。
- 401 (Unauthorized/未授权) 客户端在授权头信息中没有有效的身份信息时访问受到密码保护的页面
- 403 (Forbidden/禁止)  
- 404 (Not Found/未找到)
- 405 (Method Not Allowed/方法未允许)
- 406 (Not Acceptable/无法访问)
- 407 (Proxy Authentication Required/代理服务器认证要求)
- 408 (Request Timeout/请求超时)
- 409 (Conflict/冲突)
- 410 (Gone/已经不存在)
- 411 (Length Required/需要数据长度)
- 412 (Precondition Failed/先决条件错误)
- 413 (Request Entity Too Large/请求实体过大)
- 414 (Request URI Too Long/请求URI过长)
- 415 (Unsupported Media Type/不支持的媒体格式)
- 416 (Requested Range Not Satisfiable/请求范围无法满足)
- 417 (Expectation Failed/期望失败)

## 500-599
- 500 (Internal Server Error/内部服务器错误)
- 501 (Not Implemented/未实现) 告诉客户端服务器不支持请求中要求的功能
- 502 (Bad Gateway/错误的网关) 接收服务器接收到远端服务器的错误响应。 
- 503 (Service Unavailable/服务无法获得) 服务器由于在维护或已经超载而无法响应，服务器可提供一个Retry-After头信息告诉客户端什么时候可以在试一次。 
- 504 (Gateway Timeout/网关超时) 
- 505 (HTTP Version Not Supported/不支持的 HTTP 版本)

---
- [HTTP状态码大全](https://www.cnblogs.com/lxinxuan/archive/2009/10/22/1588053.html)
