Scope resolution operators provide context for operations and values.

In languages such as Python & Go, modules are objects, so there is a special case where `.` is used for object member access.

Typically, however, scope resolution operators are written as `::`. Here is an example snippet of how it works in Ruby;

```ruby
module Example
  Version = 1.0

  class << self # We are accessing the module's singleton class
    def hello(who = "world")
      "Hello #{who}"
    end
  end
end #/Example

Example::hello # => "Hello world"
Example.hello "hacker" # => "Hello hacker"

Example::Version # => 1.0
Example.Version # NoMethodError

# This illustrates the difference between the message (.) operator and the scope operator in Ruby (::)
# We can use both ::hello and .hello, because hello is a part of Example's scope and because Example
# responds to the message hello.
#
# We can't do the same with ::Version and .Version, because Version is within the scope of Example, but
# Example can't respond to the message Version, since there is no method to respond with.
```