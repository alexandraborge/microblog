## Truthiness

Truthiness is the concept that every object is true except for false and nil. This means that you can use any element in a condition without using it in a boolean expression.


## Example

We can use this array of hashes

```ruby
favorites = [{fruit: ‘banana’, vegetable: ‘brocolli’}, {fruit: ‘orange’, vegetable: ‘celery’}]
```
To find the hash within the array that has a fruit that is "banana"

```ruby
banana_lover = favorites.find { |f| f[:fruit] == ‘banana’ }

=> banana_lover = {fruit: ‘banana’, vegetable: ‘broccoli’}
```

`banana_lover` evaluates to true in this case because it is neither false nor nil

But if we defined a new variable 

```ruby
apple_lover = favorites.find { |f| f[:fruit] == ‘apple’ }

=> apple_lover = nil
```

`apple_lover` evaluates to false because it is nil


You can use these in conditional statements such as this:

```ruby
If banana_lover
  ‘I love bananas’
elsif apple_lover
  ‘I love apples’
end
```

`banana_lover` is defined as the hash {fruit: ‘banana’, vegetable: ‘broccoli’} but it is also truthy and evaluates to true.

`apple_lover` is nil, it is falsey and evaluates to false.
