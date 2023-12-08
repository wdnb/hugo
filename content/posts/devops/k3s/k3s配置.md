---
title: k3s配置
date: 2020-07-29 23:37:22
tags:
  - 树莓派
  - k3s
categories:
  - DevOps
---
# k3s配置

## kubectl 指令

### 查看k3s节点
```bash
sudo k3s kubectl get nodes
```

### 查看k3s节点详细
```bash
kubectl get nodes -o wide
```

### 查看所有namespace
```bash
kubectl get pods --all-namespaces
```

### 删除 namespace
```bash
kubectl delete namespaces cattle-system
```

## helm 指令

## k3s指令

### 重启k3s 
```bash
sudo systemctl restart k3s
```

### 卸载k3s
```bash
/usr/local/bin/k3s-uninstall.sh (or as k3s-agent-uninstall.sh)
```

## 树莓派安装k3s前准备配置

### Raspbian Buster 需要使用 legacy iptables
参考官方文档进行操作
>https://rancher.com/docs/k3s/latest/en/advanced/#enabling-legacy-iptables-on-raspbian-buster

Could not set up iptables canary mangle/KUBE-KUBELET-CANARY: error creating chain "KUBE-KUBELET-CANARY": exit status 3: iptables v1.8.3 (legacy): can't initialize iptables table `mangle': Table does not exist (do you need to insmod?)

## 修改树莓派主机名
所有节点不能具有相同的主机名
>https://blog.jbface.com/posts/cfe260ea.html#%E6%B7%BB%E5%8A%A0hosts%E5%9F%9F%E5%90%8D%E5%92%8Cip%E5%AF%B9%E5%BA%94%E5%85%B3%E7%B3%BB

### cgroup设置

```bash
Jul 27 14:55:53 raspberrypi k3s[849]: time="2020-07-27T14:55:53.565642894+01:00" level=error msg="Failed to find memory cgroup, you may need to add \"cgroup_memory=1 cgroup_enable=memory\" to your linux cmdline (/boot/cmdline.txt on a Raspberry Pi)"
```


#### 编辑/boot/cmdline.txt

增加 cgroup_memory=1 cgroup_enable=memory 并重启系统
```bash
sudo vim /boot/cmdline.txt
```
看是否有cgroup_memory字段
```bash
cat /proc/cmdline | grep cgroup_memory
``` 
看有没有memory 字段
```bash
cat /proc/self/cgroup
```

这里注意！当前时间：2020/07/27
树莓派3b用的Raspbian不支持内存 cgroup
参照这个issue
>https://github.com/raspberrypi/linux/issues/3644

用老版内核解决
```bash
wget http://archive.raspberrypi.org/debian/pool/main/r/raspberrypi-firmware/raspberrypi-bootloader_1.20200601-1_armhf.deb
wget http://archive.raspberrypi.org/debian/pool/main/r/raspberrypi-firmware/raspberrypi-kernel_1.20200601-1_armhf.deb
wget http://archive.raspberrypi.org/debian/pool/main/r/raspberrypi-firmware/libraspberrypi-bin_1.20200601-1_armhf.deb
wget http://archive.raspberrypi.org/debian/pool/main/r/raspberrypi-firmware/libraspberrypi-dev_1.20200601-1_armhf.deb
wget http://archive.raspberrypi.org/debian/pool/main/r/raspberrypi-firmware/libraspberrypi-doc_1.20200601-1_armhf.deb
wget http://archive.raspberrypi.org/debian/pool/main/r/raspberrypi-firmware/libraspberrypi0_1.20200601-1_armhf.deb
sudo dpkg -i *deb
```

### 执行kubectl时加载配置文件 /etc/rancher/k3s/k3s.yaml 时没有权限
WARN[2020-07-26T10:27:21.868999680+01:00] Unable to read /etc/rancher/k3s/k3s.yaml, please start server with --write-kubeconfig-mode to modify kube config permissions 
```bash
sudo su
echo "K3S_KUBECONFIG_MODE=\"644\"" >> /etc/systemd/system/k3s.service.env
sudo systemctl restart k3s
```


### helm install 报错 Error: Kubernetes cluster unreachable
执行
```bash
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
```


## Inbound Rules for K3s Server Nodes
|PROTOCOL|PORT|SOURCE|DESCRIPTION|
|---|---|---|---|
|TCP|6443|K3s agent nodes|Kubernetes API|
|UDP|8472|K3s server and agent nodes|Required only for Flannel VXLAN|
|TCP|10250|K3s server and agent nodes|kubelet|

## 设置国内加速镜像

### 查看镜像信息

```bash
crictl info | grep registry
```

k3s 会在目录 /var/lib/rancher/k3s/agent/etc/containerd 下创建一个 config.toml 文件作为 containerd 的配置文件，我们要做的就是，在同目录下把这个文件复制出来一个 config.toml.tmpl 文件，然后添加镜像源相关的配置进去

```bash
sudo cp /var/lib/rancher/k3s/agent/etc/containerd/config.toml /var/lib/rancher/k3s/agent/etc/containerd/config.toml.tmpl
```
```bash
sudo vim /var/lib/rancher/k3s/agent/etc/containerd/config.toml.tmpl
```

### 在 config.toml.tmpl 文件中添加

```bash
[plugins.cri.registry.mirrors]
  [plugins.cri.registry.mirrors."docker.io"]
    endpoint = ["https://docker.mirrors.ustc.edu.cn"]

```

### 重启服务
重启后生效
```bash
systemctl restart k3s
```