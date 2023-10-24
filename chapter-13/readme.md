# configmap and secrets

## configmap

- same image should be able to use for development, staging, and production
- configmap provides configuration information for workloads
- one or more data elements as a collection of key/value pairs
- ways to use a configmap
  - filesystem:
    - mount a configmap to a pod
    - file is created for each entry based on the key name
    - contents of that file are set to value
    - hidden files (prepended with ..) are used to do clean swap of new values when configmap is updated 
  - environment variable:
    - can be used to populate a environment variable dynamically
  - command line argument:
    - can be used to populate a command line argument dynamically
- key name should conform to environment variable names pattern


## secrets
- secrets provides sensitive information for workloads
- one or more data elements as a collection of key/value pairs
- stored on tmpfs volumes (aka RAM disks), and as such are not written to disk on nodes
- hidden files (prepended with ..) are used to do clean swap of new values when configmap is updated
- key name should conform to environment variable names pattern

## examples

## download crt and key
```shell
curl -o kuard.crt https://storage.googleapis.com/kuar-demo/kuard.crt

curl -o kuard.key https://storage.googleapis.com/kuar-demo/kuard.key
```

### imperative way of configmap
```shell
kubectl create configmap my-config --from-file=my-config.txt --from-literal=extra-param=extra-value --from-literal=another-param=another-value

# observe values under data key
kubectl get configmaps my-config -o yaml

# open in browser to view FileSystem
kubectl port-forward kuard-config 8080
```

### ways to use a configmap
```shell
kubectl apply -f kuard-config.yaml
```

### imperative way of secret
```shell
kubectl create secret generic kuard-tls --from-file=kuard.crt --from-file=kuard.key

kubectl apply -f kuard-secret.yaml

# observe values under data key, base64 encoded
kubectl get secrets kuard-tls -o yaml

# open in browser to view FileSystem
kubectl port-forward kuard-tls 8443:8443
```

### re-create and update
```shell
kubectl create secret generic kuard-tls --from-file=kuard.crt --from-file=kuard.key --dry-run=client -o yaml | kubectl replace -f -

kubectl create secret generic kuard-tls --from-literal=extra-param=extra-value  --from-file=kuard.crt --from-file=kuard.key --dry-run=client -o yaml | kubectl replace -f -

kubectl get secrets kuard-tls -o yaml
```

### live updates
```shell
kubectl create secret generic kuard-tls --from-file=kuard.crt --from-file=kuard.key --dry-run=client -o yaml | kubectl replace -f -

kubectl apply -f kuard-secret.yaml

# observe values under data key, base64 encoded
kubectl get secrets kuard-tls -o yaml

# open in browser to view FileSystem
kubectl port-forward kuard-tls 8443:8443

# observe after some time extra-param key as file in file browser
kubectl create secret generic kuard-tls --from-literal=extra-param=extra-value  --from-file=kuard.crt --from-file=kuard.key --dry-run=client -o yaml | kubectl replace -f -
```

NOTE:
- page 154, k8s secrets store csi driver info
- page 156, docker registry secret