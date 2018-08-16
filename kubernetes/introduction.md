# kubernetes

* Container
* Pod
* Node
* Deployment
* ReplicaSet

## pod

smallest unit that kubernetes manages. fundamental unit upon which the rest is
built.

a pod is made of **one or more containers** and **its metadata**.

a pod is not intended to be treated as a durable, long-lived entity.

### containers vs pods

While a container is the smallest building block of what goes into Kubernetes, the smallest
unit that Kubernetes works with is a Pod.

### pod guarantees

* all containers in a pod are run in the same node
* any container running within a pod will share the node's network with other
containers in the same pod
* containers within a pod can share files through volumes, attached to containers
* a pod has an explicit life cycle, and will always remin on the node in which
it was started

### container states

Running|Terminated

### pod life cycle

phase one: PodStatus = Pending|Running|Succeeded|Failed

source: https:/​/kubernetes.​io/​docs/​concepts/​workloads/​pods/​pod-​lifecycle/​#pod-​phase

## private network

It is important to know that everything within Kubernetes is run on a private, isolated
network that is not normally accessible from outside the cluster.

## namespaces

pods are collected into namespaces (e.g. to provide quotas, limit resource usage, etc.)


