---
layout: post
title:      "ActiveAdmin"
date:       2019-09-23 22:30:16 -0400
permalink:  activeadmin
---


My new favorite Ruby gem.

![Beautiful](https://www.google.com/url?sa=i&source=imgres&cd=&cad=rja&uact=8&ved=2ahUKEwjVo7LrtOjkAhXKHzQIHdMsAvEQjRx6BAgBEAQ&url=http%3A%2F%2Frailscasts.com%2Fepisodes%2F284-active-admin%3Fview%3Dasciicast&psig=AOvVaw2gNawNz56tRLvENNv-zBP1&ust=1569378554233534)

When setting up my Ruby on Rails project ([Climb On](https://www.climb-on.org]), an online ropes log for challenge courses to track the age of their climbing ropes), the advisor I worked with very casually mentioned [ActiveAdmin](https://github.com/activeadmin/activeadmin), a portal for administrators to manage their users, models, and almost everything. At the time, it was outside of the scope of my project, but I recently have had the absolute joy of diving into this wonderful gem. It already has a wonderful and very robust [documentation page](https://activeadmin.info/), but I am going to go through and pull out a few important snippets relevant to someone new to the tool.


## Overview

This gem adds a new route to your page: `/admin` that AdminUsers (or any user model you want) can use to sign in and manage everything. It comes with its own generated views and CSS, so it is ready to use right out of the box! You can import your own stylesheets and each of its pages if you want to give it its own look, but I haven't dove into that pandora's box yet.

![A default page](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/03/1394716826books_default_view.jpg)

## Installation

In your gemfile, add `gem 'activeadmin'`. From there, run the following commands:

```ruby
rails generate active_admin:install
rake db:migrate
rails server
```

You can then visit `localhost:3000/admin`, and login with User: admin@example.com and Password: password, and you're there! If you want to attach it to a user model of your choice (instead of it's default AdminUser), you can rails `generate active_admin:resource MODEL_NAME`
