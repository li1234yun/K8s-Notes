# configMap

[configMap介绍](https://kubernetes.io/zh/docs/concepts/configuration/configmap/)
[具体配置教程](https://kubernetes.io/zh/docs/tasks/configure-pod-container/configure-pod-configmap/)

注意事项:
- configMap名称须为合法的DNS域名
- 键名须由字母/数字/-/_/.构成
- 不可变属性：`immutable: true`

## 通过命令行创建

查看configMap配置
> kubectl get configmaps name -o json/yaml
> kubectl describe configmap name

- from files
  - 默认使用文件名作为键名
  - 可以同时指定多个文件
  - 保留文件中的引号
  
  > kubectl create configmap name --from-file[=alias_key_name]=file_path --from-file=...

- from env file 
  - 只使用最后一个指定的配置文件
  
  > kubectl create configmap name --from-env=file_path ...

- from literal

  > kubectl create configmap name --from-literal=x1=x2 --from-literal=a1=a2 ...

## 通过configMapGenerator生成

创建名为`kustomization.yml`文件，通过命令行`kubectl apply -k .`进行创建
```
configMapGenerator:
- name: game-config-8
  files:
    - configure-pod-container/configmap/game.properties
    - custom-ui=configure-pod-container/configmap/ui.properties
  literals:
    - special.how=very
    - special.type=charm
```

## 使用场景
- 用于容器环境变量值的设置

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: dapi-test-pod
spec:
  containers:
    - name: test-container
      image: k8s.gcr.io/busybox
      command: [ "/bin/sh", "-c", "ls /etc/config/" ]
      volumeMounts:
      - name: config-volume
        mountPath: /etc/config
  volumes:
    - name: config-volume
      configMap:
        # Provide the name of the ConfigMap containing the files you want
        # to add to the container
        name: special-config
  restartPolicy: Never
```

- 设置为容器的配置挂载文件
  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: dapi-test-pod
spec:
  containers:
    - name: test-container
      image: k8s.gcr.io/busybox
      command: [ "/bin/sh","-c","cat /etc/config/keys" ]
      volumeMounts:
      - name: config-volume
        mountPath: /etc/config
  volumes:
    - name: config-volume
      configMap:
        name: special-config
        items:
        - key: SPECIAL_LEVEL
          path: keys
  restartPolicy: Never
```
