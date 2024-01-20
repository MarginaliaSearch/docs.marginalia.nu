---
title: '4.4 Backups'
---

The system automatically snapshots the index data before the index is constructed.  This allows relatively quick rollbacks of the index if for some reason this operation needs to be undone. The index data is stored in the `index-n/backup` directory, where `n` is the index node number. 

Backup restoration is done from the control interface, under `Node N->Actions->Restore Backup`.  This will restore the backup and the index will be rebuilt from there.  Reconstructing the index from a backup will take a while, but the system will be usable during this time.  The domain database
will not be affected by this operation.

It may be a good practice to prune these backups from time to time, as they can take up a lot of space.  Each directory is named with an ISO-8601-style timestamp (e.g. `snapshot__YY_MM_DDThh_mm_ss_SSS`), so it's easy to see how old the backups are.  The backups are never automatically pruned, as this is a destructive operation, and it's not clear how many backups should be kept.  