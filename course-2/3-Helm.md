Helm is the package manager for Kubernetes that allows you to package, share and manage the lifecycle of your Kubernetes containerized applications. Essentially you create structured application packages that contain everything they need to run on a Kubernetes cluster; including dependencies the application requires.

Helm uses Charts to pack all the required K8S components for an application to deploy, run and scale. A chart is a collection of files that describe a related set of Kubernetes resources. A single chart might be used to deploy something simple, like a memcached pod, or something complex, like a full web app stack with HTTP servers, databases, caches, and so on.

Helm Templates is subdirectory in a chart that combines the K8S components of it, e.g. Service, ReplicaSet, Deployment, Ingress etc.

Helm Values are described in the values.yaml file which allows users to deploy their containerized applications dynamically. Its Yaml structure holds values that matches the templates defined in the application manifest.
