---
title: 使用k8s
tags: k8s
categories: 技术
abbrlink: 27141bca
date: 2018-09-26 00:00:00
---
## 起源

1.工作中测试和生产使用了k8s环境。  
2.k8s集群资源利用率高，对docker容器管理方便。

## 流程简介

目前所熟悉的开发到部署的流程。流程如下：  
```
本地 --> gitlab --> jenkins --> k8s  
```
1.&ensp;本地开发完成后推送到gitlab。  
2.&ensp;jenkins 使用pipeline-Script 编写脚本。(主要是使用脚本实现)  
&ensp;&ensp; 2.1 获取gitlab代码。获取配置文件。  
&ensp;&ensp; 2.2 根据配置文件tag，编译docker镜像。  
&ensp;&ensp; 2.3 登录远程镜像仓库，push镜像。  
&ensp;&ensp; 2.4 ssh自动登录k8s master节点，kubectl set image从远程镜像仓库滚动更新k8s容器镜像。  
3.&ensp;k8s 运行deployment和service。  
## k8s使用简介

建议新手自搭k8s集群。  
初始化创建，通过yaml创建deployment。  
Deployment代码redis-test-deploy.yaml实例：  
```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: nginx
spec:
  replicas: 3
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx #可以使用自己的web镜像仓库
        ports:
        - containerPort: 80
```
kubectl create -f redis-test-deploy.yaml  
Service代码redis-test-svc.yaml实例：  
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort #访问方式
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
```
type:外部访问方式 [原文链接](https://medium.com/google-cloud/kubernetes-nodeport-vs-loadbalancer-vs-ingress-when-should-i-use-what-922f010849e0) [译文链接](http://dockone.io/article/4884)  
kubectl create -f redis-test-svc.yaml  

自此，整个简单的开发到部署k8s流程已经简要介绍。后续还有许多将深入学习。  

## 感谢

[http://dockone.io/article/4884](http://dockone.io/article/4884)  
[https://kubernetes.io/docs/tutorials/kubernetes-basics](https://kubernetes.io/docs/tutorials/kubernetes-basics/)
