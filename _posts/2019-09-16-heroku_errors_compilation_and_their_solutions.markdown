---
layout: post
title:      "Heroku Errors Compilation (and their solutions)"
date:       2019-09-16 12:53:42 -0400
permalink:  heroku_errors_compilation_and_their_solutions
---


Having deployed several applications to Heroku recently, I have discovered that this process is not always as smooth as one would hope. Fortunately, I find myself hitting the same errors each time (mostly discovered when I google the error and all of the links are purple). This will serve as my little repository for errors I've encountered, what I tried, and what worked, so I don't have to go through ALL of the purple links every time.

Here is how they will be presented:

### Error (or what's wrong)
#### possible cause

Solution, and if available, link to the post that helped me.

Without further ado, here we go!

-------------------------------------------------------------
### What's the command for creating a Heroku app again?
#### Terrible memory

`heroku create <APPNAME>`

From Heroku's own [site](https://devcenter.heroku.com/articles/creating-apps)

-------------------------------------------------------------

### PG::ConnectionBad
#### Postgres is not up and running

Restart (or start in the first place), the postgre server with `sudo service postgresql start`

Thank you [TablePlus](https://tableplus.com/blog/2018/10/how-to-start-stop-restart-postgresql-server.html)

-------------------------------------------------------------

### How to reset a database on heroku?


`heroku restart && heroku pg:reset DATABASE --confirm APP-NAME && heroku run rake db:migrate`
(note the APP-NAME)

Thank you [zulhfreelancer and jgigault](https://gist.github.com/zulhfreelancer/ea140d8ef9292fa9165e)

-------------------------------------------------------------

### Blank Page (but a successful deploy)
#### Scripts are not properly set up

In the top-level package.json, include the following:
```
"scripts": {
    "start": "react-scripts start",
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "cd client && npm install && npm run build && cd ..",
    "deploy": "cp -a client/build/. public/",
    "postinstall": "npm run build && npm run deploy && echo 'Client built!'"
  }
```

I lost the page that gave me this tip, unfortunately.

-------------------------------------------------------------

### Uglifier::Error: Unexpected token: operator (>). To use ES6 syntax, harmony mode must be enabled with Uglifier.new(:harmony => true).
#### Uglifier doesn't like ES6

Change the js_compressor to what's below:
```ruby
# config/environments/production.rb
config.assets.js_compressor = Uglifier.new(harmony: true)
```

-------------------------------------------------------------


### sh: 1: react-scripts: Permission denied heroku
#### I couldn't find the precise cause because I did a lot to try and fix it. Next time I get it, I can narrow this down

What eventually worked:

Add all of the client's dependencies to the top level package.json. E.g.:

```
  "dependencies": {
    "react": "^16.8.6",
    "react-dom": "^16.8.6",
    "react-scripts": "^2.1.8"
  },
```

I didn't have to do this with other projects though, so I don't think that's the perfect solution. Here are the other things I did:


Commands I ran, trying to get the dependencies set up:

1) run `npm install --only=dev && npm install && npm run build` at the top level (I don't think it did anything productive)

2) `heroku run cd client && npm install`

3) `heroku run cd client && npm run build`

4) `npm run build` - nothing changed

5) `cd client && npm run build` - nothing changed?

As a possible resource, I referenced [this](https://stackoverflow.com/questions/41932041/create-react-app-deployment-to-heroku-failed-with-react-scripts-not-found) StackOverflow post a bit.

-------------------------------------------------------------
### heroku yarn executable was not detected in the system
#### This error was in the build log, after - Running: rake assets:precompile

[Install yarn](https://yarnpkg.com/lang/en/docs/install/#windows-stable).

-------------------------------------------------------------
### Heroku not showing the most updated front end
#### Need to build client and serve it from the public folder

In top level, package.json should have:

```
  "scripts": {
    "start": "react-scripts start",
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "cd client && npm install && npm run build && cd ..",
    "deploy": "cp -a client/build/. public/",
    "postinstall": "npm run build && npm run deploy && echo 'Client built!'"
  },
```

Then run `npm run postinstall`
