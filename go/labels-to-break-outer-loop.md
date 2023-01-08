Sometimes it might be necessary to break out of a nested statement without, and this can be done using a label. 

Relevant Go doc for break statements: https://go.dev/ref/spec#Break_statements

```go
OuterLoop:
	for i = 0; i < n; i++ {
		for j = 0; j < m; j++ {
			switch a[i][j] {
			case nil:
				state = Error
				break OuterLoop
			case item:
				state = Found
				break OuterLoop
			}
		}
	}
```

This is ideal for complex actions that need to jump to an outer loop.