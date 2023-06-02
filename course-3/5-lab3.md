# Install K10 and Configure Storage

Install Kasten K10
In this step, we will actually install K10 by running the following commands:

```
helm install k10 kasten/k10 --namespace=kasten-io --create-namespace
```

To ensure that Kasten K10 is running, check the pod status to make sure they are all in the Running and Ready 1/1, 2/2 state:

```
watch -n 2 "kubectl -n kasten-io get pods"
```

Once all pods have a Running and Ready 1/1, 2/2 status, hit CTRL + C to exit watch.

Configure the Local Storage System
Once K10 is running, use the following commands to configure the local storage system.

```
kubectl annotate volumesnapshotclass csi-hostpath-snapclass k10.kasten.io/is-snapshot-class=true
```
