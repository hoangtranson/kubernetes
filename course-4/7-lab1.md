# Install Kasten K10 Tooling

Install Kasten K10
In this step, we will actually install K10 by running the following commands:

```
helm install k10 kasten/k10 --namespace=kasten-io --create-namespace
```

![image](https://github.com/hoangtranson/kubernetes/assets/35447677/9a23b619-6f2d-443d-8ee7-db1cab6474f4)

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

## Expose the K10 dashboard
While not recommended for production environments, let's set up access to the K10 dashboard by creating a NodePort. Let's first create the configuration file for this:

```
cat > k10-nodeport-svc.yaml << EOF
apiVersion: v1
kind: Service
metadata:
  name: gateway-nodeport
  namespace: kasten-io
spec:
  selector:
    service: gateway
  ports:
  - name: http
    port: 8000
    nodePort: 32000
  type: NodePort
EOF
```

Now, let's create the actual NodePort Service

```
kubectl apply -f k10-nodeport-svc.yaml
```
