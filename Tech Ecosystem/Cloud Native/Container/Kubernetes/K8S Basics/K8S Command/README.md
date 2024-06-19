# K8S的命令很多，这里挑重点讲

## 集群
#### 查看集群 <font color="maroon">kubectl config get-contexts</font>
#### 切换集群 <font color="maroon">kubectl config use-context</font>
```go
kubectl config use-context k8s //切换到名称为：k8s的集群
```
## RBAC
#### 创建clusterrole
<font color="maroon">kubectl create clusterrole <font color="blue">\<role-name\> </font>  <font color="blue">--verb</font>=create <font color="blue">--resource</font>=deployments,statefulsets,daemonsets</font>

```word
备注：--verb 参数用于指定集群角色可以执行的动作。这些动作定义了集群角色对资源的访问权限。
以下是 Kubernetes 支持的一些常见谓词：

get: 读取资源信息。
list: 列出所有资源。
watch: 监听资源变化。
create: 创建新资源。
update: 更新现有资源。
patch: 部分更新现有资源。
delete: 删除资源。
deletecollection: 删除资源的集合。
exec: 在容器上执行命令。
connect: 允许连接到资源，如使用 kubectl port-forward。
proxy: 提供对资源的代理访问。
```

```word
--resource 参数用于指定集群角色可以访问的资源类型。Kubernetes 支持大量的资源类型，涵盖了从基本的 Pod、Service、Deployment 到更高级的 CustomResourceDefinition（CRD）等。

以下是一些常见的 Kubernetes 资源类型，它们都可以作为 --resource 参数的值：

pods：Pod 是 Kubernetes 中的最小部署单元，包含一个或多个容器。
services：Service 用于定义如何访问一组 Pod，通常用于负载均衡。
deployments：Deployment 用于声明式地更新应用，它管理 Pod 和 ReplicaSet。
replicasets：ReplicaSet 确保指定数量的 Pod 副本在运行。
statefulsets：StatefulSet 用于管理有状态的应用。
daemonsets：DaemonSet 确保每个节点上都运行一个 Pod 的副本。
jobs：Job 用于创建一次性的任务，确保它们完成。
cronjobs：CronJob 用于创建基于时间的任务，类似于 Unix 的 crontab。
configmaps：ConfigMap 用于存储配置信息，可以被 Pod 中的容器使用。
secrets：Secret 用于存储敏感信息，如密码、令牌等。
persistentvolumes 和 persistentvolumeclaims：用于管理持久化存储。
ingresses：Ingress 用于外部访问集群内的服务，通常与负载均衡器一起使用。
storageclasses：StorageClass 为管理员定义了如何动态地提供存储。
roles 和 clusterroles：用于定义角色和集群角色的权限。
rolebindings 和 clusterrolebindings：用于绑定角色和集群角色到用户或组。
endpoints：Endpoints 是 Service 和 Pod 之间的连接点。
events：Event 表示集群中发生的情况。
namespaces：Namespace 用于将集群资源划分到不同的虚拟集群中。
customresourcedefinitions (CRDs)：CRD 允许用户扩展 Kubernetes API 以定义自己的资源类型。
这只是一部分资源类型的示例，Kubernetes 还支持更多资源类型，并且随着版本的更新，可能会有新的资源类型被添加。你可以通过运行 kubectl api-resources 命令来查看当前 Kubernetes 集群支持的所有资源类型。
```

#### 创建serviceaccount
<font color="maroon">kubectl -n <font color="blue"> \<namespace\></font>  create serviceaccount <font color="blue">\<account-name\></font></font>

#### 查看serviceaccount
kubectl get serviceaccounts

#### 创建rolebinding
<font color="maroon">kubectl -n <font color="blue"> \<namespace\></font> create rolebinding <font color="blue">\<bindingname\></font> 
<font color="blue">--clusterrole</font>=\<rolename\> <font color="blue">--serviceaccount</font>=<b>\<namespace\>:\<account-name\></b></font>

```
-n可以写在后面，不过建议写在前面，很容易遗漏
```

#### 查看rolebinding <font color="maroon">kubectl -n <font color="blue"> \<namespace\></font> describe rolebinding <font color="blue"> \<rolebinding-name\></font></font>

## 节点 Node
#### 查看所有node <font color="maroon">kubectl get nodes</font>
#### 查看node是否包含标签 <font color="maroon">kubectl get nodes --show-labels|grep '\<label-name\>=\<label-value\>'</font>
#### 查看node（以yaml格式展示) <font color="maroon">kubectl get node \<node-name\> -o yaml</font> 

#### 编辑node <font color="maroon">kubectl edit node \<node-name\> </font>

#### 给node打标签 <font color="maroon">kubectl label nodes \<node-name\> \<label-name\>=\<label-value\></font>

#### 查看node是否打了污点 kubectl describe node \<node-name\> | grep -i \<taint-name\>
#### 给node设置污点 kubectl taint nodes \<node-name\> <\key\>=<\value\>:<\effect\>
effect包括：NoSchedule,PreferNoSchedule, NoExecute三种
#### 查看所有node名字和node标签的对应情况
kubectl describe nodes | grep -i -e Taints -e 'Hostname:'
备注-e 代表单匹配 两个-e代表同时匹配 -E 代表正则匹配
#### 暂停node调度 <font color="maroon">kubectl cordon \<node-name\></font>
#### 驱逐（腾空）node <font color="maroon">kubectl drain \<node-name\> --ignore-daemonsets</font>
#### 恢复node调度 <font color="maroon">kubectl uncordon \<node-name\></font>

```
备注：
kubectl cordon 只将节点标记为不可调度状态，但不会影响已经在该节点上运行的 Pod。
kubectl drain 会排空节点（逐出所有 Pod）并设置节点为不可调度状态。这是一个更彻底的操作，用于从集群中完全移除或维护节点。
```

```yaml
#Node Example
{
  "kind": "Node",
  "apiVersion": "v1",
  "metadata": {
    "name": "10.240.79.157",
    "labels": {
      "name": "my-first-k8s-node"
    }
  }
}
```


## 命名空间
### 备注：namespace 别名 ns
#### 查看所有命名空间 <font color="maroon">kubectl get namespaces</font>
#### 创建命名空间  <font color="maroon">kubectl create namespace <font color="blue">\<namespace-name\></font></font>
#### 查看命名空间labels  <font color="maroon">kubectl get ns <font color="blue">\<namespace-name\> --show-labels</font></font> 
#### 给命名空间打标签 <font color="maroon">kubectl label ns <font color="blue">\<namespace-name\>  \<label-name\>=\<label-value\> </font></font> 


## 工作负载 Workload
## POD
#### 获取特定标签的pod <font color="maroon">kubectl get pod -l <font color="blue">\<label-name\>=\<label-value\></font></font>
#### 获取所有命名空间下的pod <font color="maroon">kubectl get pods -A</font>
#### 根据名字查找pod <font color="maroon">kubectl get pod -A|grep nginx-kusc</font>
#### 获取pod标签 <font color="maroon">kubectl get pod <font color="blue">--show-labels</font></font>
#### 获取默认namespace下的pod <font color="maroon">kubectl get pods 或kubectl get pod</font> 
#### 获取所有pod <font color="maroon">kubectl get pod -A或--all-namespaces</font>
#### 命令行创建pod <font color="maroon">kubectl run \<node-name\> --image=\<image-name\> --dry-run=client -o yaml > pod.yaml</font>
备注：这种方式可以先简单生成pod，再基于yaml文件修改即可。
#### 查看挂载在某NODE下的POD kubectl get pod -A --field-selector spec.nodeName=\<node-name\> 

```yaml
#Pod Example
apiVersion: v1
kind: Pod
metadata:
 name: kucc8
spec:
  volumes:
    - name: task-pv-storage
      persistentVolumeClaim:
        claimName: task-pv-claim #pvc name
    - name: varlog
      emptyDir: {}    
  containers:
    - name: nginx
      image: nginx
      imagePullPolicy: IfNotPresent
    - name: memcached
      image: memcached
      imagePullPolicy: IfNotPresent
    - name: task-pv-container
      image: nginx
      ports:
        - containerPort: 80
          name: "http-server"
      volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: task-pv-storage
    #此处的sidecar只是演示效果
    - name: sidecar
      image: busybox
      imagePullPolicy: IfNotPresent
      args: [/bin/sh, -c, 'tail -n+1 f /var/log/11-factor-app.log'] 
      volumeMounts:
        - mountPath: /var/log
          name: varlog
  nodeSelector: #指定需要调度的节点 此处配置需要和node中的labels对应上
    disk: ssd 
```

备注：volumeMounts和volume在Kubernetes中协同工作，实现了容器内部文件和数据的持久化存储和管理。volume提供了数据的存储位置，而volumeMounts则负责将数据存储位置挂载到容器内部的目录上，供容器使用。

```
emptyDir：
emptyDir是一个临时存储卷，它的生命周期与Pod的生命周期绑定。当Pod被删除时，emptyDir中的数据也会被删除。
它主要用于Pod中容器之间的数据共享。因为当Pod被调度到某个节点时，emptyDir卷会被创建在节点上，并且可以被Pod中的多个容器共享。
由于emptyDir的生命周期与Pod绑定，所以它不适合用于需要持久化存储的场景。

hostPath：
hostPath类型的存储卷将工作节点上某文件的目录或文件挂载于Pod中。这意味着Pod可以直接访问宿主机上的文件系统。
它允许Pod访问宿主机上的文件或目录，这使得Pod可以读取或写入宿主机上的数据。
hostPath具有持久性，因为它直接关联到宿主机的文件系统。但是，由于它是工作节点本地的存储空间，所以它仅适用于特定情况下的存储卷使用需求，比如系统级Pod资源需要访问节点上的文件。
hostPath的一个主要缺点是它无法实现跨节点Pod之间的数据共享。

persistentVolumeClaim（PVC）：
PVC是Kubernetes中的一种存储请求机制，它允许用户声明需要什么样的存储资源，而不是直接定义存储资源。
PVC通常与PersistentVolume（PV）一起使用。PV是由集群管理员配置提供的某存储系统上的一段存储空间，它是对底层共享存储的抽象。
用户通过PVC向PV申请特定大小的空间及访问模式（如读写或只读），从而创建出PVC存储卷。Pod资源可以通过PVC存储卷关联使用PV提供的存储空间。
PVC和PV实现了“存储消费”机制，使得存储资源可以像其他Kubernetes资源一样被动态地申请和使用。
PVC和PV通常用于需要持久化存储的场景，比如数据库、文件系统等。
```

## 工作负载-资源 Workload Resources
### Deployments 用于部署和管理pod的一种控制器
#### 获取所有deployment <font color="maroon">kubectl get deployments</font>
#### 获取特定deployment <font color="maroon">kubectl get deployments <font color="blue">\<deploy-name\></font></font>

#### 修改deployment <font color="maroon">kubectl edit deployment <font color="blue">\<deploy-name\></font></font>
#### 查看特定deployment <font color="maroon">kubectl get deployment <font color="blue">\<deploy-name\></font> -o wide</font>

#### 扩容deployment <b><font color="maroon">kubectl scale deployment</b> <font color="blue">\<deploy-name\> --replicas=\<number\></font></font>

```yaml
#Deplyment Example
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec: # 整个Deployment的配置和期望状态
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template: # Pod实例的具体规格和配置
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - name: http #名为http的端口规范
          containerPort: 80
          protocol: TCP

```


## 网络策略 NetworkPolicy
#### 从yaml创建 <font color="maroon">kubectl apply -f network-policy.yaml</font>
#### 检查网络策略 <font color="maroon">kubectl describe networkpolicy -n <font color="blue">\<networkpolicyname\></font></font>


```yaml
#Network Policy Example 主要是设置被访者的配置，包括被访者命名空间，端口等。
#matchLabels代表只允许某些特定标签的pod入站访问
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-port-from-namespace
  namespace: my-app #被访问者的命名空间（echo 访问 my-app）
spec:
  podSelector: {} #全局定义，{}表示不选择任何Pod
  policyTypes:
  - Ingress #策略影响入栈流量
  ingress:
  - from: #允许流量的来源
    - namespaceSelector:
        matchLabels:
          project: echo #只允许标签为"project:echo"的Pod入站访问
    ports:
    - protocol: TCP
      port: 9000 #被访问者公开的端口
```
备注:podSelector: {}表示不直接选择任何Pod，但可以通过ingress和egress规则来定义特定的网络流量模式。

## 服务 Service(SVC) Service用于定义如何访问一组特定pod的抽象层(其实就是一个带Pod自动注册的网络代理，统一网关之类的概念)
#### 暴露对应端口 <font color="maroon">kubectl expose deployment <font color="blue">\<deploy-name\></font> <font color="blue">--type</font>=NodePort <font color="blue">--port</font>=80 <font color="blue">--target-port</font>=80 <font color="blue">--name=</font><b>\<service-name\></b></font>
<font color="red">备注：service的创建有点不同，并不是使用的kubectl create。而是通过expose deployment实现。</font>

#### 查看所有service <font color="maroon">kubectl get svc -o wide</font>
#### 查看特定service <font color="maroon">kubectl get svc <font color="blue">\<service-name\></font> -o wide</font>
#### 编辑service <font color="maroon">kubectl edit svc <font color="blue">\<service-name\></font></font>

```yaml
#Service Example
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app.kubernetes.io/name: MyApp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
```

## Ingress 类似一个基于http或https的负载均衡
#### 查看ingressclass <font color="maroon">kubectl get ingressclass <font color="blue">\<namespace\></font></font>

备注：如果不指定namespace，则使用默认的命名空间。

#### 查看特定ingress <font color="maroon">kubectl get ingress -n  <font color="blue">\<namespace\> </font></font>
#### 查看特定ingress的yaml <font color="maroon">kubectl get ingress \<ingress-name\> -n <font color="blue">\<namespace\> </font> -o yaml</font>
#### 从yaml创建 <font color="maroon">kubectl apply -f ingress.yaml</font>
#### 创建ingress <font color="maroon">kubectl create ingress <font color="blue">\<ingress-name\></font>  <font color="blue">--rule</font>=/hello=hello:5678 <font color="blue">--class</font>=nginx <font color="blue">--annotation</font>=nginx.ingress.kubernetes.io/rewrite-target=/ <font color="blue">-n \<namespace\></font> </font>

```yaml
#Ingress example
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: minimal-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: / #发到nginx的请求被重写为根路径
spec:
  ingressClassName: nginx-example
  rules: #ingress规则
  - http:
      paths:
      - path: /testpath
        pathType: Prefix #除了Prefix，还支持:Exact,ImplementationSpecific 
        backend: #目前只支持指定service，不支持直接连pod
          service:
            name: test ##名为test的service
            port:
              number: 80

```
备注：Ingress的pathType主要有以下几种：

Prefix：基于路径前缀的匹配。如果请求的URL路径以指定的path值开始，则匹配成功。这是最常用的匹配方式。

Exact：完全匹配。请求的URL路径必须完全与指定的path值匹配，才会被路由到相应的后端服务。

ImplementationSpecific：实现特定。这个值允许Ingress控制器实现自己的匹配逻辑。不同的Ingress控制器可能会有不同的实现方式，因此使用这种pathType需要确保你了解你所使用的Ingress控制器的具体实现。

## 存储 Storage
### PV Persistent Volumes
#### 获取pv <font color="maroon">kubectl get pv</font>

```yaml
#PV Example
apiVersion: v1
kind: PersistentVolume
metadata:
  name: app-config
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteMany 
  hostPath:
    path: "/srv/app-config"
```
### PVC Persistent Volume Claim
#### 获取PVC <font color="maroon">kubectl get pvc</font>
#### 更改PVC <font color="maroon">kubectl edit pvc \<pvc-name\> <font color="blue">--record</font></font>
```yaml
#PVC Example
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pv-volume
spec:
  storageClassName: csi-hostpath-sc
  accessModes:
    - ReadWriteOnce
 resources:
   requests:
     storage: 10Mi
```

备注：
一旦PVC和PV都创建好了，Kubernetes系统会自动将PVC绑定到最合适的PV上。这个过程是通过调度器（Scheduler）来完成的，调度器会根据PVC的需求和PV的可用性进行匹配。具体来说，如果PVC指定了storageClassName，那么调度器会尝试找到一个具有相同storageClassName的PV进行绑定。如果PVC没有指定storageClassName，那么调度器会尝试找到一个没有StorageClass的PV进行绑定。

PVC的重点是申请多少资源。而PV的侧重点是定义多少资源。两者相辅相成。

## 日志
#### <font color="maroon">kubectl logs <font color="blue">\<pod-name\></font> | grep <font color="blue">"\<search-condition\>"</font> > \<path\></font>

## 其他命令
### 资源使用率 kubectl top
#### <font color="maroon">kubectl top pod <font color="blue">\<pod-name\></font> -l <font color="blue">\<label-name\> --sort-by</font>=cpu -A</font>
#### <font color="maroon">kubectl top node <font color="blue">\<node-name\></font> -l <font color="blue">\<label-name\> --sort-by</font>=memory -A</font>

### systemctl
#### systemctl status kubelet
#### systemctl start kubelet
#### systemctl enable kubelet

### ETCD

#### etcd备份
ETCDCTL_API=3 etcdctl snapshot save --endpoint=https://127.0.0.1:2378 --cacert="/opt/KUIN/ca.crt" --cert="/opt/KUIN/etcd-client.crt" --key="/opt/KUIN/etcd-client.key" /var/lib/backup/etcd-snapshot.db

#### 查看etcd备份快照
ETCDCTL_API=3 etcdctl snapshot status /var/lib/backup/etcd-snapshot.db -wtable

#### etcd还原
ETCDCTL_API=3 etcdctl --endpoint=https://127.0.0.1:2378 snapshot restore /data/backup/etcd-snapshot-previous.db --data-dir=/var/lib/etcd-restore