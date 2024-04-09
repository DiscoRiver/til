Link: https://dl.acm.org/doi/pdf/10.5555/1074100.1074433

Essentialy, guarded commands are statements "guarded" by a conditional, for example;

```go
if n > 0 {
    // do something
}
```

In the above link, an interesting comment is this;


> If all of the guards in a given guarded command set are disjoint (that is, if no more than one guard is true at any given time) then the selection of s is welldefined despite the unspecified and random nature of guard evaluation and selection. If, however, the guards are not disjoint, with the possibility that more than one may be true simultaneously, then selection of s is not well defined (and indeed may be different from one execution of the program to the next)

With this in mind, and referring specifically to non-concurrent statements, I wonder if using this logic would serve as a good design across the board, in terms of only having one of the conditions true at any given time.

For example, is this bad code, or could it serve a useful purpose in certain situations?

```go
if n < 0 {
    // do something
} else if y > 0 {
    // do something else
}
```

What if both statements are `true`? does that present a potentially issue in our application? 


