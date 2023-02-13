---
layout: post
title:  "Soft Delete in Ruby on rails"
author: Mayuresh
categories: [ Ruby, Rails ]
image: assets/images/soft_delete.png
---
Building a web app that can perform basic crud operations can be easily done with Rails.

soft delete making the record appear to be deleted while keeping the actual record in the database

this can easily be done by adding a flag column to check the column while querying that particular database 

rails provide various implementations for soft deleting records 

1) **Paranoia**

2) **discard**

**PARANOIA :-** 

paranoia was a reimplementation of `acts_as_paranoid` rails plugin

it has the ability to hide and restore database records

suppose we are using `acts_as_paranoid` in a model and performing destroy on it will not delete the actual record instead of that 

it will add the current time in the `deleted_at` column and hides the records by scoping all the queries on your model which do not have a `deleted_at` field

**Installation**:-

```ruby
gem 'paranoia'
```

then 

```ruby
bundle install
```

then after that run a migration that will ad. deleted_at field in the particular model in which you want to soft delete records

```ruby
bin/rails generate migration AddDeletedAtToUsers deleted_at:datetime:index
```

and then you will have a migration like this

```ruby
class AddDeletedAtToUsers < ActiveRecord::Migration
  def change
    add_column :users, :deleted_at, :datetime
    add_index :users, :deleted_at
  end
end
```

after that

```ruby
rails db:migrate
```

**Usage**:-

suppose you have a model User, add `acts_as_paranoid` 

```ruby
class User < ActiveRecord::Base
  acts_as_paranoid

  # ...
end
```

now calling `delete` or `destroy` will now set `deleted_at` column

suppose you want to delete the entry from the database also then you need to call

```ruby
really_destroy!
```

this will hard delete(delete record from table)

**Discard**:-

It is an `ActiveRecord mixin` to add flagging for records that are discarded

**installation:-**

```ruby
gem 'discard'
```

then

```ruby
bundle install
```

**Usage**:-

```ruby
class Article < ActiveRecord::Base
  include Discard::Model
end
```

then you can either generate a migration like:

```ruby
rails generate migration add_discarded_at_to_articles discarded_at:datetime:index
```

or create manually

```ruby
class AddDiscardToArticles < ActiveRecord::Migration[5.0]
  def change
    add_column :posts, :discarded_at, :datetime
    add_index :posts, :discarded_at
  end
end
```

then you need to make a change in the controller

just replace `destroy` with `discard` 

```ruby
def destroy
  @article.discard
  redirect_to article_url, notice: "Article removed"
end
```

> For New Projects you can go with `Discard` instead of using `paranoia`!!!!
