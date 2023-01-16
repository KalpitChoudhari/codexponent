---
layout: post
title:  "Member Vs Collection Routes in Rails"
author: Kalpit
categories: [ Rails ]
featured: true
image: assets/images/routes.jpeg
absolute_url: member-vs-collection-routes-in-rails
---
To understand the difference between collection and member routes, first let’s take an example. Suppose we are having 3 models called User, Post, and Comment.

Some of the routes could be as follows (based on models):

```ruby

# Show user
/users/:id

# Search user
/users/search

# Show posts that are uploaded by the user
/users/:id/posts

# Show all the comment(s) by user
/users/:id/comments

# Show comment(s) to post which are uploaded by the user
/users/:id/posts/:id/comments
```

By looking at above routes, we can say that some of them are collection routes and some are member. So, what are member and collection routes?

### Member Routes:

As it’s name suggests, member routes are member of it’s parent resource. We need to provide resource specifier (id) in order to access member route(s).

Defining member routes:

```ruby
# Example 1.1
resources :users do
	get 'posts', on: :member # Create routes as /users/:id/posts
	get 'comments', on: :member # Create routes as /users/:id/comments
end
```

If we have more than one member route within the same resource, then simply we can define member route as follow

```ruby
# Exmaple 1.2
resources :users do
	member do
		get 'posts' # Create routes as /users/:id/posts
		get 'comments' # Create routes as /users/:id/comments
	end
end
```

Note that both the above code (1.1 and 1.2) will generate same path and will call to same action.

> Output for **rails routes —expanded**

```ruby
--[ Route 1 ]-------------------------------------------------------------------------------------------------------------------------------------------------
Prefix            | posts_user
Verb              | GET
URI               | /users/:id/posts(.:format)
Controller#Action | users#posts
--[ Route 2 ]-------------------------------------------------------------------------------------------------------------------------------------------------
Prefix            | comments_user
Verb              | GET
URI               | /users/:id/comments(.:format)
Controller#Action | users#comments
--[ Route 3 ]-------------------------------------------------------------------------------------------------------------------------------------------------
Prefix            | users
Verb              | GET
URI               | /users(.:format)
Controller#Action | users#index
...
...
...
```

### Collection Rotes:

In collection routes, call to action does not depend on resource. i.e. it does not depend on its parent id. In collection routes we don’t need to pass an id in order to access or call the action.

Defining Collection Routes:

```ruby
# Exmaple 2.1
resources :users, only: [:edit, :update, :show, :destory] do
	get 'search', on: :collection # Create routes as /users/search
	post 'create', on: :collection # Create routes as /users/create
end
```

Just like member routes, if we are having more than one collection route, than we can define them as follows:

```ruby
# Example 2.2
resources :users, only: [:edit, :update, :show, :destory] do
	collection do
		get 'search' # Create routes as /users/search
		post 'create' # Create routes as /users/create
	end
end
```

Note that both the above code (2.1 and 2.2) will generate same path and will call to same action.

> Output for **rails routes —expanded**

```ruby
--[ Route 1 ]-------------------------------------------------------------------------------------------------------------------------------------------------
Prefix            | search_users
Verb              | GET
URI               | /users/search(.:format)
Controller#Action | users#search
--[ Route 2 ]-------------------------------------------------------------------------------------------------------------------------------------------------
Prefix            | users
Verb              | POST
URI               | /users/create(.:format)
Controller#Action | users#create
--[ Route 3 ]-------------------------------------------------------------------------------------------------------------------------------------------------
Prefix            | edit_user
Verb              | GET
URI               | /users/:id/edit(.:format)
Controller#Action | users#edit
--[ Route 4 ]-------------------------------------------------------------------------------------------------------------------------------------------------
Prefix            | user
Verb              | GET
URI               | /users/:id(.:format)
Controller#Action | users#show
...
...
...
```

## References:

[Rails Routes](https://guides.rubyonrails.org/routing.html)

[Ruby in Rails](https://www.rubyinrails.com/2019/07/11/rails-routes-member-vs-collection/)
