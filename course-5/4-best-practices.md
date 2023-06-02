This section will cover the top 5 storage best practices for Kubernetes:

1. Architecture
The platform used to protect Kubernetes applications needs to automatically discover all the application components running on your cluster and must include the state that spans across storage volumes, databases (NoSQL/Relational) as well as configuration data included in Kubernetes objects such as configmaps and secrets.

2. Recoverability
The data management platforms must allow you to restore the application components you want and where you want them. You should also have the granularity to restore only an application subset such as the data volume, ConfigMaps, or Secrets. The approach must make restoring simple and powerful by also allowing you to select the appropriate point of time copy of the application.

3. Operations
It is important to ensure that a Kubernetes-native backup platform can be used at scale, provide operations teams with the workflow capabilities they require, and meet compliance and monitoring requirements.

Operators should be able to give self service capabilities to application developers without requiring application code or deployment changes.

4. Security
Controls around identity and access management and role-based access control (RBAC) must be implemented. RBAC allows different personas in an operations team to adopt a least-privilege approach to common tasks such as monitoring. Encryption at rest and in transit must always be implemented to ensure that data is secure whenever it has left the compute environment.

5. Application Portability
Living in a multi-hybrid-cloud world, a cloud-native data management platform needs to be flexible in the support for multiple distributions and cloud vendors. It needs to offer capabilities that allow it to be installed, deployed, and managed across all these diverse environments. Portability capabilities are required across multiple use cases including application restoring, cloning, and migration.
