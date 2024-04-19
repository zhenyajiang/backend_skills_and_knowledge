# K8S的命令很多，这里挑重点讲
## 集群
#### 查看集群 <font color="maroon">kubectl config get-contexts</font>
##### 切换集群 <font color="maroon">kubectl config use-context</font>
```go
kubectl config use-context k8s //切换到名称为：k8s的集群
```

## RBAC
kubectl create clusterrole \<rolename\> --verb=create --resource=deployments,statefulsets,daemonsets

kubectl -n \<namespace\> create serviceaccount \<accountname\>

kubectl -n \<namespace\> create rolebinding \<bindingname\> 
--clusterrole=\<rolename\> --serviceaccount=\<namespace\>:\<accountname\>