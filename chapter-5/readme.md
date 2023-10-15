# pods

- smallest deployable artifact in k8s cluster or atomic unit
- kubelet which exists in each node, maintains lifecycle of pod 
- 
```text
pod
|
|__ container(s)
|__ volume(s)
```

- each container has own cgroups
- share number of linux namespaces
- share same ip, port space, hostname, can communicate using native interprocess communication

# manifest

- text file representation of the desired state of k8s object
- declarative configuration

## imperative way
```shell
kubectl run kuard --image=gcr.io/kuar-demo/kuard-amd64:blue

kubectl get pods --watch

kubectl describe pod kuard

kubectl delete pod kuard
```

## declarative way

- refer for example [here](./kuard-pod-declarative.yaml) which creates a pod object

```shell
kubectl apply --filename kuard-pod-declarative.yaml
OR
kubectl apply -f kuard-pod-declarative.yaml

kubectl delete -f kuard-pod-declarative.yaml
```

- when a pod is deleted, it goes into terminating state
- default termination grace period is 30 seconds


# debugging
```shell
# concise information
kubectl get pods kuard

# more information
kubectl get pods kuard -o wide

# complete manifest
kubectl get pods kuard -o yaml
OR
kubectl get pods kuard -o json

# verbose level 10
kubectl get pods kuard --v=10

# logs of running single container in a pod
kubectl logs kuard

# stream logs of running single container in a pod
kubectl logs kuard --follow
OR
kubectl logs kuard -f 
```

Note:
- add -o wide to get more data on output of k8s command
- set verbose level using --v=<value> flag for debugging purpose
- add -o yaml or json to print complete manifest
- any data stored in pod is ephemeral, if persistence needed, use Persistent Volumes

## execute command
```shell
kubectl exec kuard -- date

# interactive shell
kubectl exec -it kuard -- ash
```

## health checks

- probes: periodic check on health of container
  - liveness: probe to check container application is healthy
  - readiness: probe to check container is ready to serve requests
  - startup: runs before any above probe(s) is started
- health checks:
  - http
  - tcpSocket
  - exec a command

# resource management

- utilization: amount of resource actively used / amount of resource has been purchased
- requests: minimum amount of resource required to run application
- limits: maximum amount of resource application can consume
- 1 MB = 1024 KB, 1MiB = 1000KiB, 100m ("millicores")
- [example](./kuard-pod-declarative-req-limits-resources.yaml)

Note:
- prefer to always requests and limits to control resource usage
- memory cannot be removed as it's allocated when already provisioned
- prefer request and limit match for java services and correlates with JVM xms and xmx parameters

**NOTE: Volumes are covered briefly in this chapter, page 62**