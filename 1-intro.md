The Kubernetes Control Plane is a collection of three processes that run on a single node in your cluster, which is designated as the Control Plane node. Those processes are: kube-apiserver, kube-controller-manager and kube-scheduler.

Each individual non-Control Plane node in your cluster runs two processes:

- kubelet, which communicates with the Kubernetes Control Plane.
- kube-proxy, a network proxy which reflects Kubernetes networking services on each node.

The various parts of the Kubernetes Control Plane, such as the kube-apiserver and kubelet processes, govern how Kubernetes communicates with your cluster. The Control Plane maintains a record of all of the Kubernetes Objects in the system, and runs continuous control loops to manage those objects’ state. At any given time, the Control Plane’s control loops will respond to changes in the cluster and work to make the actual state of all the objects in the system match the desired state that you provided.

![image](https://github.com/hoangtranson/kubernetes/assets/35447677/56bc236d-36a9-4b78-96c6-3aba92c7088d)

For example, when you use the Kubernetes API to create a Deployment object, you provide a new desired state for the system. The Kubernetes Control Plane records that object creation, and carries out your instructions by starting the required applications and scheduling them to cluster nodes–thus making the cluster’s actual state match the desired state.

## Kubernetes Control Plane
The Kubernetes Control Plane is responsible for maintaining the desired state for your cluster. When you interact with Kubernetes, such as by using the kubectl command-line interface, you’re communicating with your cluster’s Kubernetes Control Plane.

The “Control Plane” refers to a collection of processes managing the cluster state. Typically these processes are all run on a single node in the cluster, and this node is also referred to as the Control Plane. The Control Plane can also be replicated for availability and redundancy.

## Kubernetes Nodes
The nodes in a cluster are the machines (VMs, physical servers, etc) that run your applications and cloud workflows. The Kubernetes Control Plane controls each node; you’ll rarely interact with nodes directly.

![image](https://github.com/hoangtranson/kubernetes/assets/35447677/928ac4c9-34fd-4ba1-b1cd-2a519f090d7f)
.
