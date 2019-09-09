---
layout: post
title:      ".send() Me an Angel"
date:       2019-09-09 17:59:23 +0000
permalink:  send_me_an_angel
---


(best read while listening to Scorpion's [Send Me an Angel](https://www.youtube.com/watch?v=1UUYjd2rjsE))

I have a friend was going through Flatiron School's program a ways behind me, and she occasionally reaches out for help. Both of us have experience as teachers, so it is a fun exercise to practice explaining conepts to someone new. Doing so helps us both understand the concepts better (following one of my favorite Latin proverbs: Docendo discimus. Through teaching, we learn). She recently asked about a tricky method: `.send()`. Looking up the [documentation](https://apidock.com/ruby/Object/send) on that method, it was no wonder she was confused. It is not written kindly to someone new to the field. So, if you are struggling with this method, hopefully this can clear some things up for you.

## What it basically does:

At the basic level, it takes whatever string or symbol you pass in as an argument, then calls it on the object `.send()` is called on as if you had called that method. For example, let's say we have a `Dog` class that can `.bark()`:

```ruby
class Dog
	def self.bark
		"Woof!"
	end
	
	def self.speak(phrase)
		"Dog says #{phrase}"
	end
end
```

Now, we can call #bark a few ways:
```ruby
Dog.bark
=> "Woof!"

Dog.send("bark")
=> "Woof!"

Dog.speak("I love you!")
=> "Dog says I love you!"

Dog.send("speak", "I love you!")
=> "Dog says I love you!"
```

You can see in that last example that the first argument is the method name that will be called, and any following arguments are passed in as arguments to the called method. Now, why would you ever do this instead of calling the method directly? Great question!

## Why?

There are several use cases for this, and we'll go over 2: user input, and initialization.

### User Input

Let's say that we want to ask the user through a cli what method they would like to call.
```ruby
puts "Please select a method"
selection = gets.chomp
```
Now, we could write a switch statement that covers any possible method they could call, but that might get ENOURMOUS quickly if you have a lot of options. Enter: `.send()`
```ruby
puts "Please select a method"
selection = gets.chomp
Dog.send(selection)
```
We could not say `Dog.selection`, since there is no #selection method for Dog. Even if it did go the next step and become `Dog."bark"`, you would get a syntax error for calling a string on an object. Using #send lets you dynamically call a method (and in conjunction with [responds_to?](http://chrisdallasanta.com/response_required), could cover most of your bases)

### Initialization

This is a much more likely place to use #send. Let's say you have the following:
```ruby
attrs = {
	name: "Obi",
	breed: "Shi-tzu",
	temperment: "barks a lot"
}

Dog.create(attrs)
```

Now, in your initialize, you *could* do the following:
```ruby
def initialize(attrs)
	self.name = attrs.name
	self.breed = attrs.breed
	self.temperment = attrs.temperment
end
```
BUT, what if you don't know what attrs are going to be passed in? What if you have a model that has 100 attributes? That will get annoying fast. You can instead do the following:
```ruby
def initialize(attrs)
	attrs.each do |key, value|
		self.send("#{key}=", value)
	end
end
```

That's a lot, so let's unpack it.

- For each key in attr, we'll do something. So for the first pass, key = "name", value = "Obi"
- Inside self.send, we have "#{key}=". Because of string interpolation, it comes out to: `self.send("name=", "Obi")`
- The method is saying, "Hey, self, do you have a method called "name="? If so, please call it with the argument "Obi", or, `self.name= "Obi"`"

We could _not_ do the following:
```ruby
def initialize(attrs)
	attrs.each do |key, value|
		self.key = value
	end
end
```

Because, like above, `self.key` is not a method name, and even `self."name"` (which it would evaluate to) is also not a method, because of it being a string. So send lets us turn that string "name=" into a method call: self.name=

## Wrap up
A lot of frameworks take care of this for you, such as ActiveRecord. I personally haven't used it since we moved on to ActiveRecord and Sinatra in the curriculum, but it's good to know it's there though, and how/why it works. 
