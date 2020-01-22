# 流程控制语句
## (B) 循环和迭代
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

在Go中，`for`的初始化和过程语句是可选的，实际上它也是一个“while”语句


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
### 遍历一个 Array或Slice

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

Go的 `if` 可以包含初始化语句，里面的变量声明的作用域是 `if`和`else`的块中。


**Go**
```Go
if value := getSomeValue(); value < limit {
	return value
} else {
	return value / 2
}
```

## (D) Switch

swtich语句是写这篇文档的动机之一。

Go默认break，否则需要使用`fallthrough`

Javascript 默认是 fallthrough，否则需要使用 `break`

（译者注: fallthrough指的是自动执行下一条条件语句）

**JS**
```Javascript
switch (favorite) {
    case "yellow":
        console.log("yellow");
        break;
    case "red":
        console.log("red");
    case "purple":
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
case "purple":
	fmt.Println("(and) purple")
default:
	fmt.Println("white")
}
```
