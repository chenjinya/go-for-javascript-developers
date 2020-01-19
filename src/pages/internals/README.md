# 内部原理
## (S) 堆/栈 内存分配

像 “堆”和“栈”的概念，在两种语言中已经被抽象出来（译者注：意思是不用关心）。你不用为此担心。尽管Golang有指针，它在编译时使用 [escape analysis](http://blog.rocana.com/golang-escape-analysis)  来计算内存分配。

## (S) 垃圾回收

垃圾回收在两种语言里已经集成。

## (D) 编译

Go 是编译型语言，而Javascript 不是，尽管一些Javascript运行时(run-time)是使用JIT进行编译的。从开发者的经验视角来看，编译型语言的最大影响就是编译时安全。Go是编译时安全的，但是在Javascript里，你可以使用额外的代码审查(code linters) 来弥补这个功能。
