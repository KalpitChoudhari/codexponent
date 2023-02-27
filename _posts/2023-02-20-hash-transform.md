---
layout: post
title:  "Transform hash value(s) in Ruby"
author: Kalpit
categories: [ Ruby, Rails ]
image: assets/images/hash.jpg
---
In order to transform or change hash values in ruby, generally we use `map` or `each_with_obejct` or `inject`

If you’re using Ruby 2.4+, we can use a method called `transform_values` instead of map or any other methods.

In this blog post, we will explore examples of using the `map`, `each_with_object`, `inject`, and `transform_values` methods to change hash values.

There are two main ways to change hash values: directly modifying the existing hash, or returning a new hash with transformed values without modifying the original.

Example 1: Return multiple of 2 of each value in the hash

```ruby
hash = { one: 1, two: 2, three: 3, four: 4, five: 5 }

# map
hash.map { |key, value| [key, value * 2] }.to_h

# each_wth_object
transformed_hash = hash.each_with_object({}) do |(key, value), new_hash|
	new_hash[key] = value * 2
end

# inject
hash.inject({}) do |new_hash, (key, value)|
	new_hash[key] = value * 2
	new_hash
end

# transform_values
hash.transform_values { |value| value * 2 }

# If we want to change the hash value, use `!` with transform_values
hash.transform_values! { |value| value * 2 }
```

Let's consider an example where the user wants to capitalize the first letter of a string in a hash. There are multiple ways to achieve this, as shown below.

Example 2: Return capitalize the value

```ruby
hash = {
	first_name: 'luca',
	last_name: 'brasi',
	test: 3
}

# map
hash = hash.map { |key, value| value.capitalize }

# each_with_object
hash.each_with_object({}) { |(key, value), hash| hash[key] = value.capitalize }

# inject
hash = hash.inject({}) do |new_hash, (key, value)|
	new_hash[key] = value.capitalize
	new_hash
end

# transform_values
hash = hash.transform_values { |value| value.capitalize }

# If we want to change the hash value, use `!` with transform_values
hash.transform_values! { |value| value.capitalize }

```
