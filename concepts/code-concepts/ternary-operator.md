https://www.rubyguides.com/2019/10/ruby-ternary-operator/

A ternary operator is made of three parts, that’s where the word “ternary” comes from. These parts include a conditional statement & two possible outcomes.

In other words, a ternary gives you a way to write a compact if/else expression in just one line of code.

Here is a traditional if/else;

```ruby
if apple_stock > 1
  :eat_apple
else
  :buy_apple
end
```

Here is the ternary operator;

```ruby
apple_stock > 1 ? :eat_apple : :buy_apple
```

Here is the wiki which contains a list of which languages use a ternary operator; https://en.wikipedia.org/wiki/Ternary_conditional_operator

