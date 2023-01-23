---
layout: post
title:  "Dup use case explained"
author: Mayuresh
categories: [ Ruby, Rails ]
image: assets/images/dup.png
---
Let’s consider a scenario where you want to make some changes in value but also needs original value for another computation.

If we change the value then original value also gets affected. As shown below:

```ruby
class Abc
  attr_reader :word, :params

  def initialize(params)
    @word = params
    @params = params
  end

  def change_word
    @word.concat('-added')
  end

  def print_word
    puts @word
  end

  def print_params
    puts @params
  end
end
```

```ruby
a = Abc.new('hey there')

a.print_word
hey there

a.print_params
hey there

a.change_word
 => "hey there-added"

a.print_params
hey there-added

a.print_word
hey there-added
```

As you can see, the original params are changed when the variable is modified through the parameter.

Ruby copies the slot object_id during method invocations to the parameters

```ruby
a.params.object_id
=> 667980
a.word.object_id
=> 667980
```

Since the new variable points to the same page slot, any modifications you do to this object is also done on the original variable

Ruby's managed heap consists of pages and each page consists of slots of 40 bytes.
The slots are used when objects are allocated
let's say you are assigning a value to a variable
`var = 'hello'`
a free slot is found and the value is stored in that slot and then the slot is passed

To overcome above problem, we can use ruby’s `dup` method as follows:

```ruby
class Abc
  attr_reader :word, :params

  def initialize(params)
    @word = params.dup    # All magic happens here
    @params = params
  end

  def change_word
    @word.concat('-added')
  end

  def print_word
    puts @word
  end

  def print_params
    puts @params
  end
end
```

```ruby
a = Abc.new('hey there')

a.print_word
=> hey there

a.print_params
=> hey there

a.change_word
=> "hey there-added"

a.print_params
=> hey there

a.print_word
=> hey there-added

```

And when we check their object id they are different which means they allocated different slots.

```ruby
a.params.object_id
=> 532140
a.word.object_id
=> 540740
```

Now you can use a new one without touching the existing one. (That’s what she said 😎) 🎉
