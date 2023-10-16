# deployments

- manage revision/versions
- replicaset -> manage pods(s); deployment -> manage replicaset(s)
- control rollout strategy

## examples

### old and new replicaset
```shell
kubectl apply -f kuard-deployment.yaml

# change image from green to blue in tag
# check rollout history
kubectl rollout history deployment kuard

# observe in OldReplicaSets and NewReplicaSet in output
kubectl describe deployments.apps kuard

```