---
layout: post
title:  "ActiveRecord Import: Using `new` vs Hash in Bulk Record Creation"
author: Kalpit
categories: [ Ruby, Rails ]
image: assets/images/activerecord_import.jpeg
---

## Introduction:
When creating a large number of records in Rails using ActiveRecord's `create!` method, calling a database query for each record can be inefficient. However, there are alternative approaches that can significantly improve performance. In this article, we'll explore the options available when creating records in bulk and compare the usage of `new`and `hash` with ActiveRecord Import.

### The Problem:

Creating records in bulk can result in numerous database calls, impacting the overall performance. We need a more efficient way to handle large data sets without compromising speed.

### The Motivation:

Trying reduce database calls 😅

### The Approach:

Let's consider the example of creating 50,000 records in the **`Book`** model. We'll compare different approaches to achieve this and measure their performance using the Benchmark module.

```ruby
require 'benchmark'

bruteforce_approach = Benchmark.measure {
  50_000.times do |i|
    Book.create!(name: "book #{i}")
  end
}
p "Total time required using bruteforce approcah [in seconds]: #{bruteforce_approach.total}"

# Now, the thing is above query would take around 20 to 22 seconds to create 50k books (Based on Benchmark)
```

Other way could be preparing all 50,000 data and insert that data at once rather than firing new query for each book creation using a gem called ActiveRecord’s Import as follows:

```ruby
require 'benchmark'

new_res = Benchmark.measure {
  books = 50_000.times.map do |i|
    Book.new(name: "Book #{i}")
  end
  Book.import! books
}
p "Total time required using new keyword [in seconds]: #{new_res.total}"
```

Above query would take only 4 to 5 seconds to create 50k book records. As we can see, from 22 seconds to 5 seconds (Great Progress!) 🪄

Let’s try to reduce this time by using hash instead of instantiating Book object as follows:

```ruby
require 'benchmark'

hash_res = Benchmark.measure {
  books = 50_000.times.map do |i|
    { name: "Book #{i}" }
  end
  Book.import! books
}

p "Total time required using hash approcah [in seconds]: #{hash_res.total}"
```

Above query would take around 1.5 to 2 seconds to create 50k books 💫

### Conclusion:
In this article, we explored different approaches for bulk record creation in Rails. We compared the brute force approach with ActiveRecord Import using **`new`** and hash-based techniques. The use of ActiveRecord Import significantly improved performance by reducing the number of database calls. Moreover, using hashes proved to be the fastest method for bulk record creation.

In the upcoming blog series, we will delve deeper into the activerecord-import gem, exploring its options and addressing collision handling when creating records in bulk. Stay tuned for more insights and optimizations!

By implementing these strategies, you can drastically improve the efficiency and performance of bulk record creation in Rails.
