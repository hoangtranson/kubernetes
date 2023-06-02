# Profiles

A Profile custom resource (CR) is used to perform operations on K10 Profiles. You can learn more about using K10 Profiles at Location Configuration.

Location profiles (https://docs.kasten.io/latest/usage/configuration.html#location-profiles) are used to create backups from snapshots, move applications and their data across clusters and potentially across different clouds, and to subsequently import these backups or exports into another cluster.

K10 can usually invoke protection operations such as snapshots within a cluster without requiring additional credentials. While this might be sufficient if K10 is running in some of (but not all) the major public clouds and if actions are limited to a single cluster, it is not sufficient for essential operations. These include performing real backups, enabling cross-cluster and cross-cloud application migration, and enabling DR of the K10 system itself.
