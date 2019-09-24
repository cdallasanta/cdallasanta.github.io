---
layout: post
title:      "ActiveAdmin"
date:       2019-09-23 22:30:16 -0400
permalink:  activeadmin
---


My new favorite Ruby gem.

![The default dashboard](http://railscasts.com/static/episodes/asciicasts/E284I03.png)

When setting up my Ruby on Rails project ([Climb On](https://www.climb-on.org]), an online ropes log for challenge courses to track the age of their climbing ropes), the advisor I worked with very casually mentioned [ActiveAdmin](https://github.com/activeadmin/activeadmin), a portal for administrators to manage their users, models, and almost everything. At the time, it was outside of the scope of my project, but I recently have had the absolute joy of diving into this wonderful gem. It already has a wonderful and very robust [documentation page](https://activeadmin.info/), but I am going to go through and pull out a few important snippets relevant to someone new to the tool. There is so much to this gem, and pretty much all of it is customizable to an amazing degree, but I am going to run through just a few things, and as I become more familiar with the gem, will add more sections later.


## Overview

This gem adds a new route to your page: `/admin` that AdminUsers (or any user model you want) can use to sign in and manage everything. It comes with its own generated views and CSS, so it is ready to use right out of the box! You can import your own stylesheets and each of its pages if you want to give it its own look, but I haven't dove into that pandora's box yet.

![A default page](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/03/1394716826books_default_view.jpg)

Beautiful.


## Installation

In your gemfile, add `gem 'activeadmin'`. From there, run the following commands:

```ruby
bundle install
rails generate active_admin:install
rake db:migrate
rake db:seed
rails server
```

You can then visit `localhost:3000/admin`, and login with User: admin@example.com and Password: password, and you're there! If you want to attach it to a user model of your choice (instead of it's default AdminUser), you can rails `generate active_admin:resource MODEL_NAME`. 


### Resources

Resource is what ActiveAdmin calls their connection to your model. To connect a model to the gem, run `rails g active_admin:resource MODEL_NAME`. This will create a file in `app/admin/`, where you can customize how the gem utilizes the resource.


## The [Index page](https://activeadmin.info/3-index-pages.html)

AcitveAdmin utilized CRUD and REST routes, so you can perform all CRUD actions with this interface. The index route, (e.g. localhost:3000/admin/posts) lets you view all posts. From there, you can view, edit, create, and delete posts as you'd like.


### Filters

You can create filters with the filter box on the right with very simple commands. For example, if your post model has the attributes "title", "body", belongs to an `author`, and has many `tags`, you could set up a filter box with the following:

```ruby
# app/admin/posts.rb
ActiveAdmin.register Post do
  filter :title   # automatically creates a textbox search for "contains:"
  filter :body, label: "Content"  # same as above, but the users will see it labeled "Content" instead
  filter :author  # adds a dropdown menu to let you select from authors
  filter :tags, as: :check_boxes, collection: proc { Tag.all } #  forces AA to use checkboxes instead of the dropdown it would have guessed
end
```

And there it appears on the right! It creates so much for you automatically, but still allows an amazing amount of customization.


### Scope

Let's say you don't want all of the users to see all of the posts. Only superusers can view them all, everyone else can only view their own. You can do that too!

```ruby
# app/admin/posts.rb
ActiveAdmin.register Post do
  scope_to :current_user, unless: proc{ current_user.admin? }
end
```

`scope_to` can receive a block, or a custome association method with `association_method: "METHOD_NAME` if you want to have a more customized association.




## Wrap-up (for now)

To an extent, I am just rehashing what is all available in the documentation, but there is SO MUCH in there, and it's very well written. If you want to get it up and running quickly though, this blog post can get you started playing with the tools and options available. One of my favorite things about coding in general, and this tool absolutely fits, is if you look at the page and think, "I wonder if it can do X," the answer is "yeah, probably." Just staring at the index page has given me inspiration for different ways my users can interact with the models, and in every case, there was a way I could customize the gem to make it do that.

So, dear readers, give it a shot! Stare in awe at how easy it was to make such a beautiful interface, and let it inspire you to play with the gem's guts!
