# kind multi-node
```text
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
```

```shell
kind create cluster --config multi-node-kind.yaml
```

# k8s commands

## k8s version

```shell
kubectl version
```

```text
output
Client Version: version.Info{Major:"1", Minor:"27", GitVersion:"v1.27.4", GitCommit:"fa3d7990104d7c1f16943a67f11b154b71f6a132", GitTreeState:"clean", BuildDate:"2023-07-19T12:14:48Z", GoVersion:"go1.20.6", Compiler:"gc", Platform:"darwin/arm64"}
Kustomize Version: v5.0.1

# k8s api server version 
Server Version: version.Info{Major:"1", Minor:"27", GitVersion:"v1.27.3", GitCommit:"25b4e43193bcda6c7328a6d147b1fb73a33f1598", GitTreeState:"clean", BuildDate:"2023-06-15T00:38:14Z", GoVersion:"go1.20.5", Compiler:"gc", Platform:"linux/arm64"}
```

## k8s component status

```shell
kubectl get componentstatuses
```

```text
output

NAME                 STATUS    MESSAGE                         ERROR
controller-manager   Healthy   ok                              // handler controller behaviour (ex: replica)
scheduler            Healthy   ok                              // pods ont0 nodes
etcd-0               Healthy   {"health":"true","reason":""}   // storage
```

## cluster components

### k8s proxy
- runs on every node
- route network traffic to load-balanced services in k8s cluster
- usually run as daemonsets
```shell
kubectl get daemonsets.apps --namespace=kube-system kube-proxy
```

### k8s dns
- dns server which provides naming and discovery for the services
- usually run as deployment and has a service

```shell
kubectl get deployments.apps --namespace=kube-system coredns

kubectl get services --namespace=kube-system kube-dns
```