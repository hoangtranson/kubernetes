# Volume Snapshots and Snapshot Classes

Volume Snapshots

In this exercise we are going to create a volume snapshot.

When you create the volume snapshot, you have to register Volume Snapshot Classes to your cluster. The default VolumeSnapshotClass called csi-hostpath-snapclass. You can confirm by running the following command:

```
kubectl get volumesnapshotclasses
```

Next, create a persistent volume claim to create a persistent volume dynamically:

```
cat <<'EOF'> csi-pvc.yaml | kubectl apply -f csi-pvc.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: csi-pvc
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
  storageClassName: csi-hostpath-sc
EOF
```

You can confirm that persistent volume is created by the following command:

```
kubectl get pv
```

## Take a volume snapshot

You can take a volume snapshot for the persistent volume claim:

```
cat <<'EOF'> snapshot-demo.yaml | kubectl apply -f snapshot-demo.yaml
apiVersion: snapshot.storage.k8s.io/v1
kind: VolumeSnapshot
metadata:
  name: snapshot-demo
spec:
  volumeSnapshotClassName: csi-hostpath-snapclass
  source:
    persistentVolumeClaimName: csi-pvc
EOF
```

The key terms in the above snippet are listed below:

persistentVolumeClaimName is the name of the PersistentVolumeClaim data source for the snapshot.A volume snapshot can request a particular class by specifying the name of a VolumeSnapshotClass using the attribute volumeSnapshotClassName. If nothing is set, then the default class is used.

You can confirm your volume snapshot by the following command:

```
kubectl get volumesnapshot
```

## Restore from a volume snapshot

In order to perform a restore operation, we are going to source the snapshot we created in a new persistent volume claim resource:

```
cat <<'EOF'> csi-pvc-restore.yaml | kubectl apply -f csi-pvc-restore.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: csi-pvc-restore
spec:
  storageClassName: csi-hostpath-sc
  dataSource:
    name: snapshot-demo
    kind: VolumeSnapshot
    apiGroup: snapshot.storage.k8s.io
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
EOF
```

The key terms in the above snippet are listed below:

- dataSource: It specifies the snapshot resources to restore the data from.

You can confirm that a persistent volume claim was created from the VolumeSnapshot:

```
kubectl get pvc
```

![image](https://github.com/hoangtranson/kubernetes/assets/35447677/906ceed4-852d-47f8-83aa-646e3a61b3e8)
