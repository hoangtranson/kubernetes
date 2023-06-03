# Adding a Chart Repository

A Helm chart is an individual package that can be installed into your Kubernetes cluster. During chart development, you will often just work with a chart that is stored on your local filesystem.

But when it comes to sharing charts, Helm describes a standard format for indexing and sharing information about Helm charts.

A Helm chart repository is simply a set of files, reachable over the network, that conforms to the Helm specification for indexing packages.

A chart repository is an HTTP server that houses an index.yaml file and optionally some packaged charts. When you're ready to share your charts, the preferred way to do so is by uploading them to a chart repository.

Adding a Helm chart is done with the helm repo add command.

## Searching a Chart Repository

The easiest way to find the popular repositories is to use your web browser to navigate to the Artifact Hub. There you will find thousands of Helm charts, each hosted on an appropriate repository.

You can also use the command `helm search repo REPOSITORY_NAME` to search for a chart in an added repository.

## Searching Repositories

The helm search command allows you to look for charts available in the various places, including the Artifact Hub and repositories you have added.

Use Helm Hub (https://artifacthub.io/)

There are over 9000 charts available in the Artifact Hub that you can explore using the helm search hub command. More charts are added daily.

Run the following command to count the number of repositores currently available:

```
helm search hub | wc -l
```

You can narrow down your search by specifying the name of the application and repository you're looking for by running the following command:

```
helm search hub wordpress | grep bitnami
```

Next, check if the Bitnami repo is already added locally:

```
helm repo list
helm search hub sonarqube
helm search hub rabbitmq
helm search hub kafka
helm search hub prometheus-operator
helm search hub tensorflow
helm search hub tekton
```

Instead of searching the Hub for charts you can also search the Bitnami repo.

Add the repository:

```
helm repo add bitnami https://charts.bitnami.com/bitnami
```

Search the repository using the helm search repo command:

```
helm search repo bitnami/wordpress --output yaml --versions
```

The Helm show command consists of multiple subcommands which can reveal additional information about the chart:

```
all         show all information of the chart
chart       show the chart's definition
readme      show the chart's README
values      show the chart's values
```

For example:

```
helm show all bitnami/wordpress
helm show chart bitnami/wordpress
helm show readme bitnami/wordpress
helm show values bitnami/wordpress
```

Explore Repositories

Just to emphasize:

```
echo "The number of chart on Helm Hub is: $(helm search hub | wc -l)."
```

There are numerous common charts that, as a Kubernetes developer, you may want to leverage:

```
helm search hub postgres
helm search hub sonarqube
```
