# Ingress

ingress是公开从集群外部到集群内部服务的HTTP/HTTPS路由。
流量路由由ingress资源规则控制。

## ingress资源是一种服务路由规则，但实际却不起作用，更类似于路由指导配置

ingress规则：
- 可选host
- 路径列表
- 后端服务

## DefaultBackend
所有不匹配的流量入口

## 主机名通配符


