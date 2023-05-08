---
layout: post
title:  "Delegate in Ruby on Rails"
author: Mayuresh
categories: [ AWS ]
image: assets/images/delegate.jpeg
---

In Ruby on Rails, delegates provide a way to delegate certain methods to an associated object. This is useful when you have a model with an association and want to expose some of the associated object's methods through the main object.

## Example

Let's say we have two models, `User` and `Profile`. The `User` model has one `Profile`, and the `Profile` model has a `name` attribute. We want to be able to access the `name` attribute of a user's profile directly from the `User` model. This is where delegates come in.

To set up a delegate, we need to define the method we want to delegate in the model where it's defined. In our case, we want to delegate the `name` method from the `Profile` model to the `User` model. We do this using the `delegate` method in the `User` model:

```
class User < ApplicationRecord
  has_one :profile
  delegate :name, to: :profile
end

```

The `delegate` method takes two arguments: the name of the method to delegate (`:name` in our case), and the name of the object to delegate to (`:profile` in our case). This tells Rails to delegate the `name` method to the `profile` object associated with the `User` model.

Now, we can access the `name` attribute of a user's profile directly from the `User` model:

```
u = User.first
u.profile.name # returns the name of the user's profile

```

We can also use the `name` method directly on the `User` model:

```
u = User.first
u.name # also returns the name of the user's profile

```

Under the hood, Rails creates a method on the `User` model called `name` that calls the `name` method on the associated `profile` object.

Delegates can also be used with collections. Let's say we have a `Post` model that has many `Comments`, and we want to delegate the `name` method from each comment to the associated `User` model. We can do this using the `delegate` method with the `:to` option and the `:prefix` option:

```
class Post < ApplicationRecord
  has_many :comments
  delegate :name, to: :user, prefix: true, allow_nil: true, prefix: true
end

class Comment < ApplicationRecord
  belongs_to :post
  belongs_to :user
end

class User < ApplicationRecord
  has_many :comments
end

```

The `prefix` option adds a prefix to the delegated method name, based on the name of the object being delegated to. In our case, the delegated method will be called `user_name`.

Now, we can access the `name` attribute of a comment's user directly from the `Comment` model:

```
c = Comment.first
c.user_name # returns the name of the comment's user

```

Delegates are a powerful tool in Ruby on Rails that can help simplify your code and make it more readable. By delegating methods to associated objects, you can avoid cluttering your code with unnecessary method calls and keep your models lean and focused.
