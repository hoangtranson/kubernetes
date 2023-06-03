# Installing the Helm CLI

Install Helm

Use this command to install the latest version of Helm on Linux:

```
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
```

Verify the installation by running the following command:

```
helm version --short
```

The current local state of Helm which defines the runtime configuration of the client is kept in environment variables that can be fetched using the helm env command:

```
helm env
```

![image](https://github.com/hoangtranson/kubernetes/assets/35447677/1fd4cf78-362a-4acf-b92c-e6090bc781b6)

The Helm command line tool defaults to discovering the current Kubernetes host by reading the same configuration that kubectl uses in ~/.kube/config. That way, Helm will be able to connect to the Kubernetes cluster that's currently in context.
