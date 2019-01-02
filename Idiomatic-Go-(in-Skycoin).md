### Formatting

All code should be formatted with [`goimports`](https://godoc.org/golang.org/x/tools/cmd/goimports).  You can configure your text editor to do this automatically on save.  Additionally, the project should have a `Makefile` command called `make format` which formats all of the source with `goimports`.

### Testing

Use the [testify require](https://godoc.org/github.com/stretchr/testify/require) or [testify assert](https://godoc.org/github.com/stretchr/testify/assert) package for assertions.  

Mocking can be done with [testify mock](https://godoc.org/github.com/stretchr/testify/mock).  Use [mockery](https://github.com/vektra/mockery) to autogenerate mocks.

Write [table-driven tests](https://github.com/golang/go/wiki/TableDrivenTests) when appropriate; most tests can be written this way.

### Variable naming

* Camel case is always used. Do not use underscores or all caps
* Unexported names are `lowerCamelCase`
* Exported names are `UpperCamelCase`
* **WRONG** `ALLCAPS` **WRONG**
* **WRONG** `underscore_method` **WRONG**
* Variable names should be meaningful when possible

### Use struct field names when declaring a struct object instance

Given this struct:

```go
type Foo struct {
    Bar int
    Baz string
}
```

Use the struct's field names when instantiating it, and put them each on their own line:

```go
var foo = Foo{
    Bar: 1,
    Baz: "cat",
}
```

Do *not* omit the names or put them all on the same line:

```go
var foo = Foo{1, "cat"} // wrong
var foo2 = Foo{Bar: 1, Baz: "cat"} // wrong
```

If there is only one field name, sometimes it is ok to put it on the same line as the opening `Foo{`.

### Returning errors

Return errors by checking `err != nil`:

```go
x, err := foo()
if err != nil { 
    return err
}
```

or

```go
if err := foo(); err != nil {
    return err
}
```

### Error values and types

Callers of a method may want to distinguish certain errors from others. There are two ways to do this.

An instance of an error can be created at package scope during init. This instance of an error can be returned and compared against by callers.

Example:

```go
package foo

var ErrSomethingWrong = errors.New("Something wrong")

func DoSomethingWrong() error {
    return ErrSomethingWrong
}
```

```go
package main

import foo

func main() {
    err := foo.DoSomethingWrong()
    switch err {
        case foo.ErrSomethingWrong:
            fmt.Println("DoSomethingWrong returned ErrSomethingWrong")
        default:
            fmt.Println("DoSomethingWrong returned some other error")
    }
}
```

The other way is to use typed errors, which can allow the caller to check if an entire class of error had been received. The type can be an interface or a struct. An example of an interface type error in the golang stdlib is [`net.Error`](https://golang.org/pkg/net/#Error). An example of a struct type error in the golang stdlib is [`net.AddrError`](https://golang.org/pkg/net/#AddrError).

```go
package foo

type Error struct {
    Val int
}

func DoSomethingWrong() error {
    return Error{
        Val: 1,
    }
}
```

```go
package main

import foo

func main() {
    err := foo.DoSomethingWrong()
    switch err.(type) {
        case foo.Error:
            fmt.Println("This is a foo.Error")
        default:
            fmt.Println("This is not a foo.Error")
    }
}
```

### Don't use named return values unless necessary

Named return values make the code more difficult to read and should only be used when necessary, such as when using a `defer` to catch an error on function exit.  Sometimes it can be used when there are non-error return values and it is not clear what the meaning of each return value is.

For example do *not* write a function like this:

```go
func foo(i int) (bar int, err error) {
    bar = i + 2
    return
}
```

Instead, write it like this:

```go
func foo(i int) (int, error) {
    return i + 2, nil
}
```

It is ok to use it if an error in a `defer` call needs to be returned to the caller:

```go
// CopyFile copy file
func CopyFile(dst string, src io.Reader) (n int64, err error) {
	// check the existence of dst file.
	if _, err := os.Stat(dst); err == nil {
		return 0, nil
	}
	err = nil

	out, err := os.Create(dst)
	if err != nil {
		return 0, err
	}
	defer func() {
		cerr := out.Close()
		if err == nil {
			err = cerr
		}
	}()

	n, err = io.Copy(out, src)
	return
}
```

It is also ok to use if there are a lot of return values and they would be ambiguous without names:

```go
func segmentSampling(...) (start int, end int, step int) {
    ...
}
```

### Do not use dot imports

Dot (`.`) imports pollute the namespace of the current package with another's. Don't do it. Avoid renaming the package import name too, unless necessary.