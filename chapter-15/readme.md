# service meshes

- general capabilities provided by most service mesh implementations
  - network encryption and authorization
  - traffic shaping
  - observability
- we can extract out some functionalities of service and move it service mesh, so service need not handle it    

## network encryption and authorization

- encryption of network traffic between pods
  - one of the way is using mTLS (mutual transport layer security), example peer authentication in istio
  - adds a side care to the pod (envoy as example) which intercepts all network communication

## traffic shaping

- A/B experimenting
- canary deployment and shift traffic (virtual service of istio)


## observability

- single aggregate request that defines the user experience 
- kiali, appdynamics as example


NOTE:
- book specifically mentions to use service mesh if and only necessary