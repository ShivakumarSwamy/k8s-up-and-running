# ingress

very less content is added as the examples require cloud provider which is out of the scope for me

- usually service k8s object is layer 4 and ingress is used for layer 7 load balancing
- virtual hosting is implemented by ingress
  - hosts many http sites on single ip
  - load balancer or reverse proxy accepts connections
  - connection is parsed to find Host header and URL path
  - based on above rule in load balancer or reverse proxy, traffic is directed to upstream service (pods)
- setup is usually a third party controller is present in cluster which monitors ingress object annotations and 
  reacts on it, there is no action item for k8s itself
- DNS can be created by using External DNS which can be deployed in k8s cluster
- Ingress has to read and merge all configs

## ingress examples in book
- contour
- ngnix path re-writing
