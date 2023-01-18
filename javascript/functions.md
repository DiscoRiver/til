Standard declaration;

```js
function plus1(x) {
    return x + 1;
}
```

Functions are values that can be assigned to vars;

```js
let square = function(x) {
    return x * x;
}

square(plus1(y))
```

in `ECMAScript 6` there is a shorthand syntax for defining functions. The syntax uses `=>` to separate arguments from fuction body, and are known as _arrow functions_. They are mostly used when you want to pass an unnamed function as an argument to another function. Previous code would look like this with arrow functions;

```js
const plus1 = x => x + 1;
const square = x => x * x;
plus1(y)
square(plus1(y))
```

Using functions with objects results in methods;

```js
let a = [];
a.push(1,2,3);
a.reverse();
```

Custom methods;

```js
points = [
    {x: 0, y: 0},
    {x: 1, y: 1}
]

points.dist = function(){
    let p1 = this[0];
    let p2 = this[1];
    let a = p2.x-p1.x;
    let b = p2.y-p1.y;
    return Math.sqrt(a*a + b*b);
}

points.dist()
```