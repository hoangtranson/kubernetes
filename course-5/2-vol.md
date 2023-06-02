# Kubernetes Volumes and Storage types
In Kubernetes, all containers are ephemeral and Kubernetes volume is an abstraction implemented to solve two problems.

- Loss of files when a container crashes.
- Sharing files between containers running together in a Pod.

The volumes fall under three major categories:

- Persistent Volumes
- Ephemeral Volumes
- Projected Volumes

## Persistent Volumes
A persistent volume is a resource in the cluster and has a lifecycle independent of any individual pod that uses persistent volume, therefore data stored in persistent volumes will remain when the Pod is terminated.

Examples of persistent volumes are:

- NFS
- iSCSI

A Persistent Volume claim is a request for storage by a user. It is similar to a Pod. Pods consume node resources and Persistent Volume Claims consume Persistent Volume resources.

## Ephemeral Volumes

Ephemeral volumes are designed for applications that need additional storage but don't care whether that data is stored persistently across restarts. For example, caching services are often limited by memory size and can move infrequently used data into storage that is slower than memory with little impact on overall performance.

Other applications expect some read-only input data to be present in files, like configuration data or secret keys.

Data stored in ephemeral volumes does not remain when the Pod is terminated.

Types of ephemeral storage:

- emptyDir
- configMap, downwardAPI, secret
- CSI ephemeral volumes
- generic ephemeral volumes


## Projected Volumes

A projected volume maps several existing volume sources into the same directory.

This allows:

- Automatic population of a single volume to create a single directory with the keys from multiple Secrets, ConfigMaps, serviceAccountTokens and with downward API information.
- Automatic population of a single volume to create a single directory by explicitly specifying the path for each item, providing full control over the directory content with the keys from multiple secrets, ConfigMaps and with downward API information.

## Volume Snapshots

Volume snapshots provide a standardised way to copy volumes of content at a particular point in time without creating a new volume. This allows administrators to perform various backup operations on Kubernetes.

A VolumeSnapshotContent is a snapshot taken from a volume in the cluster that has been provisioned by an administrator. It is a resource in the cluster just like a PersistentVolume is a cluster resource.

A VolumeSnapshot is a request for snapshot of a volume by a user or an application. It is similar to a PersistentVolumeClaim.

Similar to how API resources PersistentVolume and PersistentVolumeClaim are used to provision volumes for users and administrators, VolumeSnapshotContent and VolumeSnapshot API resources are provided to create volume snapshots for users and administrators.
