# role-based access control for k8s

- RBAC: role-based access control (access control based on role)
- provides a mechanism for restricting both access to and actions on k8s API to ensure that only authorized users have access


## identity in k8s

- every request in k8s is associated with identity
- no identity request is associated with `system:unauthenticated` group
- service account are managed by k8s itself and are associated with components inside k8s cluster itself
- user accounts are all other accounts associated with actual users of the cluster

## roles and role bindings

- once identity is successful, authorization of the user is determined
- role represents its capabilities, which is attached to identity via roleBinding to allow identity to perform those capabilities
- Role and RoleBinding - scoped to namespace
- ClusterRole and ClusterRoleBinding - scoped to cluster

## built-in roles

- `cluster-admin` role provides complete access to the entire cluster
- `admin` role provides complete access to a complete namespace
- `edit` role allows an end user to modify resources in a namespace
- `view` role allows for read-only access to a namespace 

## aggregationRule
- aggregate role/clusterRole based on labels

## examples

### role and role binding 
```shell
kubectl apply -f example-role-and-role-binding.yaml

kubectl get roles

kubectl get rolebindings
```

### testing authorization using can-i
```shell
kubectl auth can-i create pods
```

### aggregationRule
```shell

kubectl get clusterrole edit -o yaml

# observer in output, matchLabels which aggregates all roles with that label
aggregationRule:
  clusterRoleSelectors:
  - matchLabels:
      rbac.authorization.k8s.io/aggregate-to-edit: "true"
   ...

kubectl get clusterrole --selector=rbac.authorization.k8s.io/aggregate-to-edit=true   
```

NOTE:
- page 167, `rbac.authorization.kubernetes.io/autoupdate: "true"` add this annotation on the resource to not modify builtin roles if modified explicitly in cluster 
- page 167, `--anonymous-auth=false` is set on API server, to not allow API server's API discovery endpoint
- page 170, many group systems can enable "just in time" (JIT) access, temporarily during the incident 