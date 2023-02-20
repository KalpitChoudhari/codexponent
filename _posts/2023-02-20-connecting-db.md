---
layout: post
title:  "Connecting RDS DB from PgAdmin"
author: Mayuresh
categories: [ AWS, Database ]
image: assets/images/postgres.png
---

Hey folks,

Suppose you’ve created an RDS instance that is running the PostgreSQL engine you need to do some debugging in it and you need to see its contents so for that purpose you should establish a connection to that particular RDS instance

for that while creating an RDS instance

set username and password.

for passwords you can use AWS secret manager service(we’ll write about it)

and afterward, we need to add a security group to it

that’s where you need to be careful

Create a different security group (recommended)

and in inbound rules, you need to add the following configuration

1. choose PostgreSQL as a type

then it will automatically choose the protocol and port range

1. then choose the source as custom
2. the value will be your public IP

to find your public IP go to google and search what is my IP

![aws security group](/assets/images/sgg.png)

now your RDS is ready to access from local
we’re using PgAdmin 4 which is a PostgreSQL client
Enter some name of your server(temp-server)

![PgAdmn page 2](/assets/images/pgagdmin_page1.png)

and then add the RDS endpoint in the Host name/address
The port will be 5432 by default
Database name which is PostgreSQL by default
then enter username and password

![PgAdmn page 2](/assets/images/pggadmin_page2.png)

and now you’re inside the RDS instance
if you want to see tables you can find them under public under the schema