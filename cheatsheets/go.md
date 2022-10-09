### **Resources**

https://gobyexample.com/

### **Basics**

By convention, the package name is the same as the last element of the import path. For instance, the "math/rand" package comprises files that begin with the statement package rand.

In Go, a name is exported if it begins with a capital letter. For example, Pizza is an exported name, as is Pi, which is exported from the math package.

When importing a package, you can refer only to its exported names. Any "unexported" names are not accessible from outside the package.

Shortened `(x int, y int)` is `(x, y int)`

Functions can return multiple arguments

A return statement without arguments returns the named return values. This is known as a "naked" return. Use only for short functions.

The var statement declares a list of variables; as in function argument lists, the type is last. Variables can be defined at package level or function level.

If you have initializers, the type can be omitted

Inside a function, the := short assignment statement can be used in place of a var declaration with implicit type.

Variables declared without an explicit initial value are given their *zero value*.

The expression T(v) converts the value v to the type T: `y := float(x)`

Constants are declared like variables, but with the const keyword.

### **Flow Controls**

While loop: 

```jsx
for sum < 100`or forever: for {}
```

Like for, the if statement can start with a short statement to execute before the condition.

```jsx
if v := math.Pow(x, n); v < lim {
		return v
	}
```

```jsx
switch os := runtime.GOOS; os {
	case "darwin":
		fmt.Println("OS X.")
	case "linux":
		fmt.Println("Linux.")
	default:
		// freebsd, openbsd,
		// plan9, windows...
		fmt.Printf("%s.\n", os)
	}
```

 

A defer statement defers the execution of a function until the surrounding function returns.

The deferred call's arguments are evaluated immediately, but the function call is not executed until the surrounding function returns.

Deferred function calls are pushed onto a stack. When a function returns, its deferred calls are executed in last-in-first-out order.

### Types

You can set the value of varible through dereferencing: 

```jsx
i := 42
p = &i
*p = 2
```

Struct: 

```jsx
type Vertex struct {
	X int
	Y int
}

func main() {
  v1 = Vertex{1, 2}  // has type Vertex
	v2 = Vertex{X: 1}  // Y:0 is implicit
	v3 = Vertex{}      // X:0 and Y:0
	p  = &Vertex{1, 2} // has type *Vertex
}
```

Arrays: 

```jsx
func main() {
	var a [2]string
	a[0] = "Hello"
	a[1] = "World"
	fmt.Println(a[0], a[1])
	fmt.Println(a)

	primes := [6]int{2, 3, 5, 7, 11, 13}
	fmt.Println(primes)
}
```

Arrays cannot be resized. A slice, on the other hand, is a dynamically-sized

slice : A slice does not store any data, it just describes a section of an underlying array.

```jsx
func main() {
	primes := [6]int{2, 3, 5, 7, 11, 13}

	var s []int = primes[1:4] //Includes element 1 but not 4
	fmt.Println(s)
}
```

The length of a slice is the number of elements it contains.

The capacity of a slice is the number of elements in the underlying array, counting from the first element in the slice.

The length and capacity of a slice `s` can be obtained using the expressions `len(s)` and `cap(s)`.

When you slice a slice, its capacity changes 

```jsx
b := make([]int, 0, 5) // len(b)=0, cap(b)=5

b = b[:cap(b)] // len(b)=5, cap(b)=5
b = b[1:]      // len(b)=4, cap(b)=4
```

`append` adds elements to a slice 

When ranging over a slice, two values are returned for each iteration. The first is the index, and the second is a copy of the element at that index.

When ranging over a slice, two values are returned for each iteration. The first is the index, and the second is a copy of the element at that index.`

```jsx
for i, v := range pow {
fmt.Printf("2**%d = %d\n", i, v)
}
```

You can skip the index or value by assigning to `_`
.

Maps: 

```jsx

var m map[string]Vertex

func main() {
	m = make(map[string]Vertex)
	m["Bell Labs"] = Vertex{
		40.68433, -74.39967,
	}
	fmt.Println(m["Bell Labs"])
}

```

Delete an element:

```
delete(m, key)
```

Test that a key is present with a two-value assignment:

```
elem, ok = m[key]
```

Functions Values: 

```jsx
func compute(fn func(float64, float64) float64) float64 {
	return fn(3, 4)
}

func main() {
	hypot := func(x, y float64) float64 {
		return math.Sqrt(x*x + y*y)
	}
	fmt.Println(hypot(5, 12))

	fmt.Println(compute(hypot))
	fmt.Println(compute(math.Pow))
}
```

Go functions may be closures. A closure is a function value that references variables from outside its body. The function may access and assign to the referenced variables; in this sense the function is "bound" to the variables.

It is like a function who has it’s own variable. 

### Methods & Interfaces

```jsx
type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func main() {
	v := Vertex{3, 4}
	fmt.Println(v.Abs())
}
```

You can only declare a method with a receiver whose type is defined in the same package as the method. You cannot declare a method with a receiver whose type is defined in another package (which includes the built-in types such as `int`
).

Methods with pointer receivers can modify the value to which the receiver points (as `Scale`
 does here). Since methods often need to modify their receiver, pointer receivers are more common than value receivers.

Go passes by value and not by reference. So if you want to change the something pass a pointer . while methods with pointer receivers take either a value or a pointer as the receiver when they are called. That is, as a convenience, Go interprets the statement `v.Scale(5)`
 as `(&v).Scale(5)` since the `Scale` method has a pointer receiver. The reverse also works: while methods with value receivers take either a value or a pointer as the receiver when they are called:

The first is so that the method can modify the value that its receiver points to.

The second is to avoid copying the value on each method call. This can be more efficient if the receiver is a large struct, for example.

In general, all methods on a given type should have either value or pointer receivers, but not a mixture of both

An *interface type* is defined as a set of method signatures.

A value of interface type can hold any value that implements those methods.

If the concrete value inside the interface itself is nil, the method will be called with a nil receiver.

In some languages this would trigger a null pointer exception, but in Go it is common to write methods that gracefully handle being called with a nil receiver (as with the method `M` in this example.) Note that an interface value that holds a nil concrete value is itself non-nil.

An empty interface may hold values of any type. (Every type implements at least zero methods.)

`interface{}`

To *test* whether an interface value holds a specific type, a type assertion can return two values: the underlying value and a boolean value that reports whether the assertion succeeded.

```
t, ok := i.(T)
```

One of the most ubiquitous interfaces is `[Stringer](https://go.dev/pkg/fmt/#Stringer)` defined by the `[fmt](https://go.dev/pkg/fmt/)` package.

```jsx
type Stringer interface {
    String() string
}

```

The `error` type is a built-in interface similar to `fmt.Stringer`

```jsx
type error interface {
    Error() string
}
```

The `io` package specifies the `io.Reader` interface, which represents the read end of a stream of data.

`func (T) Read(b []byte) (n int, err error)`

### **Generics**

`func Index[T comparable](s []T, x T) int`

This declaration means that `s` is a slice of any type `T` that fulfills the built-in constraint `comparable`

`comparable` is a useful constraint that makes it possible to use the `==` and `!=`
 operators on values of the type

### Concurrency

`go f(x, y, z)`

starts a new goroutine running f. The evaluation of f, x, y, and z happens in the current goroutine and the execution of f happens in the new goroutine.

Channels are a typed conduit through which you can send and receive values with the channel operator, `<-`
.

```jsx
ch <- v    // Send v to channel ch.
v := <-ch  // Receive from ch, and
           // assign value to v.
```

`ch := make(chan int)`

By default, sends and receives block until the other side is ready. This allows goroutines to synchronize without explicit locks or condition variables.

Channels can be *buffered*. Provide the buffer length as the second argument to `make`
 to initialize a buffered channel:

``ch := make(chan int, 100)``

Sends to a buffered channel block only when the buffer is full. Receives block when the buffer is empty.

A sender can `close` a channel to indicate that no more values will be sent. Receivers can test whether a channel has been closed by assigning a second parameter to the receive expression: after

`v, ok := <-ch`

`ok` is `false` if there are no more values to receive and the channel is closed.

The loop `for i := range c` receives values from the channel repeatedly until it is closed.

**Note:** Only the sender should close a channel, never the receiver. Sending on a closed channel will cause a panic.

**Another note:** Channels aren't like files; you don't usually need to close them. Closing is only necessary when the receiver must be told there are no more values coming, such as to terminate a `range` loop.

The `select` statement lets a goroutine wait on multiple communication operations.

A `select` blocks until one of its cases can run, then it executes that case. It chooses one at random if multiple are ready.

```jsx
func fibonacci(c, quit chan int) {
	x, y := 0, 1
	for {
		select {
		case c <- x:
			x, y = y, x+y
		case <-quit:
			fmt.Println("quit")
			return
		}
	}
}

func main() {
	c := make(chan int)
	quit := make(chan int)
	go func() {
		for i := 0; i < 10; i++ {
			fmt.Println(<-c)
		}
		quit <- 0
	}()
	fibonacci(c, quit)
}
```

The `default` case in a `select` is run if no other case is ready.

Use a `default` case to try a send or receive without blocking:

```jsx
func main() {
	tick := time.Tick(100 * time.Millisecond)
	boom := time.After(500 * time.Millisecond)
	for {
		select {
		case <-tick:
			fmt.Println("tick.")
		case <-boom:
			fmt.Println("BOOM!")
			return
		default:
			fmt.Println("    .")
			time.Sleep(10 * time.Millisecond)
		}
	}
}
```

Go's standard library provides mutual exclusion with `[sync.Mutex](https://go.dev/pkg/sync/#Mutex)` and its two methods:

- `Lock`
- `Unlock`

We can define a block of code to be executed in mutual exclusion by surrounding it with a call to `Lock` and `Unlock` as shown on the `Inc` method.

We can also use `defer` to ensure the mutex will be unlocked as in the `Value` method.

## Building/Running
- Install the current package: `go install`
- Add missing modules and remove unused: `go mod tidy`

## Error Handling
- use x.Must(function) to panic if function returngs a non nil error
