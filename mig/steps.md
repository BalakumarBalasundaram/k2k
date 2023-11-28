# MigrateSteps

https://docs.confluent.io/platform/current/multi-dc-deployments/cluster-linking/migrate-cp.html

When migrating data from an old cluster to a new cluster, Cluster Linking makes an identical copy of your topics on the new cluster, so it’s easy to make the move with low downtime and no data loss.

Cluster Linking can:

Automatically create matching “mirror” topics with the same configurations, so you don’t have to recreate your topics by hand
Sync all historical data and new data from the existing topics to the new mirror topics
Sync your consumer offsets, so your consumers can pick up exactly where they left off, without missing any messages or consuming any duplicates
Move consumers from the old cluster to the new cluster independently
Move producers from the old cluster to the new cluster topic-by-topic
