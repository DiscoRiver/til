It's important when shifting slices to be mindful of how they work: https://go.dev/blog/slices-intro

When shifting values from the middle of a slice, there are basically four stages to create an ammended array(slice);

1. Add numbers before your insertion (no index change)
2. Insert your new number
3. Insert shifted numbers (index++)
4. Do not cause changes to len or cap

Assuming len and cap are the same in your initial slice, this is the most important line;

```go
shiftedNumbers := make([]int, cap(numbers[i:])-1)
```

The `-1` here ensures the len/cap of the new slice will not grow when appending the values, and that we drop the value that falls outside our range (the lowest number). This is possible because copy() only copies values up the lowest length of src or dst. 

```go
package main

import "fmt"

func main() {
	// This should be largest-->smallest to demonstrate what we want.
	numbers := []int{5, 3, 1}

	fmt.Println("INSERT AND SHIFT")
	fmt.Println("*****************")
	fmt.Println("Initial slice: ", numbers)
	fmt.Println("Initial len/cap: ", len(numbers), cap(numbers))
	fmt.Println("*****************")

	numbersToInsert := []int{6, 4, 2}
	fmt.Println("Inserting numbers in slice: ", numbersToInsert)
	fmt.Println("*****************")

	/*
		Essentially, what this should do is insert a number in the correct position to retain a sorted slice
		from largest-->smallest. I wanted to experiment with "shifting" and dropping values in a slice effectively.

		[3, 2, 1] --> Insert 4 --> [4, 3, 2] --> the lowest digit is dropped.

		This is made simple because copy() will only copy the number of elements equal to the smallest
		length of src or dst, so because we define shiftedNumbers with a length of minus 1, we drop whatever happened
		to be trailing on the slice.
	*/
	for j := range numbersToInsert {
		for i := range numbers {
			// nested but this only needs to execute once
			if numbersToInsert[j] > numbers[i] {
				newSlice := make([]int, 0, cap(numbers))

				/*
				 -1 off cap to account for what we're inserting, otherwise we increase the len and cap of newSlice 
				 when appending, which has exponential results. This has the effect we want, whereby a 
				 number is "lost" if it drops out of the capacity of the initial slice.
				 */
				shiftedNumbers := make([]int, cap(numbers[i:])-1)

				// Store the numbers we need to shift.
				copy(shiftedNumbers, numbers[i:])

				// Add numbers that will not shift.
				newSlice = append(newSlice, numbers[:i]...)

				// Add new number.
				newSlice = append(newSlice, numbersToInsert[j])

				// Add shifted numbers, the index for which is now +1.
				newSlice = append(newSlice, shiftedNumbers...)
				numbers = newSlice

				break
			}
		}
	}

	fmt.Println("Ending Len/Cap: ", len(numbers), cap(numbers))
	fmt.Println(numbers)
}

```