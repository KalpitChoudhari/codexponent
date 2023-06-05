---
layout: post
title:  "Rails 7 excluding on ActiveRecord::Relation"
author: Mayuresh
categories: [ Ruby, Rails ]
image: assets/images/rails-7.png
---

Rails 7 introduced a new method **`excluding`** for **`ActiveRecord::Relation`**, offering a convenient way to exclude specific records from a collection. This addition provides developers with a cleaner and more expressive approach when filtering query results.

## **The Previous Approach: `where.not`**

Prior to Rails 7, developers relied on the **`where.not`** method to exclude records from a query result. The **`where.not`** method was introduced in Rails 4.0 and allowed for excluding records based on specific conditions. Here's an example of how it was used:

```ruby
results = Model.where.not(attribute: value)

```

In the above code snippet, **`Model`** represents the ActiveRecord model being queried, **`attribute`** refers to the attribute used for filtering, and **`value`** is the value to exclude.

## **The Introduction of `excluding` in Rails 7**

With the advent of Rails 7, the new **`excluding`** method makes filtering records even more intuitive. Let's explore how it simplifies the exclusion process.

### **Excluding a Single Record**

Suppose we have a **`Post`** model and we want to exclude the last created post from a collection. We can achieve this using **`excluding`** in the following way:

```ruby
post = Post.last
result = Post.all.excluding(post)

```

In the above code, we retrieve the last post using **`Post.last`**. Then, by invoking **`Post.all.excluding(post)`**, we retrieve all the posts except the one specified by **`post`**.

### **Excluding Records Based on Association**

We can also leverage **`excluding`** to exclude records based on their association with another model. Let's consider a scenario where we have a **`User`** model that has many posts. To retrieve all posts except those belonging to the last user, we can use **`excluding`** as follows:

```ruby
user = User.last
result = Post.all.excluding(user.posts)

```

In the code above, we retrieve the last user with **`User.last`**, and then we exclude the posts associated with that user by invoking **`Post.all.excluding(user.posts)`**.

## **Conclusion**

The introduction of the **`excluding`** method in Rails 7 enhances the filtering capabilities of **`ActiveRecord::Relation`**. It simplifies the process of excluding specific records, whether based on a single attribute or through associations with other models. With **`excluding`**, developers can write more readable and concise code when working with collections in Rails applications.

To delve deeper into the implementation details and discussions around the introduction of **`excluding`**, you can explore the corresponding pull request **[here](https://github.com/rails/rails/pull/41439)**.
