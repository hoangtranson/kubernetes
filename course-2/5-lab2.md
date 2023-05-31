# Run a Stateful Application Using MySQL

## Running Spring PetClinic on Kubernetes
The Spring PetClinic is a sample application designed to show how the Spring stack can be used to build simple, but powerful database-oriented applications.

The official version of PetClinic demonstrates the use of Spring Boot with Spring MVC and Spring Data JPA.

You can find more information in the link here https://spring-petclinic.github.io/

We are going to deploy the stack on Kubernetes which comprise of the following services:

- Discovery Server:
- Config Server
- AngularJS frontend (API Gateway)
- Customers, Vets and Visits Services
-
The application uses MySQL as backend data store.

### Pre-Requisites
- Clone the repository that has the Kubernetes object resources files that compose our application:

```
git clone https://github.com/ahmedgabers/petclinic-kubernetes
cd petclinic-kubernetes
```

### Installation

Let's create an environment variable REPOSITORY_PREFIX that has the name of the repository which stores the Docker images for PetClinic services:

```
export REPOSITORY_PREFIX=ahmedgabercod
```

Create the spring-petclinic namespace to contain all the resources we deploy for PetClinic:

```
kubectl apply -f k8s/init-namespace/
```

The PetClinic microservices communicate together by networking provided by the Kubernetes services which are going to be used by our deployments. Run the following command to create the services in the spring-petclinic namespace:

```
kubectl apply -f k8s/init-services
```

![image](https://github.com/hoangtranson/kubernetes/assets/35447677/8fb8a9de-bd22-402b-96d3-6e3dce0bfeb2)

Our application services need a relational database in order to store customers, vets, and visits information. So we will use Helm to setup the MySQL backend for each service:

Add the bitnami repository and update Helm:

```
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
```

Install the database instance for the vets service:

```
helm install vets-db-mysql bitnami/mysql --namespace spring-petclinic --version 9.1.4 --set auth.database=service_instance_db
```

Install the database instance for the visits service:

![image](https://github.com/hoangtranson/kubernetes/assets/35447677/0c9e90b7-ceca-439a-ba86-888101b79c6e)

Install the database instance for the customers service:

```
helm install customers-db-mysql bitnami/mysql --namespace spring-petclinic  --version 9.1.4 --set auth.database=service_instance_db
```

Our deployment YAMLs have a placeholder called REPOSITORY_PREFIX so we'll be able to deploy the images from our repository by simply running a shell script:

```
./scripts/deployToKubernetes.sh
```

Verify the pods are deployed:

```
watch kubectl get pods -n spring-petclinic
```

![image](https://github.com/hoangtranson/kubernetes/assets/35447677/0f9059b3-1450-4c09-8cd4-b4589e95c3ac)

Once all Pods are in Running and Ready 1/1 state, visit the application from the tab in the left upper corner and explore the frontend:

- From the 'Owners' dropdown list, select 'Register' and enter the information to register a new customer
- Make sure the Telephone field contains no more than 10 numerical values without the '+' sign. e.g (7908645213)
- From the 'Owners' dropdown list, select 'All' to verify the customer was added

You can verify the data persisted in the MySQL database by running a temporary container with the mysql:8.0 image and running the mysql binary to execute the query:

```
export MYSQL_PASSWORD=$(kubectl get secret customers-db-mysql -n spring-petclinic -o jsonpath='{.data.mysql-root-password}' | base64 -d)

kubectl run mysql-client --image=mysql:8.0 --namespace spring-petclinic -i --rm --restart=Never --\
  mysql -h customers-db-mysql -uroot -p$MYSQL_PASSWORD<<EOF
USE service_instance_db;
SELECT * FROM owners;
EOF
```

Scaling deployments horizontally by adding multiple instances of the Application to tolerate surge in traffic is important, and we can easily achieve that in Kubernetes with a single command:

```
kubectl scale deployments -n spring-petclinic {api-gateway,customers-service,vets-service,visits-service} --replicas=3
```

Verify the deployments are scaled:

```
kubectl get pods -n spring-petclinic
```
