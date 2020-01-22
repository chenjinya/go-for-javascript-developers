# 函数
## (S) 一级函数

两种语言都把函数视为一级函数成员。都允许函数当做参数传递，或者当做返回值，或者嵌套，或者闭包

嵌套函数在Javascript中可以被命名或者是匿名函数，而Go中只能是匿名函数


## (D) 多返回值

Go函数可以返回多个值

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

Javascript不能使用多返回值的语法，我们可以做一个类似的形式


**JS**
```Javascript
function hello() {
  return ["hello", "world"];
}

var [a, b] = hello();
console.log(a,b);
```

## (S) IIFE

（译者注： Immediately Invoked Function Expression 立即调用的函数表达式）

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

## (S) 闭包

两种语言都支持闭包，都需要注意的是当[在循环中使用闭包](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures#Creating_closures_in_loops_A_common_mistake). 以下是两种语言的示例，演示了绕过闭包/循环"陷阱"的方式：

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
