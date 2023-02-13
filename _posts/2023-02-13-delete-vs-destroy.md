---
layout: post
title:  "Delete VS Destroy in Rails"
author: Kalpit
categories: [ Ruby, Rails ]
image: assets/images/delete_vs_destroy.png
---

In this blog, we are going to see the difference between delete and destroy methods in rails.

Let’s take an example where we have following models: `User`, `Post`, `Comment`


```ruby
# app/models/user.rb

class User < ApplicationRecord
  self.inheritance_column = :_type_disabled

  has_many :posts, dependent: :destroy
  has_many :comments, through: :posts
end
```

```ruby
# app/models/post.rb

class Post < ApplicationRecord
  belongs_to :user

  has_many :comments, dependent: :destroy
end
```

```ruby
# app/models/comment.rb

class Comment < ApplicationRecord
  belongs_to :post
end
```

```ruby
3.0.3 :067 > User.count   #=> 10
  User Count (1.0ms)  SELECT COUNT(*) FROM "users"

3.0.3 :068 > Post.count   #=> 7
  Post Count (0.5ms)  SELECT COUNT(*) FROM "posts"

3.0.3 :069 > Comment.count   #=> 12
  Comment Count (0.6ms)  SELECT COUNT(*) FROM "comments"
```

### Destroy:

Destroy method erase record from table. Also, it will erase all the dependent records as well. Because of this functionality, destroy is time consuming as compared to delete method.

Example:

```ruby

3.0.3 :070 > User.first
  User Load (0.4ms)  SELECT "users".* FROM "users" ORDER BY "users"."id" ASC LIMIT $1  [["LIMIT", 1]]
=> #<User:0x000000010cdd67a8                                                     
 id: 2,                                                                          
 email: "micah@fisher.co",                                                       
 password: "[FILTERED]",                                                         
 name: "Curt Mertz Ret.",                                                        
 type: "simple",                                                                 
 created_at: Sat, 04 Feb 2023 17:14:59.737597000 UTC +00:00,                     
 updated_at: Sat, 04 Feb 2023 17:14:59.737597000 UTC +00:00>

3.0.3 :071 > User.first.posts.count   #=> 3
  User Load (0.5ms)  SELECT "users".* FROM "users" ORDER BY "users"."id" ASC LIMIT $1  [["LIMIT", 1]]
  Post Count (1.3ms)  SELECT COUNT(*) FROM "posts" WHERE "posts"."user_id" = $1  [["user_id", 2]]

3.0.3 :072 > User.first.comments.count   #=> 5
  User Load (0.4ms)  SELECT "users".* FROM "users" ORDER BY "users"."id" ASC LIMIT $1  [["LIMIT", 1]]
  Comment Count (0.5ms)  SELECT COUNT(*) FROM "comments" INNER JOIN "posts" ON "comments"."post_id" = "posts"."id" WHERE "posts"."user_id" = $1  [["user_id", 2]]
```

If we destroy user then output will be:

```ruby
3.0.3 :075 > User.first.destroy
  User Load (0.5ms)  SELECT "users".* FROM "users" ORDER BY "users"."id" ASC LIMIT $1  [["LIMIT", 1]]
  TRANSACTION (0.1ms)  BEGIN                                                              
  Post Load (0.3ms)  SELECT "posts".* FROM "posts" WHERE "posts"."user_id" = $1  [["user_id", 2]]
  Comment Load (0.3ms)  SELECT "comments".* FROM "comments" WHERE "comments"."post_id" = $1  [["post_id", 2]]
  Post Destroy (0.5ms)  DELETE FROM "posts" WHERE "posts"."id" = $1  [["id", 2]] 
  Comment Load (0.3ms)  SELECT "comments".* FROM "comments" WHERE "comments"."post_id" = $1  [["post_id", 3]]
  Comment Destroy (0.3ms)  DELETE FROM "comments" WHERE "comments"."id" = $1  [["id", 2]]
  Comment Destroy (0.2ms)  DELETE FROM "comments" WHERE "comments"."id" = $1  [["id", 3]]
  Comment Destroy (0.1ms)  DELETE FROM "comments" WHERE "comments"."id" = $1  [["id", 4]]
  Post Destroy (0.1ms)  DELETE FROM "posts" WHERE "posts"."id" = $1  [["id", 3]] 
  Comment Load (0.1ms)  SELECT "comments".* FROM "comments" WHERE "comments"."post_id" = $1  [["post_id", 4]]
  Comment Destroy (0.3ms)  DELETE FROM "comments" WHERE "comments"."id" = $1  [["id", 5]]
  Comment Destroy (0.2ms)  DELETE FROM "comments" WHERE "comments"."id" = $1  [["id", 6]]
  Post Destroy (0.1ms)  DELETE FROM "posts" WHERE "posts"."id" = $1  [["id", 4]] 
  User Destroy (0.2ms)  DELETE FROM "users" WHERE "users"."id" = $1  [["id", 2]] 
  TRANSACTION (1.0ms)  COMMIT
```

See in above example, all the dependent records i.e. posts and comments gets also erased from table in case of destroy method.

After destroying user:

```ruby
3.0.3 :076 > User.count   #=> 9
  User Count (0.6ms)  SELECT COUNT(*) FROM "users"

3.0.3 :077 > Post.count   #=> 4
  Post Count (0.6ms)  SELECT COUNT(*) FROM "posts"

3.0.3 :078 > Comment.count   #=> 7
  Comment Count (0.5ms)  SELECT COUNT(*) FROM "comments"
```

### Delete:

Delete method simply deletes the record in row. Unlike destroy, delete does not erase dependent records in table. Because of this, delete is faster as compared to destroy method.

```ruby
3.0.3 :083 > User.last
  User Load (0.5ms)  SELECT "users".* FROM "users" ORDER BY "users"."id" DESC LIMIT $1  [["LIMIT", 1]]
=> #<User:0x0000000107b6f668                                
 id: 11,                                                    
 email: "crissy.mohr@homenick-schuster.biz",                
 password: "[FILTERED]",                                    
 name: "Margarite Ondricka",                                
 type: "simple",                                            
 created_at: Sat, 04 Feb 2023 17:14:59.762022000 UTC +00:00,
 updated_at: Sat, 04 Feb 2023 17:14:59.762022000 UTC +00:00>

3.0.3 :084 > User.last.posts.count   #=> 3
  User Load (0.7ms)  SELECT "users".* FROM "users" ORDER BY "users"."id" DESC LIMIT $1  [["LIMIT", 1]]
  Post Count (0.4ms)  SELECT COUNT(*) FROM "posts" WHERE "posts"."user_id" = $1  [["user_id", 11]]

3.0.3 :085 > User.last.comments.count   #=> 6
  User Load (0.4ms)  SELECT "users".* FROM "users" ORDER BY "users"."id" DESC LIMIT $1  [["LIMIT", 1]]
  Comment Count (0.4ms)  SELECT COUNT(*) FROM "comments" INNER JOIN "posts" ON "comments"."post_id" = "posts"."id" WHERE "posts"."user_id" = $1  [["user_id", 11]]
```

Let’s see the output when we delete above ☝🏼 user:

```ruby
3.0.3 :086 > User.last.delete
  User Load (1.3ms)  SELECT "users".* FROM "users" ORDER BY "users"."id" DESC LIMIT $1  [["LIMIT", 1]]
  User Destroy (1.5ms)  DELETE FROM "users" WHERE "users"."id" = $1  [["id", 11]]     
=> #<User:0x000000010bec91e8                                                    
 id: 11,                                                                        
 email: "crissy.mohr@homenick-schuster.biz",                                    
 password: "[FILTERED]",                                                        
 name: "Margarite Ondricka",                                                    
 type: "simple",                                                                
 created_at: Sat, 04 Feb 2023 17:14:59.762022000 UTC +00:00,                    
 updated_at: Sat, 04 Feb 2023 17:14:59.762022000 UTC +00:00>
```

Note that only user gets deleted. All the dependent tables i.e. post and comments are present in the system.

```ruby
3.0.3 :087 > User.count   #=> 8
  User Count (0.5ms)  SELECT COUNT(*) FROM "users"

3.0.3 :088 > Post.count   #=> 4
  Post Count (0.5ms)  SELECT COUNT(*) FROM "posts"

3.0.3 :089 > Comment.count   #=> 7
  Comment Count (0.8ms)  SELECT COUNT(*) FROM "comments"
```

If we try to access deleted user from dependent table (post or comment) then, we got `nil` as result.

Example:

```ruby
3.0.3 :092 > Post.last.user   #=> nil
  Post Load (0.4ms)  SELECT "posts".* FROM "posts" ORDER BY "posts"."id" DESC LIMIT $1  [["LIMIT", 1]]
  User Load (0.1ms)  SELECT "users".* FROM "users" WHERE "users"."id" = $1 LIMIT $2  [["id", 11], ["LIMIT", 1]]
```

P.S:- Now that we have understood the difference between `destroy` and `delete`, use them as per requirement 💣. Do not use `destroy` if there is no necessity 💥.

Stay tune for upcoming blog explaining how to `soft delete` record(s) in Rails.
