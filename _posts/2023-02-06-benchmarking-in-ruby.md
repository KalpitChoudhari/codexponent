---
layout: post
title:  "Benchmark module in Rails"
author: Mayuresh
categories: [ Rails, Ruby ]
image: assets/images/benchmark.png
---

Benchmark is a module provided by ruby which helps you to measure the time taken to execute

a particular method or line in ruby code

benchmark reports the time taken to execute the ruby code

How to install:- 

> gem 'benchmark'


and then 
> bundle
 or install it as 

> gem install benchmark


How to use:-

suppose you’re doing a ton of queries and you need to figure out what is faster in retrieving the record ‘which’ or ‘find’

for that purpose, you can benchmark it

let’s consider you have one method which uses ‘find’

```ruby
def meth
  a = User.find(1)
end
```


and another method which uses ‘where’

```ruby
def meth2
  a = User.where(id: 1)
end
```

syntax to benchmark a method is:-

```ruby
Benchmark.bm do |x|
 x.report { method_name }
end
```

 the result will look like 

result for meth(find)

```ruby
Benchmark.bm do |x|
 x.report { meth }
end
       user     system      total        real
  0.050131   0.009857   0.059988 (  0.121509)
```

and result for meth2(where)

```ruby
Benchmark.bm do |x|
 x.report { meth2 }
end
       user     system      total        real
   0.000335   0.000457   0.000792 (  0.000963)
```

and suppose you have to benchmark a single line of code

```ruby
puts Benchmark.measure { [1,2,3,4,5].map{|digit| [digit, digit]}.to_h }
    user     system      total        real
0.000007   0.000005   0.000012 (  0.000005)
```

With help of this, we can figure out which method is faster

as you can see the output which comes after the benchmark is in different rows

user: the amount of time spent executing userspace code(code which we are running)
system: the amount of time spent on executing operating system's function

`total: user + real`

real: system + user + time spent waiting for I/O, network, disk, user input, etc also known as `wallclock time`
