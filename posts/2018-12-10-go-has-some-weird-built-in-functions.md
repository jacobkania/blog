---
Title: Go has some weird built-in functions
PublishedDate: 2018-12-10
---
Go is generally an exceptionally simple language, and that's reflected through it's very small number of keywords -- one of the lowest of any modern language ([source](https://github.com/leighmcculloch/keywords)). There are also some built-in functions that I'm sure you use every day -- `append()`, `make()`, and `copy()` are the main ones. You might even use `new()` to create a pointer. But did you know that there are actually over a dozen of these built-in functions, and some of them aren't even guaranteed to be permanent?

# What are all of these built-in functions?

For reference, the Go documentation has the full list [here](https://tip.golang.org/pkg/builtin/), but I'll at least mention them all in this article.

* `len(x)`: You're probably familiar with this one -- it gives you the length of `x`, and handles different types in different ways. In an array or slice, you get the number of elements contained within it. For a string, you get the number of bytes.
* `cap(x)`: This is a less-common one. It gives you the capacity of `x`, which is the same as `len(x)` for an array, but for a slice it gives you the maximum length that the slice can reach before needing to resize. Capacity grows more quickly than length in a slice, but they're equivalent when the slice is first created.
* `delete(m, key)`: This isn't the same as other languages -- since Go is garbage collected, delete isn't used for dereferencing memory. It removes items from a map (`m`) by their key.
* `close(c)`: Closes a channel between goroutines.

There are a few weird ones to do with complex numbers that most of us probably won't use very often:

* `complex(r, i)`: Creates a complex number type from real `r` and imaginary `i` floats. These complex numbers can then have their real and imaginary parts deconstructed with the built-in functions `real(c)` and `imag(c)` respectively.

They may be weird, but these are fully supported by the language without any gotchas. They're useful in certain situations involving complex math (pun intended). Developers doing some specific tasks will find them useful.

# The (possibly) temporary built-ins

There are two of these: `print()` and `println()`. No, these aren't the same as the functions within the `fmt` package, `fmt.Print()` and `fmt.Println()`, these are just base-level built-in functions. You can call them without any imports:

```go
func main() {
	print("Hello, world!")
}
```

This is a perfectly valid go program. [(try it here)](https://play.golang.org/p/iaMuo93hY33)!

But the spec says that these functions aren't guaranteed to remain in Go. They're intended for debugging, not production use. Think of it like a *more elegant way of calling `alert('got here');` in JavaScript.* I think that this feature is really nice to have for situations where you don't have a full IDE running, and you just want to see what a value is somewhere in your code while debugging. But isn't it strange that the official language spec just shrugs its shoulders and says:

>maybe this will continue existing for a while `¯\_(ツ)_/¯`

# Panic and recover

These, although they make sense, I believe work fundamentally differently than a Go program should. One of the huge advantages of the language is that there are no exceptions -- rather, there are errors that you must always make a conscious decision about what to do with them. If you ignore the error, so does your program. But errors won't bubble-up in the same way as most other modern languages, unless they cause something else to fail and return an error in the parent function.

This changes if your code calls the `panic()` function.

```go
func A() {
    println("A begins")
    B()
    println("A ends")
}

func B() {
    println("B begins")
    defer println("B defer called")
    panic("AAAAHHHHHH!!!!")
    println("B ends")
}
```
[(try it here)](https://play.golang.org/p/zv89BmonhAQ)

As soon as you call `panic("message")` within function B, the current function stops immediately and runs its deferred stack, then panics the parent function (A) from that point. The above program will output:

```
A begins
B begins
B defer called
panic: AAAAHHHHHH!!!!
```

This is fundamentally different than how errors are normally handled. Don't get me wrong, there are some perfectly valid reasons for a program to halt execution immediately, clean up after itself, and exit. I just think they're generally very rare, and most people should avoid panicking altogether.

This wouldn't be a problem if there wasn't also a function called `recover()` which, when called from within a deferred function of a panicking stack, will stop the panic sequence and allow the program to resume normal behaviour. I feel like allowing recovery like this encourages bad practices. If something has gone so wrong that you made the (hopefully) highly informed decision to panic, there should probably not be a reason to recover. If there was, you should be passing the error in your return statement like normal.

# Conclusion

There are many more built-in functions than you use everyday. Some are extremely useful, others are a bit more controversial. I think the most important thing is that you at least can understand all of the features of the language that you're using. Software engineering is as much an art as a science, and although you may not always use all of the tools at your disposal, it is important to have them tucked away in the back of your mind for the important situations that call for those tools.