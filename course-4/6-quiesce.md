## Quiescing function

To quiesce is to pause or alter a device or application to achieve a consistent state, usually in preparation for a backup or other maintenance. Application-consistent backups can be enabled if the data service needs to be quiesced before a volume snapshot is initiated.

To obtain an application-consistent backup, a quiescing function, as defined in the application blueprint, is first invoked and is followed by a volume snapshot.

To shorten the time spent while the application is quiesced, it is unquiesced based on the blueprint definition as soon as the storage system has indicated that a point-in-time copy of the underlying volume has been started. The backup will complete asynchronously in the background when the volume snapshot is complete, or in other words after unquiescing the application, K10 waits for the snapshot to complete. An advantage of this approach is that the database is not locked for the entire duration of the volume snapshot process.
