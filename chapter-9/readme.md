# replica sets

## design concept

- reconciliation loop
  - desired state vs current state
  - when we apply via k8 we give k8s a desired state
  - due to reconciliation os k8s, the current state is matched to desired state
- notion of 'coming in the front door'
  - ReplicaSet do not own the Pods they create
  - ReplicaSet uses label queries to identify pods and make api call to satisfy desired state to create pods

## topics will help for on-call
- page 105; adopting existing container
- page 105; quarantining containers
- page 109; imperative scaling with kubectl scale


## examples

### quarantining containers
```shell
kubectl apply --filename kuard-replicaset.yaml

kubectl get pods --watch

# remove label, observer 2nd pod is created
kubectl label pod kuard-89rqp version-

# find replicaset owner from pod 
kubectl get pod kuard-vmwgr -o=jsonpath='{.metadata.ownerReferences[0].name}'
```

### autoscale imperative
```shell
# metrics server install for kind cluster
kubectl apply --kustomize metrics-server/

kubectl autoscale replicaset kuard --min=2 --max=5 --cpu-percent=80
```

### cascade
```shell
# delete only replicaset and not pods tracked by it via selector
kubectl delete replicasets.apps kuard --cascade=orphan
```

Note:
- No direct link between HPA and ReplicaSet, which leads to clash between replica count when both get modified