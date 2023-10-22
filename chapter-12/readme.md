# jobs

- short-lived one-off tasks, compared to long-lived workloads
- a job creates pods that run until successful termination (exit with 0)
- primary attributes of a job
  - job completions
  - number of pods to run in parallel
  - one shot: completions 1, parallel 1
  - work queue: completions 1, parallel 2+
  - parallel fixed completions: completions 1+, parallel 1+
- job automatically pick a unique label and use it to identify pods. i.e., job owns pod

## examples

### one shot
```shell
kubectl apply -f job-oneshot.yaml

kubectl get jobs

kubectl describe jobs.batch oneshot

kubectl get pods
```

### parallelism
```shell
kubectl apply -f job-parallel.yaml

# observe completions count increase as pods are completed
kubectl get jobs --watch

# observe initially 5 pods can be seen
kubectl get pods --watch
```

### work queues
```shell
# create a work queue
kubectl apply -f rs-queue.yaml

# create a service used by consumer eventually
kubectl apply -f service-queue.yaml

# port forward to view MemQ UI 
kubectl port-forward replicasets/queue 8080:8080

# create a work queue
curl -X PUT http://localhost:8080/memq/server/queues/keygen

# load data to queue
for i in work-item-{0..50}; do curl -X POST http://localhost:8080/memq/server/queues/keygen/enqueue -d "$i"; done

# consume work items from work queue 'keygen'
kubectl apply -f job-consumers.yaml

kubectl get pods --watch
kubectl get jobs --watch

kubectl delete rs,svc,job -l chapter=jobs
```

NOTE: cronjobs example present in page 148