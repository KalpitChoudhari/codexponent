---
layout: post
title:  "Understanding alias_method in Ruby on Rails"
author: Kalpit
categories: [ Ruby, Rails ]
image: assets/images/alias.png
---

In order to create an alias for a method we can use alias_method which is provided by Ruby. Basically, we create an alias in order to override the name without changing the original implementation of it.

Let’s see the syntax of alias_method:

alias_method(new_name, old_name)


In this example, new_method_name is the name of the new method that you want to create, and existing_method_name is the name of the existing method that you want to alias.

```
Example
class User
    def name
      puts 'Luca Brasi'
    end

    alias_method :full_name, :name
end
```

If we execute above code, we will see, if we print User.new.name or User.new.full_name it will return same output as Luca Brasi.

Another Example
Let's say you have a User model with a method called full_name that returns the user's first and last name. Now, let's say you want to create an alias for full_name called name. Here's how you would do it:

```
class User < ApplicationRecord
  def full_name
    "#{first_name} #{last_name}"
  end

  alias_method :name, :full_name
end
```

Now, you can use name instead of full_name:

```
user = User.new(first_name: "John", last_name: "Doe")
puts user.name # "John Doe"
```

Conclusion: 

alias_method is a powerful tool that can help you write more maintainable and flexible code in your Ruby on Rails applications. By creating aliases for existing methods, you can rename methods without affecting their functionality, override methods without losing their original implementation, and more.

Hopefully, this introduction to alias_method has given you a better understanding of how it works and how you can use it in your own Rails applications.

