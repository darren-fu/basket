## 总体架构
> [k8s中文文档](https://jimmysong.io/kubernetes-handbook/concepts/)
 
![k8s](https://raw.githubusercontent.com/darren-fu/scrip-paper/master/imgs/k8s.jpg)

---
### 整体
![whole](https://raw.githubusercontent.com/darren-fu/scrip-paper/master/imgs/kubernetes-whole-arch.png)

### Master
![master](https://raw.githubusercontent.com/darren-fu/scrip-paper/master/imgs/kubernetes-master-arch.png)

### Node
![node](https://raw.githubusercontent.com/darren-fu/scrip-paper/master/imgs/kubernetes-node-arch.png)

### 分层
![node](https://raw.githubusercontent.com/darren-fu/scrip-paper/master/imgs/kubernetes-layers-arch.png)

- 核心层：Kubernetes最核心的功能，对外提供API构建高层的应用，对内提供插件式应用执行环境
- 应用层：部署（无状态应用、有状态应用、批处理任务、集群应用等）和路由（服务发现、DNS解析等）
- 管理层：系统度量（如基础设施、容器和网络的度量），自动化（如自动扩展、动态Provision等）以及策略管理（RBAC、Quota、PSP、NetworkPolicy等）
- 接口层：kubectl命令行工具、客户端SDK以及集群联邦
- 生态系统：在接口层之上的庞大容器集群管理调度的生态系统，可以划分为两个范畴
  - Kubernetes外部：日志、监控、配置管理、CI、CD、Workflow、FaaS、OTS应用、ChatOps等
  - Kubernetes内部：CRI、CNI、CVI、镜像仓库、Cloud Provider、集群自身的配置和管理等

