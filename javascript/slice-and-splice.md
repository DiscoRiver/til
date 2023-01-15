TLDR:

- `array.slice(start[,end])` - Does not modify original array, but returns a new array consiting of a portion of the elements.

- `array.splice(start[, deleteCount][, itemN...])` - Used to remove or replace elements in an array. 

## Example Data

```js
let fruits = ['Apple', 'Orange', 'Banana', 'Pear']
```

## Slice
---

Return new array consisting of a range of the original array's elements.

### Syntax

```
array.slice(start[,end])
```

### Examples
- If start is greater than the length of the array, returns an empty array.

```
> fruits.slice(6)
[]
```

- When passing negative index, returns items from end of array.

```
> fruits.slice(-2)
[ 'Banana', 'Pear' ]
```

- When end index is greater than the length of array, extract until arr.length.

```
> fruits.slice(1,5)
[ 'Orange', 'Banana', 'Pear' ]
```

Slice always copies to a new array.

## Splice
---

Modify original array, removing or adding elements.

### Syntax

```
array.splice(start[, deleteCount][, itemN...])
```

### Examples

- Passing only start returns from the index, until it's length.

```
> fruits.splice(2)
[ 'Banana', 'Pear' ]
```

- Passing start and delete returns the array from the start index, and removes the number of elements.

```
> fruits.splice(2,1)
[ 'Banana' ]
```

- Passing start, deleteCount, and item removes the item and replaces it with what we specify. It returns the removed item.

```
> fruits
[ 'Apple', 'Orange', 'Banana', 'Pear' ]
> fruits.splice(2,1,'Lemon')
[ 'Banana' ]
> fruits
[ 'Apple', 'Orange', 'Lemon', 'Pear' ]
```

- If start is out of range, it works like an adding function.

```
> fruits.splice(6,1,'Lemon')
[]
> fruits
[ 'Apple', 'Orange', 'Banana', 'Pear', 'Lemon' ]
```

- We can also use negative indexes.

```
> fruits
[ 'Apple', 'Orange', 'Banana', 'Pear' ]
> fruits.splice(-2,1,'Lemon')
[ 'Banana' ]
> fruits
[ 'Apple', 'Orange', 'Lemon', 'Pear' ]
```