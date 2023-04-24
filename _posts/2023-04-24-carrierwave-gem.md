---
layout: post
title:  "CarrierWave Gem: The Ultimate File Uploader for Rails"
author: Kalpit
categories: [ Ruby, Rails ]
image: assets/images/carrierwave.png
---
If you're building a Rails application that requires file uploads, you're in luck!

The CarrierWave gem is the ultimate file uploader for Rails. In this blog, we'll learn what CarrierWave is and how to use it in your Rails application.

## What is CarrierWave?

CarrierWave is a Ruby gem that provides a simple and elegant way to upload files from Rails applications. It provides a range of features that make it easy to handle file uploads such as image resizing, file type validation, and storage on cloud services like Amazon S3, Google Cloud Storage, and Microsoft Azure. It's also seamlessly integrated with popular Ruby ORMs like ActiveRecord and Mongoid.

## Installing CarrierWave

The first step in using CarrierWave is installing it. You can do this by adding the following line to your Rails application's `Gemfile`:


```ruby
gem 'carrierwave'
```

After adding the gem to your `Gemfile`, run the following command to install it:

```ruby
bundle install
```

## Using CarrierWave

Once you have CarrierWave installed, you can start using it in your Rails application. The first step is to generate an uploader using the following command:

```ruby
rails generate uploader Image
```

This will create an `image_uploader.rb` file in your `app/uploaders` directory. You can then mount the uploader in your model like this:

```ruby
class User < ApplicationRecord
  mount_uploader :avatar, AvatarUploader
end
```

This will allow you to upload images for the `User` model using the `avatar` attribute.

## Uploading Images

To upload an image using CarrierWave, you can use the `remote_url` attribute in your form. Here's an example:

```ruby
<%= form_for @user do |f| %>
  <%= f.label :avatar %>
  <%= f.file_field :avatar %>
  <%= f.hidden_field :avatar_cache %>
  <%= f.text_field :avatar_remote_url %>
  <%= f.submit %>
<% end %>
```

In the above form, we have added an extra field `avatar_remote_url`, which allows us to upload images by providing a remote URL.

## Conclusion

That's it! In this blog post, we've discussed what CarrierWave is, how to install it, and how to use it to upload files in your Rails application. With CarrierWave, you can easily handle file uploads and storage without worrying about the underlying implementation details. Happy coding!
