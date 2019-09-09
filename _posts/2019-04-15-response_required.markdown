---
layout: post
title:      "Response Required"
date:       2019-04-15 16:29:50 -0400
permalink:  response_required
---


I was working on a side project, trying to convert one of my current favorite board games, [Deep Space D-6](https://boardgamegeek.com/boardgame/183571/deep-space-d-6), into some sort of computerized version. The tricky part (well, one of the MANY tricky parts) is the enemy cards. Some do things on activation, some when they enter play, some when they are destroyed, and several other triggers. Triggering those methods was easy enough, but what if they did nothing? I could write an empty method each time:

```
def on_entry
  #do nothing
end

def on_destruction
  #do nothing
end

def on_activation
  player.deal_damage(2)
end
```

But that would get laborious and is not the most efficient for space and clarity. Enter, the [`respond_to?`](https://ruby-doc.org/core-2.6.2/Object.html#method-i-respond_to-3F) method. It essentially tests if the object has a corrosponding method (using a string or symbol), and returns true if it does.

```
"Hello World".respond_to?("length")
=> true

"Hello World".respond_to?("on_entry")
=> false
```

Then, it was an easy for each time a card object was played, it just checked if it had an on_entry method, and if so, ran it!

```
class Game
  def self.play_card(card)
    if card.respond_to?("on_entry")
      card.on_entry
    end
  end
	
  def self.destroy_card(card)
    if card.respond_to?("on_destruction")
      card.on_destruction
    end
  end
end
```

It has a default second parameter of "true," which also searches through private and protected methods. If you want to exclude those, just toss in a "false": `"Hello World".respond_to?("length", false)`

