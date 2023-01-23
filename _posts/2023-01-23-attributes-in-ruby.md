---
layout: post
title:  "Attribute reader, writer and accessor in Ruby"
author: Kalpit
categories: [ Ruby, Rails ]
image: assets/images/undraw_online_reading_np7n.png
absolute_url: attributes-in-ruby
---
Oftentimes, we need to use instance variables outside of the class in ruby code. We know that instance variables can be accessed by methods. And to access these methods, we create objects. To expose these things to the outside world we need objects.

Let’s consider we have a class called `User` having `name` as its property/attribute:

```ruby
class User
  def initialize(name)
    @name = name
  end
end

user = User.new('Diago')
user.name     # Throws NoMethodError
```

```ruby
attr_usage.rb:8:in '<main>': undefined method 'name' for #<User:0x000000013e9ffb70 @name="Diago"> (NoMethodError)
```

If we inspect above error and print all the methods of user object, then we got the following array:

```ruby
[:taint, :tainted?, :untaint, :untrust, :untrusted?, :trust, :methods,
 :singleton_methods, :protected_methods, :private_methods, :public_methods,
 :instance_variables, :instance_variable_get, :instance_variable_set,
 :instance_variable_defined?, :remove_instance_variable, :instance_of?,
 :kind_of?, :is_a?, :method, :public_method, :public_send, :singleton_method,
 :define_singleton_method, :extend, :clone, :to_enum, :enum_for, :<=>, :===,
 :=~, :!~, :nil?, :eql?, :respond_to?, :freeze, :inspect, :object_id, :send,
 :to_s, :display, :frozen?, :class, :then, :tap, :yield_self, :hash,
 :singleton_class, :dup, :itself, :!, :==, :!=, :__id__, :equal?, :instance_eval,
 :instance_exec, :__send__]    #  Output of user.methods
```

Here, we can't see any initializer or accessor to instance variable i.e. getter or setter.

To overcome this, let’s create getter and setter methods in the above code.

```ruby
class User
  def initialize(input_name)
    @name = input_name
  end

  # Returns the value of the instance variable name
  def name 
    @name
  end

  # Act as setter method, which set the value of instance variable name
  def name=(input_name)
    @name = input_name
  end
end

user = User.new('Diago')
p user.name      # returns "Diago"

user.name = 'New Name'
p user.name      # returns "New Name"
```

We can see getter and setter method for `name` attribute now.

If we have multiple instance variables and need to access them outside of the class, we need to write multiple getters and setters for each of them. `attr_accessor` comes in picture now.

### Attribute accessor :

> attr_accessor: attribute_name

If we want to get value as well as set the value of the instance variable then we can simply use as follows:

```ruby
class User
  attr_accessor :name

  def initialize(input_name)
    @name = input_name
  end
end

user = User.new('Diago')
p user.name      # returns "Diago"

user.name = 'New Name'
p user.name      # returns "New Name"
```

And if we print `user.methods` then we can see additional methods as:

```ruby
[:name=, :name, :taint, :tainted?, ....,
	....,
	....]
# Note that getter and setter added to methods of the user object
```

### Attribute reader :

> attr_reader: attribute_name

As we saw in earlier exmaple, `attr_reader` can change value. So we can't use it.

Let's say if we want to read the property and not to change it's value then we should be using `attr_reader` as follows:

```ruby
class User
  attr_reader :full_name

  def initialize(first_name, last_name = '')
    @full_name = first_name + ' ' + last_name
  end
end

user = User.new('Michael', 'Corleone')
p user.full_name
```

In the above example, we can't do `user.full_name = ‘Luca Brasi’`, as it will throw an error as:
```ruby
attr_usage.rb:11:in '<main>': undefined method 'full_name=' for #<User:0x000000015218f620 @full_name="Michael Corleone"> (NoMethodError)
```

It’s because there is no setter method created for attribute_reader.

### Attribute writer :

> attr_writer: attribute_name

Consider a scenario where we just need to set the instance variable and not read/get it.
We can’t use `attr_reader` here because we can't set value in the `attr_reader`. We can use `attr_writer` in this case as follows:

```ruby
class User
  attr_writer :full_name

  def initialize(first_name, last_name = '')
    @full_name = first_name + ' ' + last_name
  end

  def get_full_name
    @full_name
  end
end

user = User.new('Michael', 'Corleone')
p user.get_full_name      # Returns "Michael Corleone"

user.full_name = "Luca Brasi"
p user.get_full_name	 # Returns "Luca Brasi"
```
If we use `user.full_name`, then we will get an error.

To access `full_name` we have created a new method called `get_full_name`.


Now that we have covered `attr_reader`, `attr_writer` and `attr_accessor`, use them as per your requirement and convenience. 🥳
