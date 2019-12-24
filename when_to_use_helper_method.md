## When to Use a Helper Method

Helper methods are used in the views and are made to be reusable.

Below are some reasons why you might use a helper method.

## When you have a lot of repeat ruby code in your views or other methods

We want to avoid repeating the same piece of code in general. For larger blocks of code, we may use a partial but other times a helper method will do the trick.

Lets looks at an example where we have to display a cats name based on some criteria.

```erb
<div class='adult-cats'>
  <% adult_cats.each do |cat| %>
    <% if cat.personality == 'friendly' && ['medium', 'large'].include?(cat.size) && cat.health == 10 %>
      <%= cat.name %>
    <% end %>
  <% end %>
</div>

<div class='kittens'>
  <% kittens.each do |cat| %>
    <% if cat.personality == 'friendly' && ['medium', 'large'].include?(cat.size) && cat.health == 10 %>
      <%= cat.name %>
    <% end %>
  <% end %>
</div>
```

To clean this up we can move the shared code to a helper method.

```ruby
def cat_name(cat)
  return unless cat.personality == 'friendly' &&
    ['medium', 'large'].include?(cat.size) &&
    cat.health == 10

  cat.name
end
```

Then apply it to the view

```erb
<div class='adult-cats'>
  <% adult_cats.each do |cat| %>
    <%= cat_name(cat) %>
  <% end %>
</div>

<div class='kittens'>
  <% kittens.each do |cat| %>
    <%= cat_name(cat) %>
  <% end %>
</div>
```

## When you have a lot of conditional logic in your views

We want to try to keep the view file as clean and uncluttered as possible. This means that if you have conditional logic that can be moved into a helper method, it is usually best to do so.

Instead of having this in your view file

```erb
  <% if cat.age < 2 %>
     <div>Age Group: Young Cat</div>
  <% elsif cat.age < 4 %>
    <div>Age Group: Teenage Cat</div>
  <% else %>
    <div>Age Group: Old Cat</div>
  <% end %>
```

You can add this logic to a helper method and call the helper method in the view. This would look like this

```ruby
def age_group(cat)
  label = if cat.age < 2
            'Young'
          elsif cat.age < 4
            'Teenage'
          else
            'Old'
          end
  "#{label} Cat"
end
```

```erb
Age Group: <%= age_group(cat) %>
```

We turned 7 lines into one line in the view which makes it much cleaner and easier to read.

## What if you need to use a method in both your controller and your views?

In this example we want to define a method that will check if the cat has not be adopted yet. This method will be used in the controller but also the view. Right now we have to redefine it in the view in order to use it which means we are writing extra code.

**Controller**

```ruby
def show
  @all_cats = Cat.all
  @available_cats = @all_cats.select { |cat| available?(cat) }
end

def available?(cat)
  !cat.adopted
end
```

**View**
```erb
<% @available_cats.each do |cat| %>
  <%= cat.picture %>
<% end %>

<div class='cats'>
  <% @all_cats.each do %>
    <div><%= cat.name %> <%= 'available' unless cat.adopted %></div>
  <% end %>
</div>

```

Instead of redefining the `adopted?` method in the helper file you can define it in the controller like above and then use a rails method called `helper_method` which allows you to use that method the same you would a helper method https://apidock.com/rails/ActionController/Helpers/ClassMethods/helper_method. You can also pass multiple methods into it as arguments.

You would call `helper_method` like this in the controller:
```ruby
helper_method(:adopted?)
```

then in the view:
```erb
<div class='cats'>
  <% @all_cats.each do %>
    <div><%= cat.name %> <%= 'available' unless adopted?(cat) %></div>
  <% end %>
</div>
```

