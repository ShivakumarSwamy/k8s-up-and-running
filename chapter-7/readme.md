# service discovery

- service object selects pods based on a selector labels
- track pods via readiness check
- type
  - clusterIP: load balance across all pods part of service
  - nodePort: in addition to cluster ip, evey node in the cluster expose port to reach the service
  - loadBalancer: external cloud provider public or internal
- endpoints k8s object contains pod ip address of the service 


```shell
kubectl apply -f alpaca-prod-deployment.yaml -f bandicoot-prod-deployment.yaml

# expose service
kubectl expose deployment alpaca-prod

kubectl expose deployment bandicoot-prod
```

- can query the service name in the pod
```text
alpaca-prod.default.svc.cluster.local.
 |            |     |     |------|
 |       namespace  |  base domain of cluster
service name     this is k8s service
```