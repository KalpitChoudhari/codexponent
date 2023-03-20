---
layout: post
title:  "Kaminari - Using Pagination in Ruby on Rails application"
author: Kalpit
categories: [ Ruby, Rails ]
image: assets/images/kaminari.png
---
If we have hundreds of thousands of records in our model, then sending or showing them all at once increases the latency of our application. In this case, we need to implement pagination in our application.

Pagination allows showing only a specific number of records at a time rather than all at once.

In Ruby on Rails, we can implement our pagination strategy using limit and offset. However, building your pagination solution can be time-consuming and error-prone, particularly if you're unfamiliar with the underlying concepts and techniques.

Kaminari is a popular pagination gem for Ruby on Rails applications that provides a simple and easy-to-use interface for handling pagination. It comes with a lot of features out-of-the-box, including support for various data sources (such as ActiveRecord), customizable views, and internationalization.

Let’s consider we have `Post` model and we’re having thousands of records in `Post` model

```ruby
# app/controllers/posts_controller.rb

class PostsController < ApplicationController
	def index
		Post.all
	end
end
```

In the above example, whenever we call index action in post controller, we’ll get all records (all thousands of records for post)

Now, we’ll implement our own pagination strategy in posts_controller

```ruby
# app/controllers/posts_controller.rb

class PostsController < ApplicationController
	def index
		page_index = params[:page] || 1
		per_page_count = params[:per_page_count] || 15

		Post.offset(page_index - 1 * per_page_count).limit(per_page_count)
	end
end
```

You can use `limit` to tell the number of records to be fetched and use `offset` to tell the number of records to skip before starting to return the records.

In the above example, we are calculating offset by multiplying the current page index and per page count because, if we are on the second page, we need to find records from 16 to 30 which means page_index = 2 and per_page_count = 15 i.e. 1 * 15 = 15 and the limit is 15 so rails will fetch  15 records (limit is 15) from 16th offset.

The get action for calling the post could be

```ruby
{HOST}/posts?page=1&per_page_count=15

# Above query will return first 15 recods from post
```

Using kaminari for pagination:

Now let’s take same example to understand working of kaminari gem:

```ruby
# app/controllers/posts_controller.rb

class PostsController < ApplicationController
	def index
		page_index = params[:page] || 1
		per_page_count = params[:per_page_count] || 15

		Post.page(page_index).per(per_page_count)
	end
end
```

We don’t need to apply our own logic for offset and limit in case of kaminari. Just need to pass page index and how many records we want for current page i.e. per_page_count.

Now, we’ll look at how one can install kaminari gem:

You need to add `gem kaminari` in your gemfile

Then hit `bundle install` to install the gem.

And start using kaminari fetatures in your application.

Some of the kaminari examples:

```ruby
User.count                     #=> 1000
User.page(1).limit_value       #=> 20
User.page(1).total_pages       #=> 50
User.page(1).current_page      #=> 1
User.page(1).next_page         #=> 2
User.page(2).prev_page         #=> 1
User.page(1).first_page?       #=> true
User.page(50).last_page?       #=> true
User.page(100).out_of_range?   #=> true
```

Kaminari general configuration options:

```ruby
default_per_page      # 25 by default
max_per_page          # nil by default
max_pages             # nil by default
window                # 4 by default
outer_window          # 0 by default
left                  # 0 by default
right                 # 0 by default
page_method_name      # :page by default
param_name            # :page by default
params_on_first_page  # false by default
```

References:

[https://github.com/kaminari/kaminari](https://github.com/kaminari/kaminari)
