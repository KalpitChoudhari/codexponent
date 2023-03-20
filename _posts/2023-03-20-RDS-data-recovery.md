---
layout: post
title:  "Effortless Data Recovery: Choose Your Timeline with Multiple Backups a Day or Selective Restores from RDS"
author: Mayuresh
categories: [ AWS, Rails ]
image: assets/images/rds_db.png
---

If you want your database to be automatically backed up twice or thrice a day, you don’t actually need to perform multiple backups. AWS RDS provides a feature called "point-in-time recovery", which allows you to choose which time you want to restore from any point in time across all your snapshots.

Suppose you are creating automated snapshots on a daily basis with a retention period of 14 days. In the last 14 days, you can jump to any particular time of a backup. For example, if you create a snapshot daily at 7 pm and want to restore a backup from yesterday at 10 pm, then you can use point-in-time recovery.

Steps to recover the database at a particular time:-

- Go to the AWS console
- navigate to RDS
- click on automated backups
  - then you will see all the snapshots which are taken and retained
- Choose the appropriate RDS instance
- Then click on actions in which select Restore to point in time

![RDS Restore 1](/assets/images/rds_1.png)

- On this page Choose the particular time frame in which you want to restore the backup
  - Afterward, choose the essential information for snapshot backup

![RDS Restore 2](/assets/images/rds_2.png)

`Point-in-time recovery` negates the need for twice-daily backups. However, if you still want to perform backups twice a day (which doesn’t make much sense), you can create a lambda function to invoke manual snapshot creation and add CloudWatch Event Rules (to kick off the Lambda at a certain time, basically acting as a cron).
