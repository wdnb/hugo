---
title: 在 GitLab 项目中使用 k3s Kubernetes 集群
date: 2020-07-28 23:37:22
tags:
  - gitlab
  - 树莓派
  - k3s
categories:
  - DevOps
---

参考这篇文章，比较全面
>https://medium.com/better-programming/using-a-k3s-kubernetes-cluster-for-your-gitlab-project-b0b035c291a9

## 获取API URL

默认的是https://127.0.0.1:6443 可以选择把端口映射到外网

## 获取Cluster’s CA certificate
```
kubectl config view --raw \
-o=jsonpath='{.clusters[0].cluster.certificate-authority-data}' \
| base64 --decode
```
## 获取Service token

### 创建一个 ServiceAccount，并为其提供集群管理角色
```
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: ServiceAccount
metadata:
  name: gitlab-admin
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: gitlab-admin
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: gitlab-admin
  namespace: kube-system
EOF
```
### 往环境变量添加 SECRET
`SECRET=$(kubectl -n kube-system get secret | grep gitlab-admin | awk '{print $1}')`

### 往环境变量添加 TOKEN 并且 提取与 SECRET 关联的 JWT 令牌:
`TOKEN=$(kubectl -n kube-system get secret $SECRET -o jsonpath='{.data.token}' | base64 --decode)`
`echo $TOKEN`

## 现在我们使用所有的信息并填写 GitLab 的 Add existing cluster 表单