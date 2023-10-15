# label and annotations

- label is used to identify k8s objects, maybe even groups objects like in service object
- annotation add metadata to k8s objects and usually third party use this
- label are created as key ((prefix)/name) / value pairs
  - key length up to 253
  - value length to 63 
  - prefix has to be dns subdomain
  - both key and value are string
- annotations are same as label, however value is free form

## applying labels
```shell
# alpaca-prod 
# create a deployment with replica 0 
kubectl create deployment alpaca-prod --image=gcr.io/kuar-demo/kuard-amd64:blue --replicas=0

# add label to deployment
kubectl label deployments.apps alpaca-prod ver=1 env=prod

# patch template metadata label used by replica sets and pods
kubectl patch deployments.apps alpaca-prod -p '{"spec":{"template":{"metadata":{"labels":{"ver":"1", "env": "prod"}}}}}'

# scale to 2 pods
kubectl scale deployment alpaca-prod --replicas=2

# alpaca-test
# create a deployment with replica 0 
kubectl create deployment alpaca-test --image=gcr.io/kuar-demo/kuard-amd64:blue --replicas=0

# add label to deployment
kubectl label deployments.apps alpaca-test ver=2 env=test

# patch template metadata label used by replica sets and pods
kubectl patch deployments.apps alpaca-test -p '{"spec":{"template":{"metadata":{"labels":{"ver":"2", "env": "test"}}}}}'

# scale to 1 pod
kubectl scale deployment alpaca-test --replicas=1

# bandicoot-prod
# create a deployment with replica 0 
kubectl create deployment bandicoot-prod --image=gcr.io/kuar-demo/kuard-amd64:green --replicas=0

# add label to deployment
kubectl label deployments.apps bandicoot-prod ver=2 env=prod

# patch template metadata label used by replica sets and pods
kubectl patch deployments.apps bandicoot-prod -p '{"spec":{"template":{"metadata":{"labels":{"ver":"2", "env": "prod"}}}}}'

# scale to 2 pods
kubectl scale deployment bandicoot-prod --replicas=2

# bandicoot-staging
# create a deployment with replica 0 
kubectl create deployment bandicoot-staging --image=gcr.io/kuar-demo/kuard-amd64:green --replicas=0

# add label to deployment
kubectl label deployments.apps bandicoot-staging ver=2 env=staging

# patch template metadata label used by replica sets and pods
kubectl patch deployments.apps bandicoot-staging -p '{"spec":{"template":{"metadata":{"labels":{"ver":"2", "env": "staging"}}}}}'

# scale to 1 pod
kubectl scale deployment bandicoot-staging --replicas=1
```

## show and filter label
```shell
kubectl get pods --show-labels

# label as column
kubectl get pods -L env

# selector
kubectl get pods --selector="ver=2"

kubectl get pods --selector="env in (test, prod, staging)"
```