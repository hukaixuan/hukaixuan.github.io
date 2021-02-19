---
title: "图解 Kubernetes 网络"
description: "Brief intro to networking of K8s with pictures"
date: "2021-02-19"
slug: "Brief intro to networking of K8s with pictures"
image: "network.jpg"
categories: [
    "技术杂谈"
]
tags: ["Kubernetes", "Docker", "Networking"]
---

### Docker Networking
让我们先大致了解下 Docker Networking    
默认 bridge 模式    
![docker bridge mode](./docker-bridge.png)

通常 Docker 只负责当前同一 host/node 下的容器通信，不去暴露 container ip 给其他 host/node    
K8s 解决的问题之一就是不同 host/node 间的容器通信问题

### IP in Kubernetes
#### node IP

#### pod IP

#### service IP


