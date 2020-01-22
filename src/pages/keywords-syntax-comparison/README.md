# 关键词和语法比较

## (D) `this` 关键词
**JS**

在一个对象方法里面，`this` 指向这个对象自己（不过有些例外）

**Go**

在Go中，最接近的类比就是内部函数的接收器.

你*可以*使用`this` 当做接收器

```Go
type Bar struct {
	foo string
}

func (this *Bar) Foo() string {
	return this.foo
}
```

更符合Go语言风格的是使用缩写变量当做接收器。在上面的例子里，`b`比`this`是一个更好的选择


## (D) `new` 关键词
**JS**

`new Foo()` 表示一个`Foo`的实例化对象 ，`Foo`可以是一个构造函数，或者是一个类 


**Go**

`new(T)` 表示为 `T`类型分配一块空内存并且返回一个指针，`*T`。这与Javascript或者其他大多数语言不一样，他们的`new`会**初始化**对象，但是Golang只是分配一块空内存。

这里需要提到一个 [语言习惯](https://blog.golang.org/package-names)，方法名加上"New"前缀代表这它返回一个"New"后面名字类型的指针。比如:

```Go
timer := time.NewTimer(d) // timer is a *time.Timer
```

## (D) 绑定或方法的值

**JS**
```Javascript
var f = bar.foo.bind(bar2); // when calling f(), "this" will refer to bar2
```

**Go**
```Go
f := bar.foo // f(), is same as bar.foo()
```

（译者注：Go没有this，使用接收器代替this）

## (S) 延时或计时器

**JS**
```Javascript
setTimeout(somefunction, 3*1000)
```

**Go**
```Go
time.AfterFunc(3*time.Second, somefunction)
```

## (D) 定时器或时钟

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

## (D) 字符串常量
**JS**

字符串可以被单引号(`'hello'`)或者双引号(`"hello"`)初始化，当然，大多数代码风格更倾向于使用单引号。原始字符串可以使用反引号(``` `hello` ```)。


**Go**

字符串可以被双引号(`"hello"`) 或者原始字符串可以使用反引号(``` `hello` ```)初始化


## (S) 注释

两种语言都使用 `/* block comments */`  和 `// line comments`.
