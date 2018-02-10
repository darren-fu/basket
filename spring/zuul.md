[Zuul技术分析](http://tech.lede.com/2017/05/16/rd/server/SpringCloudZuul/)
 zuul中默认实现的filter,禁用指定的filter,自定义filter,zuul对动态语言的支持,全局异常处理,性能优化参考

# zuul对路由的配置 
Zuul对路由跳转的配置是在application.yml文件中，定义了两种映射方式：
- url映射
- serviceId映射
## url直接映射
- 单实例url直连
```
zuul:
  routes:
    wap:
	path: /wap/**
	url: http://192.168.1.10:8081
```
- 多实例路由 

```
zuul:
	routes:
		wap:
			path: /wap/**
			serviceId: wap
ribbon:
	eureka:
		enabled: false
wap:
	ribbon:
		listOfServers: http://192.168.1.10:8081, http://192.168.1.11:8081
``` 

- forward跳转到本地url
``` 
zuul:
	routes:
		wap:
			path: /wap/**
			url: forward:/wap
``` 
## serviceId映射 
默认serviceId，serviceId：activity，路由规则：/activity/101 -> /101
```
zuul:
	routes:
		activity: /activity/**
```
```
zuul:
	routes:
		activity:
			path: /activity/** # 指定
			serviceId: micro-activity # 指定路由的serviceId
			stripPrefix: false
```

## Cookie与头信息

默认情况下，Zuul在请求路由时，会过滤HTTP请求头信息中的一些敏感信息，默认的敏感头信息通过zuul.sensitiveHeaders定义，包括Cookie、Set-Cookie、Authorization。
- 设置全局参数覆盖默认值
```
zuul:
	sensitiveHeaders: # 使用空来覆盖默认值
```

- 指定路由的参数配置 
```
zuul:
	routes:
		[route]:
			customSensitiveHeaders: true # 对指定路由开启自定义敏感头
zuul:
	routes:
		[route]:
			sensitiveHeaders: # 对指定路由的敏感头设置为空
```
