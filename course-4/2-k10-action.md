An Action API resource is used to initiate Kasten K10 data management operations. The actions can either be associated with a Policy or be stand-alone on-demand actions. Actions also allow for tracking the execution status of the requested operations.

The Kasten K10 Platform exposes a number of different action types.

BackupAction

Backup actions are used to initiate backup operations on applications. A backup action can be submitted as part of a policy or as a standalone action.

RestoreAction

Restore actions are used to restore applications to a known-good state from a restore point.

ExportAction

Export actions are used to initiate an export of an application to external data storage, such as S3-compatible object stores, using an existing restore point.

BackupClusterAction

Backup cluster actions are used to initiate backup operations on cluster-scoped resources. A backup cluster action can be submitted as part of a policy or as a standalone action.

RestoreClusterAction

Restore cluster actions are used to restore cluster-scoped resources from a ClusterRestorePoint. A restore cluster action can be submitted as part of a policy or as a standalone action.

RunAction

RunActions are used for manual execution and monitoring of actions related to policy runs.

CancelAction

CancelActions are created to halt progress of another action and prevent any remaining retries. Cancellation is best effort and not every phase of an Action may be cancellable. When an action is cancelled, its state becomes Cancelled.

ReportAction

A ReportAction resource is created to generate a K10 Report and provide insights into system performance and status. A successful ReportAction produces a K10 Report that contains information gathered at the time of the ReportAction.
