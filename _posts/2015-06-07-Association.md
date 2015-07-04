---
title: Associations
layout: post
date: 2015-06-07
---

In Rails, an assoication is a connection between two Active Record models.  Associations are implemented using macro style calls which are basically methods calls in the context of the class object.  There are six types:
- belongs_to
- has_one
- has_many
- has_many :through
- has_one :through
- has_and_belongs_to_many

Say we want to create an association between 'Tasks' and 'Users', on the tasks side we have to delare: 

{% highlight ruby %}
class Tasks < ActiveRecord::Base 
  belongs_to :user
end
{% endhighlight %}

And on the user side we do:
{% highlight ruby %}
class Users < ActiveRecord::Base 
  has_many :tasks
end
{% endhighlight %}

Now our user objects can access the tasks attribute like so:
{% highlight ruby %}
user_object.tasks
{% endhighlight %}
And the same is applied to our task objects.  
We also have to be sure to include the user foreign key column in tasks table, otherwise we get a 'no such column' messaged.

<h4>Polymorphic Assocations</h4>
In the case where we need to make a class belong to two classes or more, we use a polymorphic has_many association.


