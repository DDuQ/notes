# Definition
Open-source container orchestrator, which helps us to deal with:
- Health monitoring.
- Managing replicas.
- Deployment of apps with zero downtime.
- Scaling across different machines.
- Load balance between container instances.

# Basic concepts

## Node
Refers to a machine, physical or virtual.
## Cluster
Group of [[Kubernetes#Node|nodes]].
## Pod
Container, those are transient; if traffic is going to the Pod we can't rely on a IP Address. We'd need to front pods with another component, namely a [[Kubernetes#Service|service]].
## Service
This is essentially a load balancer, which has a fixed IP Address. 
In case another pod tries to communicate to service's pods it will do so through the service. Likewise for external traffic.

![[Kubernetes-2.jpeg|449x321]]
## Namespace
Logical grouping of things.

![[Kubernetes-1.jpeg|447x306]]
## Deployment
Manages [[Kubernetes#Pod|pods]], which means we don't directly create them, this happens through a deployment.

![[Kubernetes-3.jpeg|446x347]]
### Replica sets
#TODO

