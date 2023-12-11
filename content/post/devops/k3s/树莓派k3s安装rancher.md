---
title: 树莓派k3s安装rancher
date: 2020-08-08 16:43:22
tags:
  - 树莓派
  - k3s
  - rancher
categories:
  - DevOps
---
# 树莓派k3s安装rancher
## 安装helm

下载官方安装包 ，注意区分helm3 和helm2
>https://github.com/helm/helm/releases

解压 
```bash
tar -zxvf helm-xxx.tgz
```

移动目录
```bash 
sudo mv helm /usr/local/bin/
```
### 添加heml chart仓库

```bash 
helm repo add rancher-stable http://rancher-mirror.oss-cn-beijing.aliyuncs.com/server-charts/stable
```

## 安装 cert-manager

[官方文档](https://cert-manager.io/docs/installation/kubernetes/#installing-with-helm)

### 安装 CustomResourceDefinition 资源

```bash 
kubectl apply --validate=false -f cert-manager.crds.yaml
```

### 为 cert-manager 创建命名空间


```bash 
kubectl create namespace cert-manager
```

### 添加 Jetstack Helm 仓库

```bash 
helm repo add jetstack https://charts.jetstack.io
```

### 更新本地 Helm chart 仓库缓存

```bash 
helm repo update
```

### 安装 cert-manager Helm chart
```bash 
helm install   cert-manager jetstack/cert-manager   --namespace cert-manager   --version v0.16.0
```