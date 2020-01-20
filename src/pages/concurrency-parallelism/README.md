# 并发与并行

## (D) 概览

**JS**

讨论Javascript并行问题最好的解答是来自这个[引用](http://debuggable.com/posts/understanding-node-js:4bd98440-45e4-4a9a-8ef7-0f7ecbdd56cb) by Felix Geisendörfer:
>  Well, in node everything runs in parallel, except your code.
>
> 好吧， 在node里面所有的东西都是并行的，除了你的代码。

也许你的Javascript 运行时 会使用多线程去处理IO，但是你自己的代码只是跑在一个线程里。这是事件模型的最深奥义。


不同的Javascript运行时在并发/并行方面进行不同的处理: NodeJS使用 [clustering](https://nodejs.org/docs/latest/api/cluster.html), 浏览器使用 [web workers](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers).


**Go**

另一方面，Go 是一个支持并行性并发的语言。它提供的`协程`允许函数并发运行，同时使用`管道`负责协程之间的通信。同时 Go的标准库文件有一个 "sync"包，提供了原生的同步方法，它提倡[encourages](https://blog.golang.org/share-memory-by-communicating) 更多的使用协程和通道，总结如下

> Do not communicate by sharing memory; instead, share memory by communicating
>
> 不要通过共享内存来通信，相反，应该通过通信来共享内存

延伸阅读:
- [Go Concurrency Patterns](https://talks.golang.org/2012/concurrency.slide#1)
- [Advanced Go Concurrency Patterns](https://talks.golang.org/2013/advconc.slide#1)


## (D) 异步 VS 同步 APIs
**JS**

JS 提倡使用异步 APIs，因为同步APIs 经常阻塞调用者，例如:

```Javascript
const fs = require('fs');

// 这个函数的调用者在文件读取时会阻塞
function fetchA() {
   return fs.readFileSync('a.txt');
}
```

上面的例子里，在大多数情况下，使用异步函数 `fs.readFile()` 将是一个更好的选择，将 `fetchA()` 变为异步，同时不阻塞他的调用者。


**Go**

另一方面，Go 提倡使用同步APIs (see “Avoid concurrency in your API” in https://talks.golang.org/2013/bestpractices.slide#26) ，原因是完全可以由它的调用者来决定是否使用阻塞或非阻塞的方式来调用它。看看下面定义的类型和同步 `fetchA`的函数:
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

如果调用者想要被阻塞，那么他可以这样调用这个函数

```Go
a := fetchA()
```

如果调用者不想被阻塞，那么他可以在协程里调用这个函数：

```Go
aChan := make(chan fetchResult, 0)
go func(c chan fetchResult) {
	c <- fetchA()
}(aChan)

// 做别的事儿，然后从管道里读取信息
a := <-aChan	
```

## (D) 顺序和并发模式

**JS**

即使没有并行，我们可以使用顺序和并发的方式构建Javascript 代码，
如下面的例子，我们假设 `fetchA()`, `fetchB()` 和 `fetchC()` 都是一部函数，并返回一个Promise.


**顺序执行**

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

如下面的例子，假设定义的 `fetchB()` and `fetchC()` 是与前面章节里`fetchA`的同步函数 (The full example is available at https://play.golang.org/p/2BVwtos4-j)



**同步**

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

	// 不考虑顺序
	a := <-aChan
	b := <-bChan
	c := <-cChan
	fmt.Println(a)
	fmt.Println(b)
	fmt.Println(c)
}
```

或者

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
