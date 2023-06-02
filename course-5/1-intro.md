# Introduction
The role of storage in application architecture has changed with Kubernetes.

When you deploy a pod to the cluster, this pod may need to store data in files. Think of how a web server logs requests and errors to log files (for example, Apache uses the access.log and error.log files for this purpose).

However, like pods, this storage is temporary. When the kubelet restarts the pod, or if you intentionally deleted and recreated the pod, any files created during the pod's lifetime are automatically lost.

Kubernetes provides various options to manage storage for applications.

Kubernetes storage
Kubernetes storage is based on the Kubernetes volumes and a volume can be thought of as a directory that is accessible to the container in a Pod. A Pod can use any number of volume types simultaneously.

Kubernetes supports several types of volumes:

- emptyDir
- hostPath
- Local
- nfs
- fc (fibre channel)
- gcePersistentDisk
- awsElasticBlockStore
- azureDisk
- azureFile
- Cephfs


## Container Storage Interface (CSI)
CSI stands for Container Storage Interface (https://kubernetes-csi.github.io/docs/) which is a standard for exposing arbitrary block and file storage systems to the containerized workload on container orchestration systems like Kubernetes.

CSI providers can write plugins and those can be used in Kubernetes to extend the storage capabilities.
