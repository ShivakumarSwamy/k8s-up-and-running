# k8s commands and concepts

Note: default config in $HOME/.kube/config or ~/.kube/config

```text
namespace(s) // folder
  |
  |___ set(objects)
```

- each k8s object is a RESTful resource 

## k8s apply manifest
```shell
kubetcl apply -f <file>.yaml
```

## k8s view object
```shell
kubectl get <resource-name> <object-name>
```

## k8s delete manifest
```shell
kubetcl delete -f <file>.yaml
```

## k8s add label
```shell
kubetcl label pods <pod-name> <key>=<value>
```

## k8s remove label
```shell
kubetcl label pods <pod-name> <key>-
```

## k8s logs of pod
```shell
kubectl logs <pod-name>

kubectl logs <pod-name> -c <container-name>
```

## k8s interactive tty
```shell
kubectl exec -it <pod-name> -- bash
```

## k8s port forward
```shell
kubectl port-forward <pod-name> <local-port>:<container-port>
```


# k8s context (minimal)
```text
            context
      ______________________
     |         |           |
   user      cluster    default namespace
```

## explain resource
```shell
kubectl explain pods
```

## cluster
- cordon a node: prevent future pods to schedule on the node
- drain a node: evict all pods on the node
- uncordon a node: allow pods to be scheduled on the node