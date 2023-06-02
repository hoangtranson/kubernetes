# Kasten K10 Backups and Disaster Recovery

Protecting an application with K10, usually accomplished by creating a policy, requires the understanding and use of three concepts:

- Snapshots and Backups: Depending on your environment and requirement, you might need just one or both of these data capture mechanisms
- Scheduling: Specification of application capture frequency and snapshot/backup retention objectives
- Selection: This defines not just which applications are protected by a policy but, whenever finer-grained control is needed, resource filtering can be used to restrict what is captured on a per-application basis

Snapshots and Backups
Snapshots are the basis of persistent data capture in K10. They are usually used in the context of disk volumes (PVC/PVs) used by the application but can also apply to application-level data capture (e.g., with Kanister).

Snapshots, in most storage systems, are very efficient in terms of having a very low performance impact on the primary workload, requiring no downtime, supporting fast restore times, and implementing incremental data capture.

Backup operations convert application and volume snapshots into backups by transforming them into an infrastructure-independent format and then deduplicating, compressing, and encrypting them before storing them in an object store or an NFS file store.

## Scheduling

There are four components to scheduling:

- How frequently the primary snapshot action should be performed
  - Actions can be set to execute at an hourly, daily, weekly, monthly, or yearly granularity, or on demand.
  - Advanced schedule options allow customers to configure the snapshots to run every 5 minutes, 10 minutes, 20 minutes or 30 minutes..etc
- How often snapshots should be exported into backups
  - Backups performed via exports, by default, will be set up to export every snapshot into a backup. However, it is also possible to select a subset of snapshots for exports (e.g., only convert every daily snapshot into a backup).
- Retention schedule of snapshots and backups
  - A powerful scheduling feature in K10 is the ability to use a GFS retention scheme


## Selection

You can select applications by two specific methods:
- Application Names
- Labels


The most straightforward way to apply a policy to an application is to use its name (which is derived from the namespace name).

For policies that need to span multiple applications (e.g., protect all applications that use MongoDB or applications that have been annotated with the gold label), you can select applications by label. Any application (namespace) that has a matching label as defined in the policy will be selected.
