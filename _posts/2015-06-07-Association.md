---
title: Associations
layout: post
date: 2015-06-07
---

In Rails, an assoication is a connection between two Active Record models.  Associations are implemented using macro style calls which are basically methods calls in the context of the class object.  
<br> There are six types:

+ has_one
+ has_many
+ has_many :through
+ has_one :through
+ has_and_belongs_to_many


<br>Say we want to create an association between 'Tasks' and 'Users', on the tasks side we have to delare: 

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
We also have to be sure to include the user foreign key column in tasks table, otherwise we get a 'no such column' message.

<h4>Polymorphic Assocations</h4>
In the case where we need to make a class belong to two classes or more, we use a polymorphic has_many association.
Lets say we two classes called video and music and we want to ratings to them. 

{% highlight ruby %}
class Rating < ActiveRecord::Base 
  belongs to :rateable, polymorphic: true 
end
{% endhighlight %}

Using 'able' at the end of the class name is a rails convention that works wonderfully because rateable accurately describes the the interface of objects that will be associated in this way.  

{% highlight ruby %}
class Videos < ActiveRecord::Base   
  has_many :ratings, as: :rateable 
end

class Music < ActiveRecord::Base 
  has_many :ratings, as: :rateable 
end 
{% endhighlight %}

The as: marks the association as polymorphic and specifies which interface we are using on the other side of the association. 
The other side of a polymorphic belongs_to association can also be a has_one.

Our Ratings table would look like this:

{% highlight ruby %}
class CreateRatings < ActiveRecord::Migration 
  def change 
    create_table :ratings do |t|
      t.integer :rating 
      t.integer :rateable
      t.string :rateable_type 
    end 
  end 

//We can also use a one line shortcut:
  t.references :rateable, polymorphic: true 
{% endhighlight %}




