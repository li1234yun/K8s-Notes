# kubernetes config

[DOC](https://kubernetes.io/zh/docs/concepts/configuration/organize-cluster-access-kubeconfig/)

配置的作用：用于控制集群的功能。

默认路径：$HOME/.kube/config 
环境变量：KUBECONFIG，当存在时，与默认配置合并
命令行指定：--kubeconfig 仅使用此配置


配置使用指定上下文
> kubectl config use-context contextName

查看合并后的配置
> kubectl config view


配置合并规则：
