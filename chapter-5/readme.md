# pods

- smallest deployable artifact in k8s cluster

```text
pod
|
|__ container(s)
|__ volume(s)
```

- each container has own cgroups
- share number of linux namespaces
- share same ip, port space, hostname, can communicate using native interprocess communication
