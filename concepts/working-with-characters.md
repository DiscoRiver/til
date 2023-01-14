When working with characters, it's good to know a few things first.

Characters (for the purposes of this, we're considering those in the latin script) are represented as unicode code points. For example charactes A-Z (uppercase) occupies the code points 65-90, and a-z (lowercase) are  97-122.

This means we can treat text as a series of numbers. In golang, a `rune` type represents a single unicode code point. We can range over strings to extract this code point;

```go
text := "Hello World!"
for _, c := range text {
    fmt.Printf("Code point: %d\n", c)
}
```

Your inclination might be to try and convert to a slice/list, but there's no need.

In the case of Advent of Code 2022, there was a need to convert the code points to incrementing numbers beginning from one, so by treating characters as numbers, and because each alphabet (lowercase/uppercase) is in sequence, we can do this easily;

```go
// Incrementing ints for lowercase and uppsercase alphabets, starting at 1
func charToInt(c rune) int {
	if int(c) > 90 {
		// lower case
		return int(c) - 96 // a == unicode(97) --> a == 1
	} else {
		// upper case
		return int(c) - 38 // A == unicode(65) --> A == 27
	}
}
```

Because they increment, we can just subtract the different to get the value we want.

