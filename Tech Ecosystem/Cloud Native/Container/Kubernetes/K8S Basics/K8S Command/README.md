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

#### 创建serviceaccount
<font color="maroon">kubectl -n <font color="blue"> \<namespace\></font>  create serviceaccount <font color="blue">\<account-name\></font></font>

#### 创建rolebinding
<font color="maroon">kubectl -n <font color="blue"> \<namespace\></font> create rolebinding \<bindingname\> 
<font color="blue">--clusterrole</font>=\<rolename\> <font color="blue">--serviceaccount</font>=<font color="blue">\<namespace\>:\<account-name\></font></font>

## 命名空间
### 备注：namespace 别名 ns
#### 查看所有命名空间 <font color="maroon">kubectl get namespaces</font>
#### 创建命名空间  <font color="maroon">kubectl create namespace <font color="blue">\<namespace-name\></font></font>
#### 查看命名空间labels  <font color="maroon">kubectl get ns <font color="blue">\<namespace-name\> --show-labels</font></font> 
#### 给命名空间打标签 <font color="maroon">kubectl label ns <font color="blue">\<namespace-name\>  \<label-name\>=\<label-value\> </font></font> 


## 工作负载 Workload
## POD
#### 获取特定标签的pod <font color="maroon">kubectl get pod -l <font color="blue">\<label-name\>=\<label-value\></font></font>
#### 获取pod标签 <font color="maroon">kubectl get pod <font color="blue">--show-labels</font></font>


## 工作负载-资源 Workload Resources
### Deployments
#### 获取所有deployment <font color="maroon">kubectl get deployments</font>
#### 获取特定deployment <font color="maroon">kubectl get deployments <font color="blue">\<deploy-name\></font></font>
#### 扩容deployment <font color="maroon">kubectl scale deployment <font color="blue">\<deploy-name\> --replicas=\<number\></font></font>
#### 修改deployment <font color="maroon">kubectl edit deployment <font color="blue">\<deploy-name\></font></font>
#### 查看特定deployment <font color="maroon">kubectl get deployment <font color="blue">\<deploy-name\></font> -o wide</font>

```yaml
#Deplyment Example
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - name: http
          containerPort: 80
          protocol: TCP

```


## 网络策略 NetworkPolicy
#### 从yaml创建 <font color="maroon">kubectl apply -f network-policy.yaml</font>
#### 检查网络策略 <font color="maroon">kubectl describe networkpolicy -n <font color="blue">\<networkpolicyname\></font></font>


```yaml
#Network Policy Example
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
 name: allow-port-from-namespace
 namespace: my-app #被访问者的命名空间（echo 访问 my-app）
spec:
 podSelector: {} #这行必须要写
 policyTypes:
 - Ingress #策略影响入栈流量
 ingress:
 - from: #允许流量的来源
 - namespaceSelector:
 matchLabels:
 project: echo #访问者的命名空间的标签 label（echo 访问 my-app）
 #- podSelector: {} #注意，这个不写。如果 ingress 里也写了- podSelector: {}，则会导致 my-app 中的 pod 可以访问 my-app 中 pod 的 9000 了，这样不
满足题目要求不允许非来自 namespace echo 中的 Pods 的访问。
 ports:
 - protocol: TCP
 port: 9000 #被访问者公开的端口
```


## 服务 Service(SVC)
#### 暴露对应端口 <font color="maroon">kubectl expose deployment <font color="blue">\<deploy-name\></font> <font color="blue">--type</font>=NodePort <font color="blue">--port</font>=80 <font color="blue">--target-port</font>=80 <font color="blue">--name</font>=\<service-name\></font>
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

## Ingress
#### 查看ingressclass <font color="maroon">kubectl get ingressclass <font color="blue">\<namespace\></font></font>

备注：如果不指定namespace，则使用默认的命名空间。

#### 查看特定ingress <font color="maroon">kubectl get ingress -n  <font color="blue">\<namespace\> </font></font>
#### 从yaml创建 <font color="maroon">kubectl apply -f ingress.yaml</font>
#### 创建ingress <font color="maroon">kubectl create ingress <font color="blue">\<ingress-name\></font>  <font color="blue">--rule</font>=/hello=hello:5678 <font color="blue">--class</font>=nginx <font color="blue">--annotation</font>=nginx.ingress.kubernetes.io/rewrite-target=/ <font color="blue">-n \<namespace\></font> </font>

```yaml
#Ingress example
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: minimal-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx-example
  rules:
  - http:
      paths:
      - path: /testpath
        pathType: Prefix
        backend:
          service:
            name: test
            port:
              number: 80

```
