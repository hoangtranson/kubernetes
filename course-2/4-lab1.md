## Storage Classes
A StorageClass provides a way for administrators to describe the "classes" of storage they offer. Different classes might map to quality-of-service levels, or to backup policies, or to arbitrary policies determined by the cluster administrators. Kubernetes itself is unopinionated about what classes represent. This concept is sometimes called "profiles" in other storage systems.

Check the current storage classes created in the cluster:

```
kubectl get storageclasses
```

## Persistent Volumes
A PersistentVolume can be mounted on a host in any way supported by the resource provider.

Providers will have different capabilities and each PV's access modes are set to the specific modes supported by that particular volume.

For example, NFS can support multiple read/write clients, but a specific NFS PV might be exported on the server as read-only.

Each PV gets its own set of access modes describing that specific PV's capabilities.

The access modes are:

- ReadWriteOnce -- the volume can be mounted as read-write by a single node
- ReadOnlyMany -- the volume can be mounted read-only by many nodes
- ReadWriteMany -- the volume can be mounted as read-write by many nodes

We are going to create a PersistentVolume of 10Gi, called 'myvolume'.

Make it have accessMode of 'ReadWriteOnce' and 'ReadWriteMany', storageClassName 'local-path', mounted on hostPath '/etc/foo'.

A hostPath PersistentVolume uses a file or directory on the Node to emulate network-attached storage.

Let's create the directory that we are going to use in this example:

```
mkdir /etc/foo
```

Next, we create the PV:

```
cat<<'EOF' > pv.yaml
kind: PersistentVolume
apiVersion: v1
metadata:
  name: myvolume
spec:
  storageClassName: local-path
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
    - ReadWriteMany
  hostPath:
    path: /etc/foo
EOF
```

To create resources in Kubernetes from YAML specification files we use the kubectl apply command:

```
kubectl apply -f pv.yaml
```

Show the PersistentVolumes that exist on the cluster:

```
kubectl get pv
```

## Persistent Volume Claims
Pods cannot access Persistent Volumes directly, we need to claim storage capacity for our applications by binding the request for capacity to PVs using PersistentVolumeClaims (https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistentvolumeclaims).

We are going to create a PersistentVolumeClaim, called 'mypvc', with a request of 4Gi and an accessMode of ReadWriteOnce.

```
cat<<'EOF' > pvc.yaml
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: mypvc
spec:
  storageClassName: local-path
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 4Gi
EOF
```

Let's create the PVC:

```
kubectl apply -f pvc.yaml
```

Show the PersistentVolumeClaims of the cluster:

```
kubectl get pvc
```

Notice the claim will remain in Pending status until it's consumed by an application. This is due to volumeBindingMode set to WaitForFirstConsumer in the storage class 'local-path' used by the PVC.

Next, let's create a Pod that consumes the claim we created.

```
cat<<'EOF'> busybox-one.yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: busybox-one
  name: busybox-one
spec:

  volumes:
  - name:  my-vol # has to match volumeMounts.name
    persistentVolumeClaim:
      claimName: mypvc

  containers:
  - args:
    - /bin/sh
    - -c
    - sleep 3600
    image: busybox
    name: busybox-one

    volumeMounts:
    - name: my-vol # has to match volumes.name
      mountPath: /etc/foo
EOF
```

Let's create the Pod:

```
kubectl apply -f busybox-one.yaml
```

Next, connect to the pod and copy '/etc/passwd' to '/etc/foo/passwd':

```
kubectl exec busybox-one -it -- cp /etc/passwd /etc/foo/passwd
```

Since '/etc/foo' is mounted on the volume we created earlier, it's lifecycle is independant from the Pod's lifecycle. We can attach the volume to another Pod and we should be able to access and read the file we copied in the previous step.

Let's create a second Pod that mounts to the same PV:

```
cat<<'EOF'> busybox-two.yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: busybox-two
  name: busybox-two
spec:

  volumes:
  - name:  my-vol
    persistentVolumeClaim:
      claimName: mypvc

  containers:
  - args:
    - /bin/sh
    - -c
    - sleep 3600
    image: busybox
    name: busybox-two

    volumeMounts:
    - name: my-vol
      mountPath: /etc/foo
EOF
```

Create the Pod

```
kubectl apply -f busybox-two.yaml
```

Let's connect to it and verify that '/etc/foo' contains the 'passwd' file:

```
kubectl exec busybox-two -- ls /etc/foo
```

You should be able to see the passwd file.

