---
layout: post
title:  "Transform hash value(s) in Ruby"
author: Kalpit
categories: [ Ruby, Rails ]
image: assets/images/hash.jpg
---

In order to transform or change hash values in ruby, generally we use map or each_with_obejct or inject

If you’re using Ruby 2.4+, we can use a method called transform_values instead of map or any other methods.

In this blog, we will see examples of map, each_with_object, inject, and transform_values to change hash values.

In order to change the hash value, we can do it in two ways, either by changing the actual hash value or by returning a new hash with transformed values without touching the existing one.

Example 1: Return multiple of 2 of each value in the hash

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


Let’s consider another example where the user wants to uppercase the first letter in a string in the hash. The following are examples to achieve this in multiple ways.

Example 2: Return capitalize the value

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
