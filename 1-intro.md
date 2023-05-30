The Kubernetes Control Plane is a collection of three processes that run on a single node in your cluster, which is designated as the Control Plane node. Those processes are: kube-apiserver, kube-controller-manager and kube-scheduler.

Each individual non-Control Plane node in your cluster runs two processes:

- kubelet, which communicates with the Kubernetes Control Plane.
- kube-proxy, a network proxy which reflects Kubernetes networking services on each node.
