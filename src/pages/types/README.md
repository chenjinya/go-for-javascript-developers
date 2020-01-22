# 类型
## (D) 值，指针，引用

在Javascript中有值类型和引用类型之分。基础类型比如`string`和 `number`是值类型。对象(Object) 包括 array 和 function，是引用类型。

在Go中又值类型，引用类型和指针。引用类型有 slices, maps 和 channels
其余的都是值类型，但是可以成为*被引用*的指针。

区分引用和指针最大的区别是他们都可以改变底层值（当它是可变时），但是使用指针，你可以重新分配它。

（译者注：有点拗口，大概说的就是使用引用可以改变原始值，但是使用指针可以重新定义变量内容）

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

## (D) 数组 / 切片
**JS**

Javascript `Array` 是一个动态大小的列表，里面可以包含多种类型的元素。

```Javascript
const foo = ['Rick','Morty'];

// Adds to the end of the array.
foo.push('Beth');

// Removes from the end of the array.
element = foo.pop()
```

Javascript 还有一些“类型化数组”,例如 `Uint8Array` 是一个含有 8-bit无符号整数的类型。这些类型与`Array`类型并不是很相同。他们支持不同的方法以及通常使用在二进制数据上。


**Go**

Go 有 slice(切片) 和 array(数组) 类型。Javascript的Array 实际上比较类似Go的Slice类型，而不是 array类型。因为Go的Slice是一个动态大小的列表，而Array是静态大小的。切片和数组都是很具体的元素类型。给一个新的数组变量赋值为一个新的表露会直接复制他的内部数据（不是深层拷贝）. 给一个新的切片变量赋值不会拷贝数组的内部数据，因为切片不包含内部数据。

每个切片的背后是一个数组。这个背后的数组在代码中定义，或者被程序创建。

```Go
a := [3]int{1, 2, 3}

// 1. Create a slice by slicing an array
s1 := a[:]

// 2. Create a slice with slice literal
s2 := []int{1, 2, 3}

// 3.1 Create a zeroed slice with make()
s3_1 := make([]int, 3)

// 3.2 Same as previous line, except an addition of a cap param.
// The cap gives a slice a defined capacity with it's backing array.
// If the following slice will grow by 2 more elements, another array allocation will NOT be performed.
s3_2 := make([]int, 3, 5)

// 4. Create an "empty" slice by variable decleration
var s4 []int
```

数组有更多的限制。他们在编译中需要已知数组大学同时大小不能改在运行时修改。

```Go
// 1. Create an array with array literal
a1 := [3]int{1, 2, 3}

// 2. Create an zeroed array by variable decleration
var a2 [3]int
```

切片从一个已存在的数组或切片中创建。它使用 `[low:high]`语法，`low`是排除的最小边界， `high`是排除最大边界. 如果没有`low`，则`low`默认是0。如果没有`high`，`high`默认是数组/切片的长度


```Go
foo := []string{"Rick", "Morty"}

// Adds to the end of the array.
foo = append(foo, "Beth")
fmt.Println(foo)

// Removes from the end of the array.
n := len(foo) - 1 // index of last element
element := foo[n] // optionally also grab the last elemen
foo = foo[:n]     // remove the last element
```

注意: 一个切片最初是与数组引用同一片内存。但是切片会在程序增加切片长度之后会引用不同的数组（译者注：也就是写时复制）。下面的例子表述了这个情况：

```Go
fmt.Println("Hello, playground")
foo := [3]string{"a","b","c"}

bar := foo[:]
bar[0] = "changed"	
fmt.Println(foo, bar) // A change in bar is reflected in foo because bar is backed by foo

bar = append(bar, "d")
bar[0] = "changed again"	
fmt.Println(foo, bar) // A change in bar is no longer reflected in foo because the `append` caused bar to backed by a different array

//output
/* 
Hello, playground
[changed b c] [changed b c]
[changed b c] [changed again b c d] 
*/
```



## (D) 字典
**JS**

Javascript 有三个字典类型： `Object`, `Map`, `WeakMap`。不过`Map`和`WeakMap`是字典的专用类型，但是`Object`经常选做使用，因为`Object`是在`Map`和`WeakMap`出现之前的唯一一个选择。`Object`的键只能是字符串或者Symbol类型，而`Map`和`WeakMap`支持任何类型的键。

不过针对上面三个类型，还有些许的不同。`Map`的key是被排序的，而`Object`不是被排序的。


**Go**

Go只有一个`Map`类型。`Map`类型是引用类型，就像切片。给一个新的Map变量赋值的时候不会拷贝数据。


初始化一个Map:
```Go
// Initialise empty map with make.
m1 := make(map[string]int)

// Initialise with struct literals.
m2 := map[string]int{
  "foo": 42,
  "bar": 99,
}
```

访问元素:
```Go
v := m["key"] // If the requested key doesn't exist, we get the value type's zero value.
v, ok := m["key"] // Behaves the same as the previous line, but addtionaly the ok bool value can be used to test existence.
```

迭代:
```Go
// 没有指定迭代顺序，也不能保证每次迭代的顺序都是相同的。

for key, value := range m {
}
```

## (D) 集合
**JS**

Javascript 有三个集合类型,`Object`,`Set`,`WeakSet`。与字典类型相似，由于历史原因 `Object`依然会被当做"set"使用。`Object`的key必须是字符串或者Symbo类型，但是`Set`和`WeakSet`支持任意类型为key.使用`Object`当集合时，值是多余的。


**Go**

Go没有集合类型。但是他可以使用`Map`来实现集合的功能。[Golang's blog](https://blog.golang.org/go-maps-in-action)建议使用`bool`来当做值类型，比如 `map[T]bool`。但是，如果考虑内存占用，使用`map[T]struct{}`是一个好的替代方案，因为空的struct不会分配内存（注意`interface{}` 会分配内存看这个 StackOverflow [answer](https://stackoverflow.com/a/22770744) 有更多的信息).


