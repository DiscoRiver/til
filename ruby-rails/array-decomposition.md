You can decompose an Array during assignment using parenthesis:

```
(a, b) = [1, 2]
p a: a, b: b 
# prints {:a=>1, :b=>2}
```

You can decompose an Array as part of a larger multiple assignment:

```
a, (b, c) = 1, [2, 3]
p a: a, b: b, c: c # prints {:a=>1, :b=>2, :c=>3}
```

Since each decomposition is considered its own multiple assignment you can use * to gather arguments in the decomposition:

```
a, (b, *c), *d = 1, [2, 3, 4], 5, 6

p a: a, b: b, c: c, d: d
# prints {:a=>1, :b=>2, :c=>[3, 4], :d=>[5, 6]}
```