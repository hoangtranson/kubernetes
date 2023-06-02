# View K10 Dashboard
Expose the K10 dashboard
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

![image](https://github.com/hoangtranson/kubernetes/assets/35447677/b3c4802d-4fc1-490e-9b35-6568bafffdd9)

View the K10 Dashboard
Once completed, you should be able to view the K10 dashboard in the other tab on the left. We recommend taking the K10 dashboard tour at this point.
