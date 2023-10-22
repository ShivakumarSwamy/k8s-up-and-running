# daemonsets

- will create a pod on each node unless a node selector is used
- usual use case is to run log collector, metrics collector so on, on each node
- mainly used by platform teams


## examples

### create a daemonset
```shell
kubectl apply -f fluentd.yaml

# observe pod on each node, also note nodeAffinity of requiredDuringSchedulingIgnoredDuringExecution, using metadata.name field 
kubectl get pods -l app=fluentd -o wide
```

### daemonset with nodeSelector
```shell
kubectl get nodes

kubectl label nodes kind-worker foo=bar

kubectl apply -f fluentd-node-selector.yaml

kubectl rollout status daemonset fluentd

# observe pod only on node kind-worker
kubectl get pods -l app=fluentd -o wide

# observe events 
kubectl describe daemonsets.apps fluentd

kubectl label nodes kind-worker foo-

# observe pod is no more and is removed
kubectl get pods -l app=fluentd -o wide

kubectl delete -f fluentd-node-selector.yaml
```