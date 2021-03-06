---
layout: post
title:      "ActiveRecord Validations"
date:       2019-05-06 15:34:02 -0400
permalink:  activerecord_validations
---


[Boy did I discover a gold mine here](https://guides.rubyonrails.org/active_record_validations.html).

This lists all of the validations that AR can do behind the scenes for us. We were taught about `validates_presence_of` in the Flatiron curriculum, but there is so, so much more in there.

Validating format in case you need the input to only be an integer or to not include spaces:

```
validates :email, format: { with: /\A\S+\z/,    message: "no spaces allowed" }
```

Validating uniqueness in the table to have unique usernames:

```
validates :username, uniqueness: true
```

And you can set a custom error message for each of these, with different amounts of dynamacy:

```
# checks the resence of the email attribute, with a default message
validates :email, presence: { message: "you must supply an email" }`

# check the format of the age attribute, with a slightly customized message
validates :age, format: { with: /\A[^\D]+\z/, message: "%{value} is not a valid age" }`

# checks to make sure the username isn't used already, with a very customized message
validates :username,
  uniqueness: {
    message: ->(object, data) do
      "Hey #{object.name}!, #{data[:value]} is taken already!
    end
  }
```

In that last example, `object` is the object the validation is running on (in this case, the instance of User), and data is a hash with the following data: `{ model: "User", attribute: "username", value: <whatever they put in> }`.

All of these messages can then be access by your flash messages, leading to some very informative error messages! Enjoy!
