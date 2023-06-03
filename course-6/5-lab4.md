# Upgrading an Installation

When we talk about upgrading in Helm, we talk about upgrading an installation, not a chart. An installation is a particular instance of a chart in your cluster, also known as Release. When you run helm install, it creates the installation. To modify that installation, use helm upgrade.

This is an important distinction to make in the present context because upgrading an installation can consist of two different kinds of changes:

- You can upgrade the version of the chart
- You can upgrade the configuration of the installation

The two are not mutually exclusive; you can do both at the same time. But this does introduce one new term that Helm users refer to when talking about their systems: a release is a particular combination of configuration and chart version for an installation.

When we first install a chart, we create the initial release of an installation. Let’s call this release 1. When we perform an upgrade, we are creating a new release of the same installation: release 2. When we upgrade again, we will create release 3, and so on.

## Upgrade Helm Releases

Upgrading
The --reuse-values flag will tell Helm to reload the server-side copy of the last set of values, and then use those to generate the upgrade.

```
helm upgrade my-wordpress bitnami/wordpress -n wordpress --reuse-values --set service.nodePorts.http=32000 --set mariadb.auth.rootPassword=secretPassword --set mariadb.auth.password=secretPassword --set wordpressPassword=wpPassword --set replicaCount=3
```

Here we are upgrading our application by increasing the number of replicaCount to 3 whilst reusing the same configuration parameters we passed during installation.

When the upgrade is complete, check the pods and the replica set count:

```
kubectl get pods -n wordpress --no-headers -l app.kubernetes.io/name=wordpress
```

The deployment has scaled the number of pods from 1 to 3.

## Uninstall Helm Releases

Clean Up

To remove a Helm installation, use the helm uninstall command:

```
helm uninstall my-wordpress -n wordpress
```

The command will delete all the resources we deployed as part of the chart installation except for Persistent Volumes which we will have to delete manually by deleting the namespace.

Next, delete the namespace:

```
kubectl delete namespace wordpress
```

This will delete all the Persistent Volume Claim resources which will automatically delete the PVs they are bound to.
