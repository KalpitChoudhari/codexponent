---
layout: post
title:  "Prosopite: A Comprehensive Solution for Detecting N+1 Queries in Rails"
author: Kalpit
categories: [ Rails ]
image: assets/images/prosopite.png
---
As the size of the application increases, there may be latency from the backend. One possibility is that we are experiencing the N+1 query problem in our backend queries. In this blog post, we will discuss how to detect the N + 1 queries.

There are many gems in marketplace, and we are going to discuss about `Prosopite` and how it is different from `Bullet`.


While both `Prosopite` and `Bullet` are gems used to detect N + 1 queries in Rails, they differ in their approach. `Bullet` is a gem that works by analyzing your code and pointing out potential N + 1 queries during runtime. On the other hand, `Prosopite` is a runtime analysis tool that works by analyzing SQL queries made to the database during runtime. This means that `Prosopite` is able to detect N + 1 queries that `Bullet` might not be able to catch, and is generally considered to be a more comprehensive solution.

## How to configure Prosopite in existing application:

1. Add `gem prosopite` in your gemfile and then execute `bundle install`  OR
2. `gem install prosopite` to use prosopite directly.

Under `config/environments/development.rb`, use the following configuration:

```ruby
config.after_initialize do
	Prosopite.rails_logger = true        # Send warnings to the Rails log
	Prosopite.prosopite_logger = true    # Send warnings to log/prosopite.log
end
```

Since we are using prosopite in our development environment, we have added its configuration in `environments/development.rb`.

After starting the development server with the `rails server` command, Prosopite will log any N+1 queries in your application to the `log/prosopite.log` file.

In summary, `Prosopite` is a comprehensive solution for detecting N+1 queries in Rails applications. While there are other gems available in the marketplace, `Prosopite` offers a unique approach to detecting these queries by analyzing SQL queries made during runtime. By configuring and implementing `Prosopite` in your application, you can optimize performance and improve the overall user experience.

Here are some references to the blog post:

- [Prosopite on Github](https://github.com/twopoint718/prosopite)
- [Bullet on Github](https://github.com/flyerhzm/bullet)

Hope this helps!
