# Install MySQL and Create a Demo Database

To experiment with backup and recovery of a cloud-native application, we will install MySQL and create a database in this step.

First, install MySQL using the following commands:

```
kubectl create namespace mysql
helm install mysql bitnami/mysql --namespace=mysql
```

![image](https://github.com/hoangtranson/kubernetes/assets/35447677/74f2ca0b-2d88-487f-a644-55c16e061df5)

To ensure that MySQL is running, check the pod status to make sure they are all in the Running and Ready 1/1 state:

```
watch -n 2 "kubectl -n mysql get pods"
```

Once all pods have a Running and Ready 1/1 status, hit CTRL + C to exit watch and then run the following commands to create a local database.

```
MYSQL_ROOT_PASSWORD=$(kubectl get secret --namespace mysql mysql -o jsonpath="{.data.mysql-root-password}" | base64 --decode; echo)

kubectl exec -it --namespace=mysql $(kubectl --namespace=mysql get pods -o jsonpath='{.items[0].metadata.name}') \
  -- env MYSQL_PWD=$MYSQL_ROOT_PASSWORD mysql -u root -e "CREATE DATABASE k10demo"
```

