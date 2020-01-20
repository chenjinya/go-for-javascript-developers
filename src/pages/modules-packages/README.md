# 模块 / 包
## 规范与实践
**JS**

从ES6开始，Javascript规定了一个模块引用模式，不过在这之前，AMD和CommonJS规范已经非常流行了。

在ES6模块化之前，规范只支持 *script* 模式，其中每个文件共享相同的顶级全局作用域。这意味着脚本没有正式的 "文件作用域"。实际上，文件模块作用域是很常见的，不管是通过代码(`window.moduleA = …`)，或者外部工具(requireJS)，还是在运行时中的模块系统(NodeJS)

因此，可以肯定地说，Javascript程序通常使用具有本地作用域的文件与模块一对一对应的形式构建

**Go**
Go 从一开始就支持了import声明和包的支持。在Go中没有文件作用域，只有包作用域。从Go 1.6 (or 1.5 + flag)开始，对把依赖包放到项目目录中的 [vendor folder](https://blog.gopheracademy.com/advent-2015/vendor-folder/) 提供了更好的支持。然而它没有试图解决任何问题:


> … this does not attempt to solve the problem of vendoring resulting in multiple copies of a package being linked into a single binary. Sometimes having multiple copies of a library is not a problem; sometimes it is. At least for now, it doesn’t seem that the go command should be in charge of policing or solving that problem.
> 
> 它没有试图解决同一个第三方包被多次拷贝的问题。有些时候存在多拷贝不是一个问题，但是有些时候它是个问题。至少现在看来，Go并没有想负责解决这个问题。

如果选择使用Go模块（如go 1.11中介绍的），vendor文件夹默认是不必要的。(下载的模块源代码被存在目录`$GOPATH/pkg/mod`)。`go.mod`和`go.sum`替代了vendor文件夹，这两个文件提供 100% 可复制和构建安全（提供获取扩展模块的方式）。不过你也可以[保留](https://github.com/golang/go/wiki/Modules#how-do-i-use-vendoring-with-modules-is-vendoring-going-away) vendor文件夹 。


**不同点**

一个 JS 模块可以使任意有效的 JS类型。通过导出一个object,他可以将将多个函数、变量等放到*package*里。如果导出一个函数，它可以只具备这个函数的功能。另一方面，一个Go package，就像他的名字一样，只是一个包（译者注：老外真能瞎扯）。所以如果一个Javascript模块是一个函数类型，那么它可以直接被调用，这在Go里面是不可以的。

另外一个不同点是同一个项目中其他内部组件的调用成本。在Javascript中，因为每个文件都是一个模块（通常是这样），所以文件都需要被导入后才能使用。另一方面，在Go中，因为没有文件作用域，所有在同一包中的文件相互之间可以任意访问。

## 包管理

在Javascript中，**NPM** 是NodeJS的包管理工具，在客户端（译者注：浏览器）也几乎是如此。**Bower**在客户端也是一个流行的包管理软件

Go 1.11 引入了 [go mod](https://github.com/golang/go/wiki/Modules), 算是一个正式的依赖管理工具。

由于这是一个后期补充，所以其他社区的解决方案也在一些项目中使用。如下列表是部分的这些社区的解决方案:

- https://github.com/Masterminds/glide
- https://github.com/tools/godep
- https://github.com/kardianos/govendor
- https://github.com/mattn/gom
- https://github.com/golang/dep
