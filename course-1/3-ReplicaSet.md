A ReplicaSet controller ensures that a specified number of Pod replicas are running at any given time.

While ReplicaSets can be used independently, today it’s mainly used by Deployments as a mechanism to orchestrate Pod creation, deletion and updates. When you use Deployments you don’t have to worry about managing the ReplicaSets that they create. Deployments own and manage their ReplicaSets.
