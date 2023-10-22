# deployments

- manage revision/versions
- replicaset -> manage pods(s); deployment -> manage replicaset(s)
- deployments do not own the replicasets they create
- deployments uses label queries to identify replicasets and make api call to satisfy desired state to create pods

- control rollout strategy
- can configure how many deployment revisions it can save (default 10)

## deployment strategy

- recreate: 
  - terminates all old pods, adds new pods and will lead to downtime
- rolling update:
  - rollout deployment without downtime using `maxUnavailable` and `maxSurge` parameters
  - `maxUnavailable` parameter sets the number of pods that can be unavailable during a rolling update
    - example: old A 4, 25% maxUnavailable, 0% maxSurge, new B 4
      - 3 old A, 1 new B = 4 
      - 2 old A, 2 new B = 4
      - 1 old A, 3 new B = 4 
      - 0 old A, 4 new B = 4
  - maxUnavailable 100% == Recreate strategy
  - `maxSurge` parameter controls how many extra resources can be created to achieve a rollout 
    - example: old A 4, 25% maxSurge, 0% maxUnavailable, new B 4
      - add 1 (25% of 4) to new B 4 = 5, 4 old A, 1 new B = 5
      - 3 old A, 1 new B = 4
      - add 1 (25% of 4) to new B 4 = 5, 3 old A, 2 new B = 5
      - 2 old A, 2 new B = 4
      - add 1 (25% of 4) to new B 4 = 5, 2 old A, 3 new B = 5
      - 1 old A, 3 new B = 4
      - add 1 (25% of 4) to new B 4 = 5, 1 old A, 4 new B = 5
      - 0 old A, 4 new B = 4 
    - maxSurge 100% == blue/green deployment
  - Deployment waits until a pod is ready, to move on to create new pod by the following,
    - pod readiness check
    - `minReadySeconds` Minimum number of seconds for which pod is available, default is `0`
    - `progressDeadlineSeconds` Maximum time in seconds for which pod is considered to be failed, default is `600`
    - _progressDeadlineSeconds does not represent total length of deployment.,_ 

## examples

## satisfy desired state
```shell
# creates 2 pods
kubectl create -f kuard-deployment.yaml

# selector labels of replicaset
kubectl get deployments.apps kuard --output jsonpath --template {.spec.selector.matchLabels}

kubectl get replicasets.apps --selector=run=kuard

kubectl scale deployment kuard --replicas=3

kubectl get replicasets.apps --selector=run=kuard

# scale to 1 and observe, replicaset count goes back to 3 due to reconciliation
kubectl scale replicaset kuard-645b4bff86 --replicas=1

kubectl get replicasets.apps --selector=run=kuard

# manage replicaset directly
kubectl delete deployments.apps kuard --cascade=orphan
```

### old and new replicaset
```shell
kubectl apply -f kuard-deployment.yaml

# change image from green to blue in tag
# check rollout history
kubectl rollout history deployment kuard

# observe in OldReplicaSets and NewReplicaSet in output
kubectl describe deployments.apps kuard

```

### change in template leads to new deployment
```shell
kubectl apply -f kuard-deployment.yaml

kubectl apply -f kuard-deployment-annotation.yaml

# observe new revision
kubectl rollout history deployment kuard
```

### pause or resume an deployment
```shell
kubectl apply -f kuard-deployment.yaml

kubectl rollout pause deployment kuard

kubectl apply -f kuard-deployment-update-image.yaml

# observe no new pods are scheduled
kubectl get pods --watch

# after resume new pods are present
kubectl rollout resume deployment kuard
```

### rollout undo to revision
```shell
kubectl apply -f kuard-deployment.yaml

kubectl apply -f kuard-deployment-annotation.yaml

kubectl apply -f kuard-deployment-update-image.yaml

kubectl rollout history deployment kuard

# rollback to previous version
kubectl rollout undo deployment kuard --to-revision=0
OR 
kubectl rollout undo deployment kuard

# rollback to specific version
kubectl rollout undo deployment kuard --to-revision=3

# note rolling back to version leads to creation of new revision number and removal of the chosen revision number 
```

### recreate strategy
```shell
kubectl apply -f kuard-deployment-recreate-strategy-1.yaml

kubectl get pods --watch

kubectl scale deployment kuard --replicas=5

# observer 5 pods instantaneously terminated, which leads to downtime 
kubectl apply -f kuard-deployment-recreate-strategy-2.yaml
```

## topics will help for on-call
- page 120: pause and resume deployment
- page 121: undo deployment