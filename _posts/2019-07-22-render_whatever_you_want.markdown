---
layout: post
title:      "Render whatever you want!"
date:       2019-07-22 22:01:53 +0000
permalink:  render_whatever_you_want
---


When working with my API, I wanted to render an alert when something went wrong, and one when something went correctly. Rendering the error was easy enough by using `render partial:`
```
render partial: 'application/errors', locals: {object: @inspection, alert_type: "alert-danger", message: "Error:"}, status: 422
```
In my errors partial, I could pass in the inspection which had failed to save, a class "alert-danger" (for bootstrap purposes), and the status code that told my AJAX that the saving had failed, thus diverting to `.fail()` and putting the error into the DOM.

The problems started when I wanted to render my successful object AND a partial. In the example above, I only sent back the rendered partial, in which the errors were listed out. This eventually led me to 2 discoveries with Activemodel Serializer:

## 1 - You can use AMS to render whatever you want!
Again, using my inspections model as an example, with the attributes of `id` and `complete?`.
```
class InspectionSerializer < ActiveModel::Serializer
  attributes :id, :complete?, :arbitrary_message
  
  def arbitrary_message
   "Hello!"
  end
end
```

This would then give us the following JSON when it's rendered:
```
{
  id: 1,
  complete?: true,
  arbitrary_message: "Hello!"
}
```

Even though my model doesn't have an `arbitrary_message` attribute, I was still able to send one! From there, I could render my partial within in the serializer:

```
class InspectionSerializer < ActiveModel::Serializer
  attributes :id, :complete?, :alert
	
  def alert
    ApplicationController.new.render_to_string(partial: 'application/errors', locals: {object: object, alert_type: "alert-success"})
  end
end
```

Now my AJAX call call access `response["alert"]` and receive the html for my success message! But, there was one more issue. What if I didn't want to render an alert? Or I wanted to customize what the message in the alert said? Enter:

## 2 - @instance_options
`@instance_options` can be called within an AMS class and access variables sent to it from the initial render call. For example, in my controller, I rendered the inspection with the following:
```
render json: @inspection, message: "Inspection updated successfully", status: 201
```

In the serializer, I can access `@instance_options[:message]`, and it will be whatever the string I passed in is! Now my serializer looks like the following:

```
class InspectionSerializer < ActiveModel::Serializer
  attributes :id, :complete?, :alert
	
  def alert
    if @instance_options[:message]
      ApplicationController.new.render_to_string(partial: 'application/errors', locals: {object: object, alert_type: "alert-success", message: @instance_options[:message]})
    end
  end
```
