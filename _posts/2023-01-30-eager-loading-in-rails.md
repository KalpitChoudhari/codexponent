---
layout: post
title:  "Eager loading in rails"
author: Kalpit
categories: [ Ruby, Rails ]
image: assets/images/eager_loading.png
absolute_url: eager-loading-in-rails
---
Eager loading is noting but loading all the associated records using as few queries as possible.

Most of us heard about N + 1 query problem.

Let’s assume we have 2 models as `User` and `Company`. As association as follows

```ruby
# models/user.rb

class User < ApplicationRecord
	has_one :company
end
```

```ruby
# models/comapny.rb
class Company < ApplicationRecord
	belongs_to :user
end
```

 Consider the following code, which finds all the users and prints the company name

```ruby
users = User.all

users.each do |user|
	puts user.company.name
end
```

Above code looks fine and in working condition. But we have problem of N + 1 query here.

The above code executes 1 (find all users) + N (one per each user to load company) = N + 1 queries.

Console output for above queries:

```ruby
3.0.3 :006 > users = User.all
  User Load (0.6ms)  SELECT "users".* FROM "users"
 =>                                                                
[#<User:0x0000000115644b30                                         
...                                                                
3.0.3 :007 > users.each do |user|
3.0.3 :008 >   puts user.company.name
3.0.3 :009 > end
  Company Load (0.7ms)  SELECT "companies".* FROM "companies" WHERE "companies"."user_id" = $1 LIMIT $2  [["user_id", 1], ["LIMIT", 1]]
Pres. Timmy Mayer                                                                         
  Company Load (0.7ms)  SELECT "companies".* FROM "companies" WHERE "companies"."user_id" = $1 LIMIT $2  [["user_id", 2], ["LIMIT", 1]]
Li Weber                                                                                  
  Company Load (0.2ms)  SELECT "companies".* FROM "companies" WHERE "companies"."user_id" = $1 LIMIT $2  [["user_id", 3], ["LIMIT", 1]]
Emmett Dicki                                                                              
  Company Load (0.1ms)  SELECT "companies".* FROM "companies" WHERE "companies"."user_id" = $1 LIMIT $2  [["user_id", 4], ["LIMIT", 1]]
Jessie Johnson LLD                                                                        
  Company Load (0.2ms)  SELECT "companies".* FROM "companies" WHERE "companies"."user_id" = $1 LIMIT $2  [["user_id", 5], ["LIMIT", 1]]
Jesus Ankunding                                                                           
  Company Load (0.1ms)  SELECT "companies".* FROM "companies" WHERE "companies"."user_id" = $1 LIMIT $2  [["user_id", 6], ["LIMIT", 1]]
Oswaldo Kris                                                                              
  Company Load (0.1ms)  SELECT "companies".* FROM "companies" WHERE "companies"."user_id" = $1 LIMIT $2  [["user_id", 7], ["LIMIT", 1]]
Miss Rich Reynolds                                                                        
  Company Load (0.1ms)  SELECT "companies".* FROM "companies" WHERE "companies"."user_id" = $1 LIMIT $2  [["user_id", 8], ["LIMIT", 1]]
The Hon. Felipe Casper
  Company Load (0.1ms)  SELECT "companies".* FROM "companies" WHERE "companies"."user_id" = $1 LIMIT $2  [["user_id", 9], ["LIMIT", 1]]
Stan Willms
  Company Load (0.1ms)  SELECT "companies".* FROM "companies" WHERE "companies"."user_id" = $1 LIMIT $2  [["user_id", 10], ["LIMIT", 1]]
Gov. Alleen Murray
```

### Solution to N + 1 queries problem:

We can use `.includes()` in order to solve N + 1 query problem.

The `includes` method eager loads all the associated records. Hence, no need to load associated records every time. No need to call or execute extra query each time when we call associated record.

Using includes in above code:

```ruby
users = User.incldues(:comapny)

users.each do |user|
	puts user.comapny.name
end
```

Output for above code:

```ruby
3.0.3 :010 > users = User.includes(:company)
  User Load (1.0ms)  SELECT "users".* FROM "users"
  Company Load (1.2ms)  SELECT "companies".* FROM "companies" WHERE "companies"."user_id" IN ($1, $2, $3, $4, $5, $6, $7, $8, $9, $10)  [["user_id", 1], ["user_id", 2], ["user_id", 3], ["user_id", 4], ["user_id", 5], ["user_id", 6], ["user_id", 7], ["user_id", 8], ["user_id", 9], ["user_id", 10]]
 =>                                                                                           
[#<User:0x00000001148cd5b0                                                                    
...                                                                                           
3.0.3 :011 > user.each do |user|
3.0.3 :012 >   puts user.company.name
3.0.3 :013 > end
Pres. Timmy Mayer
Li Weber                                                                                      
Emmett Dicki                                                                                  
Jessie Johnson LLD                                                                            
Jesus Ankunding                                                                               
Oswaldo Kris                                                                                  
Miss Rich Reynolds
The Hon. Felipe Casper
Stan Willms
Gov. Alleen Murray
```

Note that only 2 queries are executed when we used includes method. In this way we can optimise performance of our rails application. 🥂

Stay tuned for difference between `incldues`, `preload` and `eager_load`.
