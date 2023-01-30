---
layout: post
title:  "Taking database backup from RDS snapshots for multitenancy application"
author: Mayuresh
categories: [ Rails, AWS ]
image: assets/images/rds.png
---
By default, Amazon RDS creates and saves automated backups of your DB instance securely in Amazon S3 for a particular retention period, we can change the period or we can also backup it manually on any particular day.

To restore the backup aws have provided one option of restore snapshot which will restore the whole RDS instance.

But suppose you have a multitenancy application and you need to revert only one tenant’s database.

Assume the case:

your RDS has different databases like DB1, DB2, and DB3

but you have to restore only DB2, not DB1 and DB3

for this, you need to follow the following steps:


1. first of all create a temporary RDS instance, you can also create it while restoring the snapshot process
![imgage_rds.png](/assets/images/rds_content_2.png)
2. restore the snapshot backup on that particular RDS instance
![Restore Snapshot](/assets/images/rds_content_1.png)

3. then use an application that will connect to the database our RDS engine was PostgreSQL so we used PGAdmin
4. Connect to that particular instance from PGAdmin or anything else by the credentials of RDS
5. Now you can see the different databases on that restored instance
6. Then backup manually the current tenant database
7. delete that particular RDS instance and work


There you go you got the database backup of a particular instance then you can manually install/put that database dump on a particular tenant’s database.
