---
layout: post
title:      "Linked Lists and Ruby"
date:       2019-10-14 17:40:25 +0000
permalink:  linked_lists_and_ruby
---


"Why not just use an array?"


As I practiced and studied for technical interviews, I found myself asking this question quite a bit. Ruby does not natively have Linked Lists (or Binary Trees for that matter, but that might be a later blog post). Still, coding problems ask about them, and it's a useful data structure with some advantages over arrays. (it's worth noting that this post is mostly a summary of [this](https://www.rubyguides.com/2017/08/ruby-linked-list/) amazing post from [rubyguides.com](https://www.rubyguides.com)).


## Big O

Looking at Big O notation and time complexities, arrays aren't always the most efficient data structure. See the [Big O cheet sheet](https://www.bigocheatsheet.com/) for a nice demonstration on the advantages of other methods. Arrays are good for reading data, but bot the best when you need to do manipulations. Linked Lists on the other hand, are great for manipulation. In an array, when you insert/delete an element from the middle, all of the remaining elements are updated with their new position. Each element knows its index from the start, so all following elements need that changed. Thus, the Array has a time complexity of `O(n)` when inserting and deleting. In a Linked List however, each element (or node) does not know its index, it only knows what the next node is (or in the case of a Doubly Linked List, what comes after and what came before). When you insert/delete a node, the only thing that needs updating is the preceeding node. Just point it to it's new destination, which gives us a time complexity of `O(1)`. Much nicer.


## Implimentation in Ruby

To use this data structure in Ruby, we need to create our own class. Let's create a LinkedList class with a nested ListNode class for each node:

```
class LinkedList
  attr_accessor :head, :tail

  def initialize(val)
    self.head = ListNode.new(val)
    self.tail = self.head
  end

  class ListNode
    attr_accessor :val, :next

    def initialize(val)
      self.val = val
      self.next = nil
    end
  end
end
```

This particular implimentation needs to be initialized with a starting value for it's first node (the `head`, which is useful when starting a search), and also has access to its `tail`, which is useful for adding elements on to the end. While we're at it, let's add those methods now.

```
class LinkedList
...
  def add(val)
    self.tail.next = ListNode.new(val)
    self.tail = self.tail.next
  end

  def find(item)
    current = self.head
    until current.next == nil
      if current.val == item
        return current
      end

      current = current.next
    end
    return nil
  end
	...
end
```

We can now initialize a LinkedList and add some values thusly:
```
list = LinkedList.new(1)
list.add(2)
list.add(3)
list.add(4)

list => [1, 2, 3, 4]    # note that it's not an acutal array though
```

Let's create an `#insert_after` method.

```ruby
def insert_after(index, val)
  # if provided a ListNode as the index, such as after a #find call, go straight there
  if index.class == LinkedList::ListNode
    currentNode = index
  # otherwise advance through the list until the indexed node is found
  elsif index.class == Integer
    i = 0
    currentNode = self.head
    until i == index do
      currentNode = currentNode.next
      i += 1
    end
  end
		
  temp = currentNode.next
  newNode = ListNode.new(val)
  currentNode.next = newNode
  newNode.next = temp
	return self
end
```


There we go! There is a lot more funcionality to Ruby's Arrays than what we have here, but I recommend creating a LinkedList class yourself so that you can better understand the concept behind the data structure.
		
