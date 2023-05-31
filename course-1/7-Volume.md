On-disk files in a Container are ephemeral, which presents some problems for non-trivial applications when running in Containers. First, when a Container crashes, kubelet will restart it, but the files will be lost - the Container starts with a clean state. Second, when running Containers together in a Pod it is often necessary to share files between those Containers. The Kubernetes Volume abstraction solves both of these problems.

At its core, a Volume is just a directory, possibly with some data in it, which is accessible to the Containers in a Pod. How that directory comes to be, the medium that backs it, and the contents of it are determined by the particular volume type used.

A Kubernetes Volume has an explicit lifetime - the same as the Pod that encloses it. Consequently, a volume outlives any Containers that run within the Pod, and data is preserved across Container restarts. When a Pod ceases to exist, the volume will cease to exist too. Kubernetes supports many types of Volumes, and a Pod can use any number of them simultaneously.

Some examples of Volumes are:

- emptyDir - just an empty directory
- azureDisk - a disk on Microsoft Azure
- hostPath - a directory that lives on the node itself
- nfs - an exported NFS share
- 
but there are many more. Take a look here (https://kubernetes.io/docs/concepts/storage/volumes/#types-of-volumes)
