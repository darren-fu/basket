# K8S On Ubuntu 18.04

[TOC]

## 环境准备

### 关闭防火墙

ubuntu默认未启用，无需设置

```shell
$ sudo ufw disable
```
### 禁用SELINUX
ubuntu默认不安装，如果安装了，禁用方式：
``` shell
#临时禁用
$ sudo setenforce 0                 
#0代表permissive 1代表enforcing
#永久禁用
$ sudo vi /etc/selinux/config
SELINUX=permissive
```
> kubelet目前对selinux的支持还不好，需要禁用掉。
不禁用selinux的话有时还会出现明明容器里是root身份运行，操作挂载进来的主机文件夹时没有权限的情况，这时候要么配置selinux的权限，要么就禁掉selinux
另外，当docker的storage-driver使用overlay2的时候，低版本内核的selinux不支持overlay2文件驱动，docker启动时设置为--selinux-enabled会失败报错:“Error starting daemon: SELinux is not supported with the overlay2 graph driver on this kernel”，需设置--selinux-enabled=false

### 开启数据包转发

#### 内核开启ipv4转发

1. 修改/etc/sysctl.conf，开启ipv4转发：
```shell
$ sudo vim /etc/sysctl.conf
net.ipv4.ip_forward = 1           
#开启ipv4转发，允许内置路由
```

2. 写入后执行如下命令生效：
```shell
$ sudo sysctl -p
```
> 什么是ipv4转发：出于安全考虑，Linux系统默认是禁止数据包转发的。转发即当主机拥有多于一块的网卡时，其中一块收到数据包，根据数据包的目的ip地址将数据包发往本机另一块网卡，该网卡根据路由表继续发送数据包。这通常是路由器所要实现的功能。
kube-proxy的ipvs模式和calico(都涉及路由转发)都需要主机开启ipv4转发。
另外，不使用k8s，即使只使用docker的时候，以下两种情况也依赖ipv4转发：
<1>当同一主机上的两个跨bridge(跨bridge相当于跨网段，跨网络需要路由)的容器互访
<2>从容器内访问外部

#### 防火墙修改FORWARD链默认策略
数据包经过路由后，假如不是发往本机的流量，下一步会走iptables的FORWARD链，而docker从1.13版本开始，将FORWARD链的默认策略设置为DROP，会导致出现一些例如跨主机的两个pod使用podIP互访失败等问题。解决方案有2个：

在所有节点上开机启动时执行iptables -P FORWARD ACCEPT
让docker不操作iptables

- 方案 1
配置写进/etc/rc.local中，重启后自动执行
```shell
/usr/sbin/iptables -P FORWARD ACCEPT
```
- 方案 2
设置docker启动参数添加--iptables=false选项，使docker不再操作iptables，比如1.10版以上可编辑docker daemon默认配置文件/etc/docker/daemon.json:
```json
{
    "iptables": false
}
```

> 建议方案二
> kubernetes官网建议和k8s结合的时候docker的启动参数设置--iptables=false使得docker不再操作iptables，完全由kube-proxy来操作iptables。
> 参考：
> <1>https://docs.docker.com/v17.09/engine/userguide/networking/default_network/container-communication/#container-communication-between-hosts
> <2>https://github.com/kubernetes/kubernetes/issues/40182
> <3>https://kubernetes.io/docs/setup/scratch/#docker 

### 禁用SWAP
``` shell
$ sudo swapoff -a
```
同时修改/etc/fstab文件永久关闭swap
> Kubernetes 1.8开始要求关闭系统的Swap，如果不关闭，默认配置下kubelet将无法启动，虽然可以通过kubelet的启动参数--fail-swap-on=false更改这个限制，但不建议，最好还是不要开启swap。
一些为什么要关闭swap的讨论：
<1>https://github.com/kubernetes/kubernetes/issues/7294
<2>https://github.com/kubernetes/kubernetes/issues/53533

### 配置iptables参数，使得流经网桥的流量也经过iptables/netfilter防火墙
```shell
$ sudo tee /etc/sysctl.d/k8s.conf <<-'EOF'
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

$ sudo sysctl --system
```
> 网络插件需要为kube-proxy提供一些特定的支持，比如kube-proxy的iptables模式基于iptables，网络插件就需要确保容器的流量可以流过iptables。比如一些网络插件会用到网桥，而网桥工作在数据链路层，iptables/netfilter防火墙工作在网络层，以上配置则可以使得通过网桥的流量也进入iptables/netfilter防火墙中，确保iptables模式的kube-proxy可以正常工作。
默认没有指定kubelet网络插件的情况下，会使用noop插件，它也会设置net/bridge/bridge-nf-call-iptables=1来确保iptables模式的kube-proxy可以正常工作。
参考：
<1>https://kubernetes.io/docs/concepts/extend-kubernetes/compute-storage-net/network-plugins/#network-plugin-requirements
<2>https://kubernetes.io/docs/setup/independent/install-kubeadm/

## 安装docker
###　卸载旧版本docker
``` shell
$ sudo apt-get remove docker docker-engine docker.io                     
```
### 安装依赖
``` shell
sudo apt-get update

sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
```
### 安装docker
不指定版本，下方步骤可忽略
```shell
# 添加docker的GPG key:
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
# 国内访问download.docker.com不稳定的话可以使用阿里云的
$ curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -

# 设置docker镜像源
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
# 国内访问download.docker.com不稳定的话可以使用阿里云镜像源
$ sudo add-apt-repository \
   "deb [arch=amd64] https://mirrors.aliyun.com/docker-ce/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
# 安装指定版本docker-ce
$ apt-cache madison docker-ce
 docker-ce | 18.06.1~ce~3-0~ubuntu | https://mirrors.aliyun.com/docker-ce/linux/ubuntu bionic/stable amd64 Packages
 docker-ce | 18.06.0~ce~3-0~ubuntu | https://mirrors.aliyun.com/docker-ce/linux/ubuntu bionic/stable amd64 Packages
 docker-ce | 18.03.1~ce~3-0~ubuntu | https://mirrors.aliyun.com/docker-ce/linux/ubuntu bionic/stable amd64 Packages
# 安装18.03.1版
$ sudo apt-get install -y docker-ce=18.03.1~ce~3-0~ubuntu
```
```shell
sudo apt-get install docker.io
sudo systemctl enable --now docker.service
#将当前登录用户加入docker用户组中
$ sudo usermod -aG docker lk
```
> docker部署参考：https://docs.docker.com/install/linux/docker-ce/ubuntu

### docker启动参数配置
为docker做如下配置：
- 设置阿里云镜像库加速dockerhub的镜像。国内访问dockerhub不稳定，将对dockerhub的镜像拉取代理到阿里云镜像库
- 禁用iptables的设置
- 如果想让podIP可路由的话，设置docker不再对podIP做MASQUERADE，否则docker会将podIP这个源地址SNAT成nodeIP
- 设置docker存储驱动为overlay2（需要linux kernel版本在4.0以上，docker版本大于1.12）
- 根据业务规划修改容器实例存储根路径（默认路径是/var/lib/docker）
最终配置如下：
```shell
$ sudo tee /etc/docker/daemon.json <<-'EOF'
{
 "registry-mirrors": ["https://xxxxxxxx.mirror.aliyuncs.com"],
 "iptables": false,
 "ip-masq": false,
 "storage-driver": "overlay2",
 "graph": "/home/lk/docker"
}
EOF

$ sudo systemctl restart docker
```
> docker的所有启动参数可见：https://docs.docker.com/engine/reference/commandline/dockerd/
将xxxxxxxx替换成阿里云为你生成的镜像代理仓库前缀

### 为docker设置http代理
假如机器在内网环境无法直接访问外网的话，还需要为docker设置一个http_proxy。
``` shell
$ sudo mkdir /etc/systemd/system/docker.service.d

$ sudo tee /etc/systemd/system/docker.service.d/http-proxy.conf <<-'EOF'
[Service]
Environment="HTTP_PROXY=http://xxx.xxx.xxx.xxx:xxxx"
Environment="NO_PROXY=localhost,127.0.0.0/8"
EOF

$ sudo systemctl daemon-reload
$ sudo systemctl restart docker
```

## 安装kubeadm、kubelet、kubectl
- 饿汉子respo
```shell
sudo apt-get update && sudo apt-get install -y apt-transport-https
sudo apt install curl
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg |sudo  apt-key add -
#或者 sudo curl -s https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg | sudo apt-key add -
cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
# 或者 deb https://mirrors.aliyun.com/kubernetes/apt kubernetes-xenial main
EOF

sudo apt-get update
```
- 安装
```shell
#查看可用软件版本：
$ apt-cache madison kubeadm
kubeadm |  1.12.1-00 | https://mirrors.aliyun.com/kubernetes/apt kubernetes-xenial/main amd64 Packages
   kubeadm |  1.12.0-00 | https://mirrors.aliyun.com/kubernetes/apt kubernetes-xenial/main amd64 Packages
   kubeadm |  1.11.3-00 | https://mirrors.aliyun.com/kubernetes/apt kubernetes-xenial/main amd64 Packages
   ......
＃安装指定版本：
$ sudo apt-get install -y kubelet=1.12.0-00 kubeadm=1.12.0-00 kubectl=1.12.0-00
$ sudo apt-mark hold kubelet=1.12.0-00 kubeadm=1.12.0-00 kubectl=1.12.0-00

# 设置开机自启动并运行kubelet：
sudo systemctl enable kubelet && sudo systemctl start kubelet
```
> 此时kubelet的服务运行状态是异常的(因为缺少主配置文件kubelet.conf等，可以暂不处理，因为在完成Master节点的初始化后才会生成这个配置文件）;
> 参考: https://kubernetes.io/docs/setup/independent/install-kubeadm/

## 安装K8S集群
### Master节点
#### 提前下载镜像（FQ的不需要此步骤）
看一下kubernetes v1.12.0需要哪些镜像：
```shell
$ kubeadm config images list --kubernetes-version=v1.12.0

k8s.gcr.io/kube-apiserver:v1.12.0
k8s.gcr.io/kube-controller-manager:v1.12.0
k8s.gcr.io/kube-scheduler:v1.12.0
k8s.gcr.io/kube-proxy:v1.12.0
k8s.gcr.io/pause:3.1
k8s.gcr.io/etcd:3.2.24
k8s.gcr.io/coredns:1.2.2
# 从镜像站下载
docker pull anjia0532/google-containers.kube-apiserver:v1.12.0
docker pull anjia0532/google-containers.kube-controller-manager:v1.12.0
docker pull anjia0532/google-containers.kube-scheduler:v1.12.0
docker pull anjia0532/google-containers.kube-proxy:v1.12.0
docker pull anjia0532/google-containers.pause:3.1
docker pull anjia0532/google-containers.etcd:3.2.24
docker pull anjia0532/google-containers.coredns:1.2.2
# 重新打回k8s.gcr.io的镜像tag:
docker tag anjia0532/google-containers.kube-apiserver:v1.12.0 k8s.gcr.io/kube-apiserver:v1.12.0
docker tag anjia0532/google-containers.kube-controller-manager:v1.12.0 k8s.gcr.io/kube-controller-manager:v1.12.0
docker tag anjia0532/google-containers.kube-scheduler:v1.12.0 k8s.gcr.io/kube-scheduler:v1.12.0
docker tag anjia0532/google-containers.kube-proxy:v1.12.0 k8s.gcr.io/kube-proxy:v1.12.0
docker tag anjia0532/google-containers.pause:3.1 k8s.gcr.io/pause:3.1
docker tag anjia0532/google-containers.etcd:3.2.24 k8s.gcr.io/etcd:3.2.24
docker tag anjia0532/google-containers.coredns:1.2.2 k8s.gcr.io/coredns:1.2.2

```
以上两步也可以直接通过以下脚本image-process.sh完成：
```shell
#!/bin/bash
images=(kube-proxy:v1.12.0 kube-scheduler:v1.12.0 kube-controller-manager:v1.12.0 kube-apiserver:v1.12.0 etcd:3.2.24 pause:3.1 coredns:1.2.2)
for imageName in ${images[@]} ; do
  docker pull anjia0532/google-containers.$imageName
  docker tag anjia0532/google-containers.$imageName k8s.gcr.io/$imageName
  docker rmi anjia0532/google-containers.$imageName
done
```
####  kubeadm init初始化集群
- 使用命令行初始化
```shell
sudo kubeadm init --apiserver-advertise-address=192.168.1.175 --pod-network-cidr=172.16.0.0/16 --service-cidr=10.233.0.0/16 --kubernetes-version=v1.12.0
```
- 使用配置文件初始化
 配置文件写法参考：
https://unofficial-kubernetes.readthedocs.io/en/latest/admin/kubeadm/
http://pwittrock.github.io/docs/admin/kubeadm/

#### 检查kubelet使用的cgroup driver
kubelet启动时指定的cgroup driver需要和docker所使用的保持一致。
- 查看 Docker 使用的 cgroup driver:
```shell
$ docker info | grep -i cgroup
-> Cgroup Driver: cgroupfs
```
- kubelet目前默认是cgroups，yum安装可能会被修改成'systemd' 
如需修改，参考：https://kubernetes.io/docs/setup/independent/install-kubeadm/#configure-cgroup-driver-used-by-kubelet-on-master-node

#### 创建kubectl使用的kubeconfig文件
```shell
# kubeadm init完成后，会输出此命令
$ mkdir -p $HOME/.kube
$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
$ sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
#### 设置master参与工作负载
使用kubeadm初始化的集群，将master节点做了taint（污点），使得默认情况下（不设置容忍）Pod不会被调度到master上。这里搭建的是测试环境可以使用下面的命令去掉master的taint，使master参与工作负载：
```shell
$ kubectl taint nodes --all node-role.kubernetes.io/master-
 node/lk-thinkpad-t470 untainted
```

### 网络部署
可以选择不同的网络插件，本文介绍calico和flannel。注意，同时只能使用一种网络插件。
> 详细的网络列表可参考插件页面 https://kubernetes.io/docs/concepts/cluster-administration/addons/
``` shell
# flannel
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```
插件安装完成后，可以通过检查coredns pod的运行状态来判断网络插件是否正常运行。等待coredns pod的状态变成Running，就可以继续添加从节点了。

## SLAVE

- 安装好docker、kubelet，
- 执行kubeadm join 加入集群

注意：默认token的有效期为24小时，当过期之后，该token就不可用了。此时可以重新生成token:

```shell
kubeadm token generate
kubeadm token create <generated-token> --print-join-command --ttl=0
```

