Containers allow you to package your application and its dependencies together into one succinct manifest that can be version controlled, allowing for easy replication of your application across developers on your team and machines in your cluster.

Containers work best for service based architectures. As opposed to monolithic architectures, where every piece of the application is intertwined — from IO to data processing to rendering — service based architectures separate these into separate components.

From an application point of view, instantiating an image (creating a container) is similar to instantiating a process like a service or a web app.

A stateless app is an application program that does not save client data generated in one session for use in the next session with that client e.g. print services, microservices.

In stateful applications, the state is recorded. By state, we mean any changeable occurrence that includes internal operations, interactions with other applications, environment variables, user-set preferences, memory content, and temporary storage. The data that such applications store depends on their types and other factors under which they operate. Usually, a stateful application is able to record your preferences, track your window size and location, and memorize the files that you have recently opened. Some known examples of stateful applications include MongoDB, Cassandra, and MySQL.
