# Kasten K10 Backup and Disaster Recovery

The below section provides a complete end-to-end example of how to extend your application to support generic backup and restore using Kasten K10.

Deploy the application

Open the Terminal tab and run the below command to create a new namespace to house our application

```
kubectl create namespace kasten-demo
```

Next, copy the application specification from below and deploy the demo-app application in the kasten-demo namespace.

```
cat <<'EOF'> deployment.yaml | kubectl apply -f deployment.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  namespace: kasten-demo
  name: demo-pvc
  labels:
    app: demo
    pvc: demo
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
---
apiVersion: apps/v1
kind: Deployment
metadata:
  namespace: kasten-demo
  name: demo-app
  labels:
    app: demo
spec:
  replicas: 1
  selector:
    matchLabels:
      app: demo
  template:
    metadata:
      labels:
        app: demo
    spec:
      containers:
      - name: demo-container
        image: alpine:3.7
        resources:
            requests:
              memory: 256Mi
              cpu: 100m
        command: ["tail"]
        args: ["-f", "/dev/null"]
        volumeMounts:
        - name: data
          mountPath: /data
      volumes:
      - name: data
        persistentVolumeClaim:
          claimName: demo-pvc
EOF
```

Let's check the status of the deployed application:

```
POD_NAME=$(kubectl --namespace kasten-demo get pods -l app=demo --no-headers | awk '{print $1}')
kubectl --namespace kasten-demo get pods "$POD_NAME"
```

## Insert Data
The easiest way to insert data into the demo application is to simply copy it in:

```
kubectl cp /etc/passwd kasten-demo/${POD_NAME}:/data/ -c demo-container
```

Verify if the data was copied successfully:

```
kubectl --namespace kasten-demo exec ${POD_NAME} -c demo-container -- ls -l /data
```

## Backup Data
We are going to backup the application by creating a custom backup policy.

```
cat <<'EOF'> custom-policy.yaml | kubectl apply -f custom-policy.yaml
kind: Policy
apiVersion: config.kio.kasten.io/v1alpha1
metadata:
  name: sample-custom-backup-policy
  namespace: kasten-io
spec:
  frequency: "@hourly"
  retention:
    hourly: 24
    daily: 7
    weekly: 4
    monthly: 12
    yearly: 7
  selector:
    matchExpressions:
      - key: k10.kasten.io/appNamespace
        operator: In
        values:
          - kasten-demo
  actions:
    - action: backup
EOF
```

Once applied, open the Kasten K10 tab and from the dashboard window, accept the license agreement and select Policies.

Next, select the policy with the name sample-custom-backup-policy then click on run once to run the policy now. This will immediately execute the actions in the policy sample-custom-backup-policy.

Navigate back to the dashboard to see action in progress.

Wait until the action status update: All actions completed successfully.

## Destroy Data
To destroy the data manually, navigate back to the Terminal window and run the following command:

```
kubectl -n kasten-demo exec ${POD_NAME} -c demo-container -- rm -rf /data/passwd
```

Verify

```
kubectl -n kasten-demo exec ${POD_NAME} -c demo-container -- cat /data/passwd
```

The command will return No such file or directory

![image](https://github.com/hoangtranson/kubernetes/assets/35447677/ed10fbec-383c-4268-8f0d-09f1477f64dd)

## Restore Data
Open the Kasten K10 tab, and from the dashboard window, select Applications.

On the kasten-demo application card, click restore and select the appropriate restore point and click restore again.

Navigate back to the dashboard to view the Restore action in progress and wait until it completes.

## Verify Data
After restore, you should verify that the data is intact. One way to verify this is to use diff tool.

First, copy the restored data back to local env

```
POD_NAME=$(kubectl --namespace kasten-demo get pods -l app=demo --no-headers | awk '{print $1}')
kubectl cp kasten-demo/${POD_NAME}:data/passwd /tmp/passwd -c demo-container
```

Next, run diff on both files:

```

The diff should show no difference.

```
