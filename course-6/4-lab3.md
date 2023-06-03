# Installing an Application
At a minimum, installing a chart in Helm requires two pieces of information:

- The name of the installation
- The chart you want to install

For example, use the command helm install RELEASE_NAME CHART to install a single instance of the chart.

## Configuration at Installation Time

While the default configuration is good sometimes, more often we want to pass our own configuration to the chart.

Many charts will allow you to provide configuration values. If we take a look at the Artifact Hub page for Wordpress, we would see a long list of configurable parameters. For example, we can configure the username of the Wordpress admin account by setting the wordpressUsername value.

You can use the --set flag which takes one or more values directly to add individual parameters to an install or upgrade.

For example helm install mysite bitnami/drupal --set wordpressUsername=admin

You can use the --values flag which takes a file directly to add individual parameters to an install or upgrade.

## Install Kubernetes Applications

Deploy Wordpress
We're going to demonstrate deploying a Kubernetes application using the helm install command.

Create a namespace for the installation target:

```
kubectl create namespace wordpress
```

Use the install command to deploy the chart to your cluster:

```
helm install my-wordpress bitnami/wordpress --set wordpressUsername=admin --set service.type=NodePort --set wordpressPassword=password --set mariadb.auth.rootPassword=secretpassword --namespace wordpress
```

With the install command, Helm will launch the required Deployments, ReplicaSets, Pods, Services, ConfigMaps, or any other Kubernetes resource the chart defines.

The notes will provide helpful information on how to access the new services.

You can inspect the deployment of the application resources using the following command:

```
kubectl get all -n wordpress
```

Use the helm list command with the --all-namespaces option to list releases across all namespaces.

```
helm list --all-namespaces
```

Or list releases to a specific namespace scope:

```
helm ls -n wordpress
```

## Chart Installation Information

Deployment information is maintained in a secret stored on the targeted Kubernetes cluster.

```
kubectl get secrets --all-namespaces --selector owner=helm
```

For the Wordpress chart, you installed to the wordpress namespace you can see the secret information about the deployment:

```
kubectl --namespace wordpress describe secret sh.helm.release.v1.my-wordpress.v1
```

Notice the field Release which stores the release state.

```
kubectl --namespace wordpress get secret sh.helm.release.v1.my-wordpress.v1 -o jsonpath='{.data.release}' | base64 -d | base64 -d | gunzip -c
```

However, the Helm client offers a friendly way to inspect your release directly using the helm get commands:

```
helm get all - get all information for a named release
helm get hooks - get all hooks for a named release
helm get manifest - get the manifest for a named release
helm get notes - get the notes for a named release
helm get values - get the values file for a named release
```

Run the following commands to see the output:

```
helm get manifest my-wordpress --namespace wordpress
helm get notes my-wordpress --namespace wordpress
helm get values my-wordpress --namespace wordpress
helm get all my-wordpress --namespace wordpress
```

The difference between get and show is that the former helps you get information about a release while the latter helps you get information about a chart.
