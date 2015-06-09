---
title: Building a todo app like habitrpg with rails pt I
layout: post
date: 2015-06-06
---


So there's this really cool todo app called [habitrpg](http://www.habitrpg.com) It basically treats your life as if it were an rpg game.
You have an avatar that has levels, hp and an experience bar.  If you complete the tasks you assign to your self, then you get 
experience, coins, and level up!!  Conversely if you miss a task, you lose hp and you can die! It's a really cool idea and it 
really motivates you to do stop procrastinating.  The only thing is, real life isn't being controlled by some system and no one can actually if you complete your tasks or not besides yourself lol.  But if you're motivated -like I am - it'll totally work.  

So when my assignment for GoTealeaf was to create my own app, I wanted to make something similar.  Not as robust of course, but at least something with similar UI and a reward system. 

The first problem I ran into was creating the datepicker form.  I encountered two problems.

the form code:
{% highlight ruby %}

<%= form_for task do |f| %>
  <div class='form-group'>
    <%= f.text_field :description, placeholder: 'create a task' %>
  </div>

  <div class='form-group'>
    <%= f.text_field :due_date, class:'due_date', placeholder: 'due date' %>
  </div>
<% end %> 

---jquery---
$('.due_date').datepicker();

{% endhighlight %}

The problem with this code was that there was no unique id assigned to the text_field.  So when I clicked the text field to activate datepicker, it worked...but only for the first task.  I couldn't assign the date to any other tasks on the list.  
So a search on google immediately told me that I was missing a unique id.  

The syntax was a bit tricky for me but I eventually figured it out:
{% highlight ruby %}
  <%= f.text_field :due_date, class:'due_date', id: "'due_date_'#{task.id}", placeholder:'due date', readonly:'readonly' %>
{% endhighlight %}

A check in firebug let me know that I got a unique id.  The readonly attribute makes the text area only editable through datepicker, and not through type.

The last hurdle was the js code.  

With the previous code, the date didn't hit the database upon submit. The correct way to do it was to iterate through all the date forms that were on the page through the class selector 'due_date', and use $('this').datepicker on it:

{% highlight ruby %}
$('.date').each(function(){
  $(this).datepicker({
    dateFormat: 'yy-mm-dd'
  });
});
{% endhighlight %}

The result!!!

![screenshot]({{site.url}}/images/todo_pic.jpg)










