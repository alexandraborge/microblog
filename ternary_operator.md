## Ruby Ternary Operator

Ternary Operators in are used in place of `if/else` conditional statements. Using them can refactor a five line `if/else` statement into one line.

Ternary Operators thave 3 parts.
- The condition
- Result if the condition evaluates to true
- Result if the condition evaluates to false

## Example

You can turn this:

```ruby
if cats.length > 4
  'Crazy Cat Person'
else
  'Average Cat Enthusiast'
end
```

into this:

```ruby
cats.length > 4 ? 'Crazy Cat Person' : 'Average Cat Enthusiast'
```
