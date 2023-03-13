---
layout: post
title:  "Effortless Database Migration: Converting MySQL to PostgreSQL with pgloader Gem in Your Rails Application"
author: Kalpit
categories: [ Ruby, Rails ]
image: assets/images/migration.png
---

As your application grows in size, migrating to a different database can be a daunting task. In this blog, we'll explore a step-by-step guide on how to effortlessly switch your database configuration from MySQL to PostgreSQL.

Before moving forward, please note to take backup of your database. (Extremely important)

You can take backup of mysql DB as follows:

```ruby
mysqldump -u <USER_NAME> -p<PASSWORD> -i -c -q <DATABASE_NAME> > <FILE_NAME>

Usage:
mysqldump -u root -i -c -q migration_demo_development > db_backup.psql
```

Now that you’ve created backup, let’s take a look on how to convert to PostgreSQL:

1. Replace `gem "mysql2"` to  `gem "pg"` in your `gemfile`
2. Update `database.yml` [Update `adapter`, `encoding` and remove `socket` in existing config]
    1. Before:
    
    ```ruby
    default: &default
      adapter: mysql2
      encoding: utf8mb4
      pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
      username: root
      password:
      socket: /tmp/mysql.sock
    ```
    
    b. After:
    
    ```ruby
    default: &default
      adapter: postgresql
      encoding: utf8
      pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
      username: root
      password:
    ```
    
3. Run command `rails db:create` to create postgres database
4. Run `rails db:schema:load` to load the schema file into the database
5. Install `pgloader` by executing: `apt-get install pgloader`, `brew install pgloader`
6. Run `pgloader mysql://<HOST>@<USER>/<SOURCE_DB_NAME> pgsql:///<DESTINATION_DB_NAME>`
