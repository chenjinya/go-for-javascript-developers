# 错误处理
## (B) 流控制与值

两种语言把错误信息当做正则值(regular value)传递。另外两者都是用流控制结构：Javascript使用 `throw catch finally` ，Go使用 [`panic` `recover` `defer` ](https://blog.golang.org/defer-panic-and-recover)



## (D) 使用方式

经管上面的声明是相似的，这两种语言在错误处理的方式和都不太一样。

**JS**

在JS中，抛出错误的方式取决于函数的同步或异步性质。
如果函数是同步的，那么错误发生时可以使用 `throw`，调用者可以用 `try/catch`来捕获它。

另外，异步函数需要通过回调函数的第一个参数来传递错误信息，或者返回一个 'rejected' Promise.

需要注意 `async/await` 机制（草案中），它可以通过`try/catch`捕获异步函数的错误，从而使同步或异步函数都能被`try/catch`捕获处理。


**Go**

在Go中，抛出错误的方式取决于整个应用的上下文中的严重程度。

例如，对于一个Web服务器应用，如果错误发生在请求处理的代码路径中，它不应该导致整个服务挂掉。 因此，这些错误应该当做最后参数返回给调用者。

另外，如果错误发生在应用初始化阶段，毫无疑问，程序不需要再继续运行下去了，所以这里应该使用 `panic`

## (S) 跟踪堆栈的丢失

当把错误当做一个值来传递时，这里有个缺点，就是丢失了跟踪堆栈。这俩种语言都有此问题。这里有一些运行时的扩展库来帮助解决这个问题. 比如下面的库:

- JS: [longjohn](https://github.com/mattinsler/longjohn)
- Go: [errgo](https://github.com/juju/errgo)

## (D) 前景

Go的错误处理问题是讨论的热点。批评人士指出经常出现"nil handling"的错误处理代码。通常看起来像下面的代码:

```Go
if err != nil {
  return nil, err
}
```

一些人甚至踢出应该把上面的代码块制作成一个键盘按键，因为它使用的次数太多了。

这些评论也来自Go的用户。2018年有个[用户调查](https://blog.golang.org/survey2018-results) 展示出有5%的回答者将"错误处理"标记在 "当下使用Go语言面临最大的挑战是什么？"问题的下面。

Go团队已经回复了此问题，并且在[Go 2.0](https://blog.golang.org/go2-next-steps) 中，改善错误处理是其目标之一。错误处理改善提升之路注定是崎岖的。`try`的提议在一个公开讨论里被拒绝了，这表现出很多社区对提出的补丁并不是很满意。更多的细节请看[summary](https://www.infoq.com/news/2019/07/go-try-proposal-rejected/)。

我们可能需要到将来才能看到Go的错误处理到底变成了什么样子。
