---
layout: post
title:      "I Can See Clearly Now"
date:       2019-06-21 00:32:26 +0000
permalink:  i_can_see_clearly_now
---

As I approached my rails project, I had this fantastic idea that I was very excited about ([Climb On](https://github.com/cdallasanta/climb-on-rails), a web app for challenge course managers). As it came time to code though, I had *no* idea where to start. What models do I need? What associations do they need? What do I want users to be able to do, precisely? And a bigger question, what is in the scope of this project? The more I thought about it, the bigger the project became, and it was spiralling a bit out of control. This post from the programmer humor [subreddit](https://www.reddit.com/r/ProgrammerHumor/) was starting to speak to me a little too strongly.

![](https://i.imgur.com/lz6hIPfm.jpg)

By chance, I discovered a wonderful invention that has been around somewhat longer than even Fortran: paper.


### Break it down

It all started really coming together when I was able to sit down during a slow time at work and get this down:

![](https://i.imgur.com/KIITOYGl.jpg)

For your ease of reading, it is a list of what could eventually be written as several tests. I want facilitators (the lowest level of permission) to be able to: view/edit their details, create pre-use inspections for element, etc. Leads (the next level of permission), should be able to: create periodic inspections, and admins can do it all and more. What's more, I started writing down the associations for the models, which led me to the next step.

### ~~Be~~ Write the ~~change~~ migrations you want to see in the world

Being a spatially minded person myself, I continued putting things down where I could see them. There are several great database designers out there. I just googled "schema create online free" and picked the first one that came up, in this case: [dbdesigner](https://www.dbdesigner.net/). I stuck a screenshot of this in my /db folder, and had it open next to me as I wrote each migration. It was invaluable.

![](https://i.imgur.com/223pbkml.png)

I spent a lot of time tweaking many things in here before I even started coding. From here, I discovered that I needed to learn about [polymorphic associations](https://guides.rubyonrails.org/association_basics.html#polymorphic-associations) for my comments model, I restructred my preuse/setup/takedown associations several times, and generally got a better grasp of what I wanted my views to do. The database has shifted around quite a bit since then (with the addition of not one, but *three* new join tables), but with how interrelated the models, migrations, controllers, and views are, this got me started with getting things down.

### Untangling the web

Trying to figure out what I wanted the views to do started to feel like another gordian knot. I needed to figure where I wanted to route users based on their permissions, give extra options if they were a lead or admin at various points, and the interaction between the setup and takedown inspections was daunting. Again, I turned to the paper.

![](https://i.imgur.com/IR964UTl.jpg)

Handwriting out the views made it vastly easier to see how I wanted the user directed through the routes, and I even jotted some of the routes down alongside the views, to help me track how it will be set up. Even this step caused me to go back and rewrite some migrations, since I better understood the information I would need to present to the user,

## To no one’s great surprise

Who knew that such a primitive tool could be so invaluable to such a modern process? Well, everyone likely, but it’s nice to have a reminder every once in a while that those tools exist. And if, like me, you find yourself in [Howard Gardner’s](https://en.wikipedia.org/wiki/Theory_of_multiple_intelligences) visual-spatial bucket, this may save you a lot of time and frustration down the road.

Happy coding, y’all!



