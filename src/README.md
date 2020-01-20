![book cover](./images/cover.png)

# 介绍

通常，开发者经常使用多种编程语言同时编程。在不同语言之间切换，是一个让人头疼的事儿。这种切换也会导致很多的Bug。比如如果你在Python和Javascript之间切换，可能会在判断数组是否为空的问题上犯错误（译者注: python: [] == False -> False, javascript: [] == false -> true )。同样，如果你在Go和Javascript来回切换，在`switch`语法上，会在 break/fallthrough 的默认行为上出现错误（译者注: Go 的switch默认在case里Break，而Javascript需要手动Break )。明确语言之间的差异能够帮助减少错误发生，且使语言之间切换更容易。

本文档在Golang与Javascript之间进行了比较。这两个语言搭配使用是在这些语言中最流行的。他们不一样，换句话说，他们是完全不同的。Javascript 是一个事件驱动、动态类型的解释型语言，但是Go是一个静态类型的编译型语言。

当你阅读此文档的时候，很有可能你已经了解了Javascript，并且刚刚开始学习Go。如果那样的话，需要你先完成如下的学习[A tour of go](https://tour.golang.org) and [Effective go](https://golang.org/doc/effective_go.html).


## 我应该使用哪种语言?

你应该选择正确的语言才能正确的完成工作。
不幸的是，永远不会有一个简单的公式来指导你应该选择哪种语言来完成给定你的任务。


![science is more art than science](./images/science_art.png)

除了技术方面的考虑，其他方面比如社区的发展同样重要。[据报道](http://highscalability.com/blog/2014/2/26/the-whatsapp-architecture-facebook-bought-for-19-billion.html)，Facebook 摒弃了 Erlang 是因为很那找到符合要求的程序员

尽管如此，值得注意的是Javascript在I/O密集型应用程序中表现优异，而在CPU密集型应用程序中表现较差。（译者注：不知道为啥要说这个 ）


## 语意

每个章节/主题用 (D), (S), (B) 来区分，以表明它们是以 '大部分不同', '大部分相似', '大部分两者都有'的形式对两种语言进行比较。

文档使用  ECMAScript 2015 (ES6) 标准的Javascript。同时需要注意一些章节使用了"NodeJS"的运行时环境


## 贡献

本书乐于接受建议、指正和参与。如此，请访问本书源码 [https://github.com/pazams/go-for-javascript-developers](https://github.com/pazams/go-for-javascript-developers) ，同时提issue或者PR

## 关于中文版

中文版翻译与注释由 [@chenjinya](https://github.com/chenjinya/go-for-javascript-developers)在业余时间完成，关于中文版的翻译问题、内容问题等的issue和PR可以提到中文版里。