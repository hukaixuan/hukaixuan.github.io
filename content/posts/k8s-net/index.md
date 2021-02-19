---
title: "图解 Kubernetes 网络"
description: "Brief intro to networking of K8s with pictures"
date: "2021-02-19"
slug: "Brief intro to networking of K8s with pictures"
image: "network.jpg"
categories: [
    "技术杂谈"
]
tags: ["Kubernetes", "Docker", "Networking", "图解系列"]
---

### Docker Networking
让我们先大致了解下 Docker Networking    
默认 bridge 模式    
![docker bridge mode](./docker-bridge.png)

通常 Docker 只负责当前同一 host/node 下的容器通信，不去暴露 container ip 给其他 host/node    
K8s 解决的问题之一就是不同 host/node 间的容器通信问题

### IP in Kubernetes
下面大概介绍下 K8s 中的三大类 IP
- **node IP**         
    - 节点 IP，每个 K8s 节点都需要有一个 IP 地址           
    - 其实不属于 K8s 管理的范畴，通过其他方式分配，如：DHCP、手动配置、云厂商分配等
- **pod IP**     
    - 一个 pod 里的多个 containers 共享同一个 network namespace     
    - 每个 pod 会被分配 IP 地址，通过你当前使用的 CNI(Container Network Interface) 插件的 IPAM 功能分配
        - 最基本的方式是通过给每个 node 分配一段 IP 地址段，node 再在这个地址段中分配 IP 给其上的 pod
        - 有些插件会动态分配 IP 段
    - kube-apiserver 通过启动参数 `--cluster-cidr=172.16.0.0.0/16` 来决定所有 pod 的 IP 地址段
- **service IP**
    - K8s service 是对一组 pod 的一个抽象
    - 所有 non-headless service 都有一个 ClusterIP
    - kube-apiserver 通过参数 `--service-cluster-ip-range=172.15.100.0/23` 来决定 ClusterIPs 的 IP 池
    - 通过 apiserver 组件管理分配，告诉 kubelet 进程 serives 与 IP 的映射关系，及 serivce 对应的 endpoints IP（如对应 pod 的 IP）

以上三者关系如图

![K8s IP relationship](./k8s-ip-rel.png)

### 参考
[Tutorial: Communication Is Key - Understanding Kubernetes Networking - Jeff Poole, Vivint Smart Home](https://youtu.be/InZVNuKY5GY)