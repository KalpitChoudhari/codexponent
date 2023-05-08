---
layout: post
title:  "Whenever - Scheduling Jobs"
author: Kalpit
categories: [ Ruby, Rails ]
image: assets/images/whenever.svg
---

If you are developing web applications in Ruby on Rails, then you are probably already familiar with the concept of scheduling jobs to run at specific intervals. One popular tool for scheduling jobs in Rails is the Whenever gem. In this blog post, we will explore the features of the Whenever gem and demonstrate how to use it with a simple example.

What is the Whenever gem?

The Whenever gem is a Ruby gem that provides a simple and intuitive syntax for defining cron jobs. Cron jobs are recurring tasks that run at specified intervals on a server. For example, you might schedule a cron job to run a database backup every night at midnight.

How to use the Whenever gem

To use the Whenever gem in your Rails application, you first need to add it to your Gemfile:

```ruby
gem 'whenever', require: false
```

Once you have added the gem to your Gemfile, you need to run **`bundle install`** to install it.

Next, you need to generate the default configuration file for Whenever by running the following command:

```ruby
wheneverize .
```

This command will create a **`config/schedule.rb`** file in your Rails application, which is where you will define your cron jobs.

Let's look at a simple example of how to define a cron job using the Whenever gem. Suppose you want to schedule a task to run every day at noon to send an email to all users who have registered within the past 24 hours.

```ruby
every 1.day, at: '12:00 pm' do
  runner "UserMailer.send_new_user_emails"
end
```

In this example, we are using the **`every`** method to define a task that runs every day at noon. We are specifying the time using the **`at`** option, which takes a string in the format "HH:MM am/pm". We are also using the **`runner`** method to specify that we want to run a Ruby script that sends the new user emails using the **`UserMailer`** class.

Once you have defined your cron jobs in the **`config/schedule.rb`** file, you need to generate the actual cron tab by running the following command:

```ruby
whenever --update-crontab
```

This command will generate the cron tab based on the contents of your **`config/schedule.rb`** file and install it on your server.

To get list of all the Cron jobs, we can execute the following command in our terminal:

```ruby
crontab -l
```

Conclusion

The Whenever gem is a powerful and easy-to-use tool for scheduling cron jobs in your Rails application. It provides a simple and intuitive syntax for defining cron jobs directly within your application, making it easy to manage your scheduling tasks. With the Whenever gem, you can easily schedule recurring tasks such as database backups, email notifications, and more.
