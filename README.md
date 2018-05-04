**-- WORK IN PROGRESS --**

![book cover](/images/cover.png)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [Introduction](#introduction)
  - [Preface](#preface)
  - [Which language should I use?](#which-language-should-i-use)
  - [Semantics](#semantics)
  - [Contributions](#contributions)
- [Internals](#internals)
  - [(S) Heap/Stack Memory Allocation](#s-heapstack-memory-allocation)
  - [(S) Garbage Collection](#s-garbage-collection)
  - [(D) Compilation](#d-compilation)
- [Concurrency & Parallelism](#concurrency--parallelism)
  - [(D) Overview](#d-overview)
  - [(D) Async vs Sync APIs](#d-async-vs-sync-apis)
  - [(D) Sequential and Concurrent Patterns](#d-sequential-and-concurrent-patterns)
- [Modules / Packages](#modules--packages)
  - [Spec & Practice](#spec--practice)
  - [Management](#management)
- [Error Handling](#error-handling)
  - [(B) Flow control and values](#b-flow-control-and-values)
  - [(D) Usage](#d-usage)
  - [(S) Loss of stack trace](#s-loss-of-stack-trace)
- [Keywords & Syntax Comparison](#keywords--syntax-comparison)
  - [(D) `this` keyword](#d-this-keyword)
  - [(D) `new` keyword](#d-new-keyword)
  - [(D) bind / method values](#d-bind--method-values)
  - [(S) setTimeout / timer](#s-settimeout--timer)
  - [(D) setInterval / ticker](#d-setinterval--ticker)
  - [(D) String literals](#d-string-literals)
  - [(S) Comments](#s-comments)
- [Variables](#variables)
  - [(D) Values, Pointers, References](#d-values-pointers-references)
  - [Types](#types)
- [Flow control statements](#flow-control-statements)
  - [(B) Loops and iteration](#b-loops-and-iteration)
    - [For](#for)
    - [While](#while)
    - [Iterating over an Array/Slice](#iterating-over-an-arrayslice)
  - [(B) If/Else](#b-ifelse)
  - [(D) Switch](#d-switch)
- [Functions](#functions)
  - [(S) first-class functions](#s-first-class-functions)
  - [(D) Multiple returns](#d-multiple-returns)
  - [(S) IIFE](#s-iife)
  - [(S) Closures](#s-closures)
- [License](#license)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Introduction

## Preface

Often, a developer will use more than one programming language at a certain timeframe. Switching back and forth between languages can come with some overhead. These context switches can also result in bugs. For instance, if you switch back and forth between Python and Javascript, there's a likelihood you'll mistake evaluation of an empty array between truthy and falsey. Similarly, if you switch back and forth between Go and Javascript, there's a likelihood you'll mistake `switch` statements default behavior of break/fallthrough. Outlining the differences between languages can help mitigate these potential issues, and make it easier to transition back and forth.
通常，开发者在某个期间都会使用多个编程语言。在多个语言之间切换会带来某些开销。在语言之间的上下文也可能错误发生。例如，如果你在Python和Javascript之间来回切换，有可能你会错误地评估一个空数组array是true还是false。同样的，如果你在Go与Javascript之间来回切换，有可能你会错误地评估`switch`声名的默认行为是往下走还是跳过。概述语言之间的差异可以帮助解决潜在问题，并且可以更容易地在多个语言来回转换

This document compares between two programming languages, Golang (or "Go") and ECMAScript (or "Javascript" / "JS"). The merits of this pairing is the popularity of these languages. That's it. They are not similar, in fact, they are quite different. Javascript is an event driven, dynamically typed and interpreted language, while Go is a statically typed and compiled language.
这个文档是 Golang (or "Go") and ECMAScript (or "Javascript" / "JS")两个编程语言之间的比较。值得比较的是两个语言都是流行的语言。其实。它们并不相似。Javascript是事件驱动，动态类型的解释性语言，Go是静态类型编译性语言

If you're reading this there's a high chance you already know your Javascript and are just starting with Go. If so, make sure you first complete [A tour of go](https://tour.golang.org) and [Effective go](https://golang.org/doc/effective_go.html).
如果你正在阅读这个篇文章说明你已经熟悉Javascript并且开始使用Go [A tour of go](https://tour.golang.org) and [Effective go](https://golang.org/doc/effective_go.html)

## Which language should I use?
## 我应该使用那个语言？

You should always pick the right tool for the right job. Unfortunately, there will never be a simple formula that will instruct you which programming language you should choose to complete a given task.
你应该始终为正确的任务选择合适的工具。 不幸的是，永远不会有一个简单的公式来指导您选择哪种编程语言来完成给定的任务。


![science is more art than science](/images/science_art.png)

Aside of technical considerations, other considerations, such as community adoption are also important. It was [reported](http://highscalability.com/blog/2014/2/26/the-whatsapp-architecture-facebook-bought-for-19-billion.html) that Facebook moved away from the Erlang language because it was hard to find qualified programmers.
除了技术上的考虑外，其他考虑因素（如社区采用）也很重要。 这是[报道]（http://highscalability.com/blog/2014/2/26/the-whatsapp-architecture-facebook-bought-for-19-billion.html）Facebook从Erlang语言转移出去，因为它 很难找到合格的程序员。

Having said that, it is worthy to note that Javascript excels in I/O intense applications, and less so in CPU intense applications.
话虽如此，值得注意的是，Javascript擅长I/O密集型应用程序，在轻CPU密集型应用程序中也是如此。

## Semantics
## 语义
Each subchapter/subject is denoted with (D),(S) or (B) to indicate how it compares across both languages with 'mostly **D**ifferent', 'mostly **S**imilar' or 'mostly **B**oth'.
每个子章节使用（D），（S）或（B）表示如何在两种语言中比较'**不同**'，'**相似**'或'**都一样**'。

This document uses ECMAScript 2015 (ES6).
Also, some subjects will note the run-time environment "NodeJS".
本文档使用ECMAScript 2015（ES6）。 另外，有些章节需要注意运行环境是“NodeJS”。

## Contributions
This document is a work in progress. Contributions and PRs are most welcomed.
If you edit the chapters layout, be sure to rebuild the table of contents by
这份文档还在处理中，欢迎贡献和PR。
如果你对章节进行过编辑，请使用以下命令重新构建并且通过
```bash
npm install
npm run toc
```
If you edit go code, be sure to format it with (requires [mdgofmt-cli](https://github.com/pazams/mdgofmt-cli))
```bash
npm run fmt
```
Or just run both commands with
```bash
npm run build
```

# Internals
# 内部机制
## (S) Heap/Stack 内存分配
Concepts such "Heap" and "Stack" are abstracted away in both languages. You do not needed to worry about it. Even though GO has pointers, it uses [escape analysis](http://blog.rocana.com/golang-escape-analysis) in compile-time to figure out the memory allocation.
“堆”和“堆栈”这样的概念在两种语言中都被抽象出来。你不需要担心它。尽管GO有指针，但它在编译时使用[escape analysis]（http://blog.rocana.com/golang-escape-analysis）来计算内存分配。
## (S) Garbage Collection(内存回收)
Garbage collection is implemented in both languages.
## (D) Compilation(编译)
Go is compiled. Javascript is not, though some Javascript runtimes use JIT compilation. From the developer experience perspective, the biggest effect of compiled languages is compile-time safety. You get compile-time safety with Go, while in Javascript you can use external code linters to ease the missing of this feature.
Go是需要编译的而Javascript不需要，Javascript运行是通过JIT编译
Go是需要编译。Javascript不是，尽管一些Javascript运行时使用JIT编译。从开发人员的经验角度来看，编译语言的最大影响是编译时安全。您可以通过Go获得编译时安全性，而在Javascript中，您可以使用外部代码库来缓解此功能的缺失。

# Concurrency & Parallelism
# 并发 & 并行
## (D) Overview
## (D) 概述

**JS**

The best way to describe Parallelism in Javascript is with this [quote](http://debuggable.com/posts/understanding-node-js:4bd98440-45e4-4a9a-8ef7-0f7ecbdd56cb) by Felix Geisendörfer:
>  Well, in node everything runs in parallel, except your code.

最好的描述Javascript并行机制的文章 [quote](http://debuggable.com/posts/understanding-node-js:4bd98440-45e4-4a9a-8ef7-0f7ecbdd56cb) by Felix Geisendörfer:
>  在Node.js除了你的代码，所有都是并行运行的

So while your Javascript runtime may use multiple threads for IO, your own code is getting run just by one. That's just how the *evented* model works.
Different Javascript runtimes offer some options for concurrency or parallelism: NodeJS offers [clustering](https://nodejs.org/docs/latest/api/cluster.html), and Browsers offer [web workers](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers).

所以，当你的Javascript运行时使用多个线程进行IO操作时，你自己的代码只运行一次。 这就是*evented*模型的工作原理。
不同的Javascript运行时为并发或并行提供了一些选项：NodeJS提供[clustering]（https://nodejs.org/docs/latest/api/cluster.html），浏览器提供[web workers](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers).

**Go**

On the other hand, Go is all about a concurrency which enables parallelism. It offers Goroutines which enables functions to run concurrently, and channels to communicate between them. While Go standard library has the "sync" package for synchronization primitives, it [encourages](https://blog.golang.org/share-memory-by-communicating) more the use of Goroutines and channels, summarized as:

另一方面，Go是关于并行性的并发性。 它提供了Goroutines，它使功能可以同时运行，并可以在它们之间进行通信。 虽然Go标准库具有同步原语的“同步”包，但它[鼓励]（https://blog.golang.org/share-memory-by-communicating）更多地使用Goroutines和channels，总结如下：

> Do not communicate by sharing memory; instead, share memory by communicating

> 不要通过共享内存来通讯，应该通过通讯共享内存

More on this subject:
- [Go Concurrency Patterns](https://talks.golang.org/2012/concurrency.slide#1)
- [Advanced Go Concurrency Patterns](https://talks.golang.org/2013/advconc.slide#1)


## (D) Async vs Sync APIs
**JS**

JS promotes writing async APIs, since sync APIs always block the caller, e.g:
JS提倡编写异步API，因为同步API总是会被阻塞，例如：
```Javascript
const fs = require('fs');

// The caller to this function will be blocked while the file is being read.
// 当文件开始读取时，调用者调用这个函数将会被阻塞
function fetchA() {
   return fs.readFileSync('a.txt');
}
```
In the example above, the async `fs.readFile()` would be a better choice in most scenarios, making `fetchA()` async and unblocking its caller.
在上面的示例里，异步`fs.readFile()`是最佳的选择，让`fetchA()`不会阻塞调用者

**Go**

On the other hand, Go promotes the sync APIs (see “Avoid concurrency in your API” in https://talks.golang.org/2013/bestpractices.slide#26)
The reasoning behind this is that it is completely up to the caller's choice to be blocked or not by a sync API. Consider the following type definition and sync `fetchA` function:
在另一方面，Go优先是使用同步API，(see “Avoid concurrency in your API” in https://talks.golang.org/2013/bestpractices.slide#26)
是否被阻塞完全取决于调用者。 考虑下面的类型定义和同步`fetchA`函数：
```Go
type fetchResult struct {
	Message string
	Error   error
}

func fetchA() fetchResult {
	time.Sleep(time.Second * 4)
	return fetchResult{"A data", nil}
}
```
If the caller wants to be blocked, then he can just call the function
如果调用者想使用被阻塞方式，那么它可以如下调用这个函数
```Go
	a := fetchA()
```
If the caller does not want to be blocked, then he could call the function inside a goroutine:
如果调用者不想被阻塞，那么它可以使用goroutine调用这个函数
```Go
	aChan := make(chan fetchResult, 0)
	go func(c chan fetchResult) {
		c <- fetchA()
	}(aChan)
```

## (D) Sequential and Concurrent Patterns
## (D) 串行与并发模式

**JS**

Even without parallelism, we can structure Javascript code in both sequential and concurrent flows.
For the following exmaples, let’s assume `fetchA()`, `fetchB()` and `fetchC()` are all async functions returning a promise.

即使没有并行性，我们也可以在串行和并发流程中构造Javascript代码。
对于下面的例子，假设`fetchA（）`，`fetchB（）`和`fetchC（）`都是返回promise的异步函数。

**串行**

```Javascript
function fetchSequential() {
    fetchA().then(a => {
        console.log(a);
        return fetchB();
    }).then(b => {
        console.log(b);
        return fetchC();
    }).then(c => {
        console.log(c);
    })
}
```

## (D) 串行与并发模式
或者 

```Javascript
async function fetchSequential() {
    const a = await fetchA();
    console.log(a);
    const b = await fetchB();
    console.log(b);
    const c = await fetchC();
    console.log(c);
}
```

**并发**

```Javascript
function fetchConcurrent() {
    Promise.all([fetchA(), fetchB(), fetchC()]).then(values => {
        console.log(values);
    }
}
```

或者

```Javascript
async function fetchConcurrent() {
    const values = await Promise.all([fetchA(), fetchB(), fetchC()])
    console.log(values);
}
```

**Go**

For the following examples, assume `fetchB()` and `fetchC()` are defined as a sync function similarly to `fetchA` in the previous section (The full example is available here https://play.golang.org/p/2BVwtos4-j)

对于下面的例子，假设`fetchB（）`和`fetchC（）`被定义为一个同步函数，类似于前一节中的`fetchA`（完整的例子可以在这里https://play.golang.org/p/2BVwtos4-j）

**串行**

```Go
func fetchSequential() {
	a := fetchA()
	fmt.Println(a)
	b := fetchB()
	fmt.Println(b)
	c := fetchC()
	fmt.Println(c)
}
```
**并发**

```Go
func fetchConcurrent() {
	aChan := make(chan fetchResult, 0)
	go func(c chan fetchResult) {
		c <- fetchA()
	}(aChan)
	bChan := make(chan fetchResult, 0)
	go func(c chan fetchResult) {
		c <- fetchB()
	}(bChan)
	cChan := make(chan fetchResult, 0)
	go func(c chan fetchResult) {
		c <- fetchC()
	}(cChan)

	// order doesn't really matter!
	a := <-aChan
	b := <-bChan
	c := <-cChan
	fmt.Println(a)
	fmt.Println(b)
	fmt.Println(c)
}
```

or 

```Go

func fetchConcurrent() {
	aChan := make(chan fetchResult, 0)
	bChan := make(chan fetchResult, 0)
	cChan := make(chan fetchResult, 0)

	go func(c chan fetchResult) {
		c <- fetchA()
	}(aChan)
	go func(c chan fetchResult) {
		c <- fetchB()
	}(bChan)
	go func(c chan fetchResult) {
		c <- fetchC()
	}(cChan)

	for i := 0; i < 3; i++ {
		select {
		case a := <-aChan:
			fmt.Println(a)
		case b := <-bChan:
			fmt.Println(b)
		case c := <-cChan:
			fmt.Println(c)

		}
	}
}

```

# Modules / Packages
## Spec & Practice
**JS**

As of es6, the Javascript spec includes a module system, however the external specs of AMD and CommonJS are also popular since the language began to address this issue rather late.
从es6开始，Javascript规范包含一个模块系统，然而AMD和CommonJS的外部规格也很流行，因为语言开始解决这个问题的时间比较晚。

Before es6 modules, the spec only supported the *script* mode, of which every file shares the same top-level global scope. This means that there was no official "file scope" for scripts. In practice, file-module scope was common since it was either introduced by code (`window.moduleA = …`), an external tool (requireJS), or by a runtime that baked-in a module system (NodeJS).
在es6模块之前，该规范仅支持* script *模式，其中每个文件共享相同的顶级全局范围。 这意味着对于脚本没有官方的“文件范围”。 在实践中，文件模块作用域很常见，因为它是由代码（`window.moduleA = ...`），外部工具（requireJS）引入的，或者是由模块系统（NodeJS）中的烘焙运行时引入的。

Therefore, it is safe to say that Javascript programs are commonly structured with a 1-to-1 relationship between files and modules with local scope.

**Go**

Go's import statement and package support were part of the spec from the beginning. In Go there is no file scope, only package scope. As of Go 1.6 (or 1.5 + flag), there's better support for encapsulating dependent packages inside a project with the [vendor folder](https://blog.gopheracademy.com/advent-2015/vendor-folder/). However, it doesn't attempt to solve everything:
> … this does not attempt to solve the problem of vendoring resulting in multiple copies of a package being linked into a single binary. Sometimes having multiple copies of a library is not a problem; sometimes it is. At least for now, it doesn’t seem that the go command should be in charge of policing or solving that problem.

**The differences**

A Javascript module can be any valid Javascript type. By exporting an object, it can *package* multiple functionalities. By exporting a function it can surface a single functionality. On the other hand, a Go package, is as its a name- just a package. So while a Javascript module can be directly invoked if it is a function type, this is not a possibility with a Go package.

Another difference is the consumption of other internal components within your project. In Javascript, since each file is (usually) a module, then each of the files that were decoupled from the current file must be imported. On the other hand, in Go, all files within the same package can have access to each other since there is no file scope.

## Management

For Javascript development, NPM is the de-facto package manager for NodeJS, and may also be used for client side projects. Bower is also a popular for client side projects.

The `go get` tool will only get you as far as getting a dependency latest master code. This will not suffice if you need accurate dependency management with pinned versions. The Go community came up with several package managers, here's a partial list:

- https://github.com/kovetskiy/manul
- https://github.com/tools/godep
- https://github.com/kardianos/govendor
- https://github.com/FiloSottile/gvt
- https://github.com/Masterminds/glide
- https://github.com/mattn/gom

Go has acknowledged the need for a dependency management tool by starting its own project: [dep](https://github.com/golang/dep). As of the time of writing, it is still in Alpha phase, and not part of official Go toolchain yet. Watch that project [roadmap](https://github.com/golang/dep/wiki/Roadmap) for status updates!

# Error Handling
## (B) Flow control and values

Both languages pass errors as regular values. Also, both languages leverage flow control constructs: Javascript uses `throw` `catch` `finally` block, and Go uses [`panic` `recover` `defer` ](https://blog.golang.org/defer-panic-and-recover)

## (D) Usage
Despite the similarity claimed above, the languages differ on how and when errors are handled:

**JS**
In JS, the way to propegate an error is determined by the synchorinic nature of the function.
If a function is synchronous, then it should use `throw` when an error occurs, and the caller should use `try/catch` blocks.

Otherwise, an asynchronous function should propagate the error by passing it as a first value to a callback function, or it should return a rejected promise.

Note the `async/await` mechanism, which is in draft, will consolidate both worlds by having asynchronous errors being handled inside `try/catch` blocks.

**Go**
In Go on the other hand, the way to propagate an error is determined by the degree of severity with context of the whole application.

For example, for a web-server application, if errors occur in a request handling code path, they should not crash the entire server.  Therefore, these errors should be returned as a last argument to the caller.

On the other hand, if an error occurs during the application init, it can be argued that there’s no reason to continue, and therefore `panic` would make sense.

## (S) Loss of stack trace
While passing errors as values, one drawback is the loss of stack trace. Both languages suffer from this. Some runtimes and libraries try to help. Some libraries:
- JS: [longjohn](https://github.com/mattinsler/longjohn)
- Go: [errgo](https://github.com/juju/errgo)

# Keywords & Syntax Comparison

## (D) `this` keyword
**JS**

Inside an object method, `this` refers to the object (with some exceptions).

**Go**

In Go, the closest analogy would be receivers inside method functions.
You *may* use `this` as a receiver:
```Go
type Boo struct {
	foo string
}

func (this *Boo) Foo() string {
	return this.foo
}
```
It is more idiomatic to use short variables as receivers. In the example above `b` would have been a better fit over `this`.

## (D) `new` keyword
**JS**

`new Foo()` instantiates an object from `Foo`, a constructor function or a class.

**Go**

`new(T)` allocates zeroed storage for a new item of type `T` and returns a pointer, `*T`. This is different than Javascript and most other languages where `new` will **initialize** the object, while in Golang it only **zeros** it.

It is worthy to mention that it is [idiomatic](https://blog.golang.org/package-names) to name methods with a "New" prefix to denote it returns a pointer to the type following in the method name. e.g:
```Go
timer := time.NewTimer(d) // timer is a *time.Timer
```

## (D) bind / method values

**JS**
```Javascript
var f = boo.foo.bind(boo2); // when calling f(), "this" will refer to boo2
```

**Go**
```Go
f := boo.foo // f(), is same as boo.foo()
```

## (S) setTimeout / timer

**JS**
```Javascript
setTimeout(somefunction, 3*1000)
```

**Go**
```Go
time.AfterFunc(3*time.Second, somefunction)
```

## (D) setInterval / ticker

**JS**
```Javascript
setInterval(somefunction, 3*1000)
```

**Go**
```Go
ticker := time.NewTicker(3 * time.Second)
go func() {
	for t := range ticker.C {
		somefunction()
	}
}()
```

## (D) String literals
**JS**

Strings are initialized with single quotes (`'hello'`) or double quotes (`"hello"`), yet most coding styles prefer the single quotes variation. Raw string literals use backticks (``` `hello` ```).

**Go**

Strings are initialized with double quotes (`"hello"`) or raw string literals with backticks (``` `hello` ```)

## (S) Comments
Both languages use the same `/* block comments */`  and `// line comments`.

# Variables
## (D) Values, Pointers, References

In Javascript there are value types and reference types. Primitives such as `string` and `number` are value types. Objects, including arrays and functions, are reference types.

In Go, there are value types, reference types, and pointers. References types are slices, maps, and channels. All the rest are value types, but have the ability "*to be referenced*" with pointers.
The most practical difference to remember between references and pointers, is that while you can use both to mutate the underlaying value (when it is mutable), with pointers you can also reassign it.

**JS**
```Javascript
var a = {
    message: 'hello'
}

var b = a;

// mutate
b.message = 'goodbye';
console.log(a.message === b.message); // prints 'true'

// reassign
b = {
    message: 'galaxy'
}
console.log(a.message === b.message); // prints 'false'
```

**Go**
```Go
a := struct {
	message string
}{"hello"}

b := &a

// mutate
// note b.message is short for (*b).message
b.message = "goodbye"
fmt.Println(a.message == b.message) // prints "true"

// reassign
*b = struct {
	message string
}{"galaxy"}
fmt.Println(a.message == b.message) // prints "true"
```

## Types
TBD

# Flow control statements
## (B) Loops and iteration
### For
**JS**
```Javascript
for(let i=0;i<10;i++){
    console.log(i);
}
```

**Go**
```Go
for i := 0; i < 10; i++ {
	fmt.Println(i)
}
```

### While
In Go, the `for`'s init and post statement are optional, effectively making it also a "while" statement:

**JS**
```Javascript
var i=0;
while(i<10){
    console.log(i);
    i++;
}
```

**Go**
```Go
i := 0
for i < 10 {
	fmt.Println(i)
	i++
}
```
### Iterating over an Array/Slice

**JS**
```Javascript
['Rick','Morty','Beth','Summer','Jerry'].forEach(function(value,index){
    console.log(value + ' at index ' + index);
});
```

**Go**
```Go
for i, v := range []string{"Rick", "Morty", "Beth", "Summer", "Jerry"} {
	fmt.Printf("%v at index %d", v, i)
}
```


## (B) If/Else
Go's `if` can contain an init statement, with variables declared scoped only to the `if` and `else` blocks.

**Go**
```Go
if value := getSomeValue(); value < limit {
	return value
} else {
	return value / 2
}
```

## (D) Switch
The switch statement was one of the motivation for writing this document.

Go defaults to break, and `fallthrough` needed for otherwise.

Javascript defaults to fallthrough, and `break` needed for otherwise.

**JS**
```Javascript
switch (favorite) {
    case "yellow":
        console.log("yellow");
        break;
    case "red":
        console.log("red");
    case "pruple":
        console.log("(and) purple");
    default:
        console.log("white");
}

```

**Go**
```Go
switch favorite {
case "yellow":
	fmt.Println("yellow")
case "red":
	fmt.Println("red")
	fallthrough
case "pruple":
	fmt.Println("(and) purple")
default:
	fmt.Println("white")
}
```

# Functions
## (S) first-class functions
Both languages treat functions as first-class citizens. Both allow functions to be passed as arguments, to be a returned value, to be nested, and have closures.

Function nesting in Javascript can be done both with named and anonymous functions, while in Go this can only be done with anonymous functions.

## (D) Multiple returns
Go functions can return multiple values

**Go**
```Go
func hello() (string, string) {
	return "hello", "world"
}

func main() {
	a, b := hello()
	fmt.Println(a, b)
}
```
Javascript cannot, however by using destructuring assignment syntax, we can get a similar behavior

**JS**
```Javascript
function hello() {
  return ["hello", "world"];
}

var [a, b] = hello();
console.log(a,b);
```

## (S) IIFE

**JS**
```Javascript
(function () {
  console.log('hello');
}());
```

**Go**
```Go
func main() {
	func() {
		fmt.Println("Hello")
	}()
}
```

## (S) Closures
Both languages have closures. Both require caution when [creating closures inside loops](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures#Creating_closures_in_loops_A_common_mistake). Here are examples in both languages that demonstrate a similar technique to bypass the closure/loop trap:

**JS (with bug)**
```Javascript
var i = 0;
for (;  i < 10 ; i++) {
  setTimeout((function() {console.log(i);}),0);
}
```

**JS (solved)** (note that using `for(let i=0; …` instead of `var` is a more practical solution)
```Javascript
var i = 0;
for (;  i < 10 ; i++) {
  (function (i) {
    setTimeout((function() {console.log(i);}),0);
  }(i));
}
```

**Go (with bug)**
```Go
var wg sync.WaitGroup
wg.Add(10)
for i := 0; i < 10; i++ {
	go func() {
		defer wg.Done()
		fmt.Println(i)
	}()
}
wg.Wait()
```

**Go (solved)**
```Go
var wg sync.WaitGroup
wg.Add(10)
for i := 0; i < 10; i++ {
	go func(i int) {
		defer wg.Done()
		fmt.Println(i)
	}(i)
}
wg.Wait()
```
# License

Copyright Maor Zamski & Daniel Singer

"Go for Javascript Developers" is released under the [Creative Commons Attribution-ShareAlike 4.0 International License.](http://creativecommons.org/licenses/by-sa/4.0/).

The "gopher" used at the cover was [created](https://github.com/golang-samples/gopher-vector) by [Takuya Ueda](https://twitter.com/tenntenn). It is licensed under the [Creative Commons 3.0 Attributions license](https://creativecommons.org/licenses/by/3.0/deed).
