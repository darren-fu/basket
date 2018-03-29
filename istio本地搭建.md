
# Istio
> an open platform to connect, manage, and secure microservices. Istio provides an easy way to create a network of deployed services with load balancing, service-to-service authentication, monitoring, and more, without requiring any changes in service code. You add Istio support to services by deploying a special sidecar proxy throughout your environment that intercepts all network communication between microservices, configured and managed using Istio’s control plane functionality.



[TOC]

## 安装 k8s

### Minikube

#### 准备工作
- VT-x 或 AMD-v 虚拟化支持必须在电脑的bios中开启
- 安装虚拟机: 对于 Linux, 可以安装 VirtualBox 或 KVM

#### 安装VitualBox
地址: https://www.virtualbox.org/wiki/Downloads
- VirtualBox 5.1.xx platform packages
- VirtualBox 5.1.xx Oracle VM VirtualBox Extension Pack
**5.2在Ubuntu16.04 无法安装成功**
分别安装VitualBox和扩展包

#### 安装kubectl
地址: https://kubernetes.io/docs/tasks/tools/install-kubectl/
```
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl

chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
```
注意：如果更新了Minikube，务必重新再执行上述步骤以便更新kubectl到最新版本，否则可能出现问题，比如minikube dashboard打不开浏览器。
_ _ _

#### 安装Minikube
地址: https://github.com/kubernetes/minikube/releases 查看最新版本
使用: https://github.com/kubernetes/minikube/blob/v0.23.0/README.md
安装命令:
```
curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.23.0/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/

```
如果有更新，修改上面命令中的版本号即可。+
_ _ _

#### 通过minikube运行k8s
参考: https://kubernetes.io/docs/getting-started-guides/minikube/
> 切记要设置代理，否则会因为网络被墙导致无法获取镜像，kubectl get pod 会发现一直阻塞在 status　ContainerCreating． 
注意：这里的代理一定要是http代理，因此不能直接输入shadowsocks的地址，要用pilipo提供的http代理，而且要设置pilipo的proxyAddress，不能只监听127.0.0.1．

**使用自动注入需要启用kuneneats的Initillizers功能**
```
minikube start --docker-env http_proxy=http://192.168.31.152:8123 --docker-env https_proxy=http://192.168.31.152:8123 --docker-env no_proxy=localhost,127.0.0.1,::1,192.168.31.0/24,192.168.99.0/24 --extra-config=apiserver.Admission.PluginNames="Initializers,NamespaceLifecycle,LimitRanger,ServiceAccount,DefaultStorageClass,GenericAdmissionWebhook,ResourceQuota" 

minikube start --docker-env no_proxy=localhost,127.0.0.1,::1,192.168.31.0/24,192.168.99.0/24 --extra-config=apiserver.Admission.PluginNames="Initializers,NamespaceLifecycle,LimitRanger,ServiceAccount,DefaultStorageClass,GenericAdmissionWebhook,ResourceQuota" --extra-config=apiserver.Features.EnableSwaggerUI=true
```
如果没有正确设置代理就执行了 minikube start，则只能删除虚拟机然后重新来过，后面再设置代理是没有效果的

*http://localhost:8000/swagger-ui/*
```
 切换image仓库,修正image pull error:
minikube ssh
docker pull registry.cn-hangzhou.aliyuncs.com/google-containers/kubernetes-dashboard-amd64:v1.7.0
docker tag registry.cn-hangzhou.aliyuncs.com/google-containers/kubernetes-dashboard-amd64:v1.7.0 gcr.io/google_containers/kubernetes-dashboard-amd64:v1.7.0
```

## 安装Istio
### 下载
参考：
http://istio.doczh.cn/docs/setup/kubernetes/quick-start.html
linux:
```
curl -L https://git.io/getLatestIstio | sh -
sudo mv istio-0.2.12/ /usr/local/share/istio
# istio加入Path
export PATH=/usr/local/share/istio/bin:$PATH
```

### 安装istio核心
简单起见，不做双向认证。方便期间，安装istio-initializer以便自动注入envoy。
```
cd /usr/local/share/istio/
kubectl apply -f install/kubernetes/istio.yaml
kubectl apply -f install/kubernetes/istio-initializer.yaml
```
验证:
```
kubectl get svc -n istio-system
kubectl get pods -n istio-system
```
_ _ _

### 安装示例项目 BookInfo
- 将目录更改为 Istio 安装目录的根目录。

- 构建应用程序容器：
如果您使用 自动注入 sidecar 的方式部署的集群，那么只需要使用 kubectl 命令部署服务：
```
 kubectl apply -f samples/bookinfo/kube/bookinfo.yaml
```
如果您使用 手动注入 sidecar 的方式部署的集群，清使用下面的命令：
```
 kubectl apply -f <(istioctl kube-inject -f samples/bookinfo/kube/bookinfo.yaml)
``` 

- 确认所有服务和 pod 已正确定义并运行：
```
 kubectl get services
```
- export 端口
```
export GATEWAY_URL=$(kubectl get po -l istio=ingress -n istio-system -o 'jsonpath={.items[0].status.hostIP}'):$(kubectl get svc istio-ingress -n istio-system -o 'jsonpath={.spec.ports[0].nodePort}')
echo $GATEWAY_URL
```
- 确认已经注入envory
```
echo $(kubectl get deployment reviews-v2 -o jsonpath='{.metadata.annotations.sidecar\.istio\.io\/status}')
# 或者查看完整的的的deployment信息
kubectl get deployment reviews-v2 -o yaml
```


- 验证：
```
kubectl get ingress -o wide
echo ${GATEWAY_URL}
curl -o /dev/null -s -w "%{http_code}\n" http://${GATEWAY_URL}/productpage
# 200
```
你也可以通过在浏览器中打开 http://$GATEWAY_URL/productpage 页面访问 Bookinfo 网页。如果您多次刷新浏览器将在 productpage 中看到评论的不同的版本，它们会按照 round robin（红星、黑星、没有星星）的方式展现，因为我们还没有使用 Istio 来控制版本的路由。
_ _ _

### 监控
#### Grafana
1. kubectl apply -f install/kubernetes/addons/prometheus.yaml
2. kubectl apply -f install/kubernetes/addons/grafana.yaml
3. kubectl -n istio-system get svc grafana
4. kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=grafana -o jsonpath='{.items[0].metadata.name}') 3000:3000 &
5. http://localhost:3000/dashboard/db/istio-dashboard

6. kubectl port-forward -n istio-system $(kubectl get pod -n istio-system -l app=zipkin -o jsonpath='{.items[0].metadata.name}') 9411:9411 &


_ _ _

### 卸载 Kubernetes 环境
- 删除路由规则，终止应用程序 pod
```
samples/bookinfo/kube/cleanup.sh
```
- 确认关闭
```
istioctl get routerules   #-- there should be no more routing rules
kubectl get pods          #-- the BookInfo pods should be deleted
```

- 清理路由规则
```
istioctl delete -f samples/bookinfo/kube/route-rule-all-v1.yaml
istioctl delete -f samples/bookinfo/kube/route-rule-reviews-test-v2.yaml
```

- 删除 istio
```
kubectl delete -f install/kubernetes/istio-initializer.yaml
kubectl delete -f install/kubernetes/istio.yaml
```
- 清除可能还在运行的kubectl继承
```
killall kubectl
```










## 常用命令
#### docker
- docker build -t xxx/image_name:tag_version .
- docker push xxx/image_name:tag_version
- docker run -p host_port:container_port image_name
- docker exec -it 24ea7afc0059 /bin/bash

#### kubectl
- kubectl get pod
- kubectl get pods --all-namespaces
- kubectl get pods -n istio-system
- kubectl get service
- kubectl get svc -n istio-system
- kubectl describe pod hello-minikube-180744149-lj0rd
- kubectl describe --namespace=kube-system pod kube-addon-manager-minikube

#### minikube
- minikube stop
- minikube delete
- minikube dashboard
- minikube status
- minikube service hello-minikube --url
- curl $(minikube service hello-minikube --url)



## 示例command
- kebu kubectl get pods --all-namespaces(pod状态)
- kubectl get service --all-namespaces(service状态)
- 修正image pull error
 minikube ssh
docker pull registry.cn-hangzhou.aliyuncs.com/google-containers/kubernetes-dashboard-amd64:v1.7.0
docker tag registry.cn-hangzhou.aliyuncs.com/google-containers/kubernetes-dashboard-amd64:v1.7.0 gcr.io/google_containers/kubernetes-dashboard-amd64:v1.7.0
- kubectl describe --namespace=kube-system pod kube-addon-manager-minikube
- kubectl run hello-minikube --image=registry.cn-hangzhou.aliyuncs.com/google-container/echoserver:1.4 --port=8080

## 参考链接
- [官方Github](https://github.com/istio/istio)
- [Istio官方文档](https://istio.io/docs/concepts/what-is-istio/overview.html)
- [Istio官方安装教程](https://istio.io/docs/setup/)
- [Istio官方文档中文版](http://istio.doczh.cn/)
- [敖小剑分享Istio安装](https://skyao.gitbooks.io/learning-istio/installation/)
- [敖小剑分享Minikube安装](https://skyao.gitbooks.io/learning-kubernetes/installation/minikube.html)
- [中文简介 - 数人云](http://blog.csdn.net/shurenyun/article/details/78066126)
- [模式-ServiceMesh](http://www.sohu.com/a/198655597_467759)
- [Enovy-介绍](http://blog.csdn.net/zvayivqt0ufji/article/details/78351355)

