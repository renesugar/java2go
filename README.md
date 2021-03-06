java2go
=======

##### Convert Java code to something like Go

This has been my free-time hacking project for the last few months.  It's in a pre-alpha state, but it can convert around 75% of the Java code on my laptop into something that kind of looks like Go code, and if you're lucky that converted code may even be compilable.

##### Basic instructions

To use it:

	go get github.com/dglo/java2go

then either:

	go run src/github.com/dglo/java2go/main.go -- path/to/my.java

or:

    go build github.com/dglo/java2go
	./java2go -- path/to/my.java

##### Translating multiple files

If you specify a directory with `-dir`, it will write the translated file(s) to the corresponding `.go` file:

	go run src/github.com/dglo/java2go/main.go --dir . -- *.java

##### Customizing the translation

You can specify a config file with the `-config` option to specify how to translate Java packages to Go packages.  The config file supports three different directives:

* `PACKAGE a.b.c -> go_a_b_c` maps Java package `a.b.c` to Go package `go_a_b_c`
* `INTERFACE go_a_b_c.FooInterface` says Go object `FooInterface` in Go package `go_a_b_c` is an interface.  This is only needed for interfaces which are referenced but not defined in a class.
* `RECEIVER go_a_b_c.BarClass -> bc` uses `bc` as the name of the receiver object for all functions defined on BarClass, rather than the default `rcvr`.

##### Tweaking the code to translate your project

This project does some minor transformations from idiomatic Java into
somewhat idiomatic Go, but doesn't do anything with channels or goroutines.

If you'd like to add your own Java-to-Go transformation(s), you can check out `java2go/parser/transform.go`.  The `-report` option is helpful in determining what should be transformed.

##### Bugs

The most difficult bug to fix is the Lex/Yacc code which chokes on some legal Java, especially the  '...' operator.
