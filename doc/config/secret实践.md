# Secret

不同类型secret的创建

## Opaque Secret

- type: Opaque
- 

## Service Accout Secret

- type: kubernetes.io/service-account-token
- metadata annotations 包含`kubernetes.io/service-account-name`键值

## Docker Config Secret

- type: kubernetes.io/dockerconfigjson 
- data包含`.dockerconfigjson`键

## Basic Secret

- type: kubernetes.io/basic-auth
- 包含`username`/`password`字段

## SSH Secret

- type: kubernetes.io/ssh-auth 
- data包含`ssh-privatekey`键


## TLS Secret

- type: kubernetes.io/tls
- data需包含`tls.crt`和`tls.key`字段
  
> kubectl create secret tls my-tls-secret --cert=... --key=...

## 管理secret

### 创建secret
- 命令行
- 配置文件
- kustomize方式

### 使用secret

- 以挂载卷的方式使用
- 以环境变量方式使用

## 安全设计

1. 仅对挂载到pod secret卷的容器可以访问secret
2. secret被kubelet存储在tmpfs中，pod删除时secret副本被删除
3. secret是pod间互相隔离
4. 用户和API服务之间的通信进行加密

## 风险规避

1. 开启静态加密
2. 限制etcd的访问
3. etcd间通信使用加密
4. 注意应用程序对secret的操作
5. 避免用户通过操作pod导致secret的泄露
6. 限制任何节点root用户模拟kubelet获取所有的secrets

