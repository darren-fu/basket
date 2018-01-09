# docker command 

## 创建镜像
```shell
$ docker build -t "darrenfudocker/test:v1" .
```
## 删除容器
```shell
# 删除所有容器
$ docker rm $(docker ps -a -q)
```


## 进入容器 

```shell
#使用该命令进入一个已经在运行的容器
$ sudo docker ps  
$ sudo docker exec -it 775c7c9ee1e1 /bin/bash 
```
