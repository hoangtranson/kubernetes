Kubernetes Native Backup

Kubernetes-native backup is one of the most critical, yet overlooked, challenges faced by Kubernetes operators today.

Kubernetes, the fastest-growing infrastructure platform, is quickly becoming the foundation for all applications, no matter where they might be deployed. Its ubiquitous nature has it on track to be the next enterprise platform of choice.

The Kubernetes platform is fundamentally different from all earlier compute infrastructures. It uses its own placement policy to distribute application components across all servers for fault-tolerance and performance. As such, different applications may be co-located on the same server. Also, there is the dynamic nature of cloud native environments: containers can be dynamically rescheduled or scaled on different nodes for better load balancing. New deployments, that happen on an hourly basis, can involve rolling upgrades, and new application components can be added or removed at any time.

In short, the definition of a cloud native application is constantly shifting. A data management solution needs to understand this cloud native architectural pattern, be able to work with a lack of IP address stability, and be able to deal with continuous change.

Purpose-built for Kubernetes, Kasten K10 provides enterprise operations teams an easy-to-use, scalable, and secure system for backup/restore, disaster recovery, and mobility of Kubernetes applications.

Kanister vs K10

For stateful, cloud-native applications, data operations must often be performed by tools with a semantic understanding of the data. The volume-level primitives provided by orchestrators are not sufficient to support data workflows like backup/recovery of complex, distributed databases.

K10 supports a number of different community databases that includes MySQL, PostgreSQL, MongoDB, and Cassandra.

![image](https://github.com/hoangtranson/kubernetes/assets/35447677/a6f72e0d-7347-46ee-92a7-16cb3568f909)

To bridge this gap between operational requirements for these applications and Kubernetes, the open source project Kanister was created. Kanister is a framework to support application-level data management in Kubernetes. It lets developers define relationships between tools and applications, and then makes running those tools in Kubernetes simple.

Kasten's K10 platform uses the Kubernetes custom resources provided by Kanister for data management. This enables domain experts to capture application specific data management tasks in blueprints which can be easily shared and extended. The framework takes care of the tedious details around execution on Kubernetes and presents a homogeneous operational experience across applications at scale. Further, it gives the user a natural mechanism to extend the K10 platform by adding personal code to modify any desired step performed for data lifecycle management.
