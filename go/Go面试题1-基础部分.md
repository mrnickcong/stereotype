# Go面试题-基础部分

## Q1、 `=` 和 `:=` 的区别？

<details>
<summary>答案</summary>
<div>

`:=` 声明+赋值
`=` 仅赋值
```go
var foo int
foo = 10
// 等价于
foo := 10
```

</div>
</details>

## Q2.1 什么是指针和指针变量

<details>
<summary>答案</summary>
<div>

普通的变量，存储的是数据，而指针变量，存储的是数据的内存地址。

学习指针，主要有两个运算符号，要记牢

-   `&`：地址运算符，从变量中取得值的内存地址

```go
// 定义普通变量并打印
age := 18
fmt.Println(age) //output: 18

ptr := &age
fmt.Println(ptr) //output: 
```

-   `*`：解引用运算符，从内存地址中取得存储的数据

```go
myage := *ptr
fmt.Println(myage) //output: 18
```

</div>
</details>


## Q2.2、 指针的作用？

<details>
<summary>答案</summary>
<div>

指针用来保存变量的地址。

例如

```go
var x =  5
var p *int = &x
fmt.Printf("x = %d",  *p) // x 可以用 *p 访问
```

- `*` 运算符，也称为解引用运算符，用于访问地址中的值。
- `＆`运算符，也称为地址运算符，用于返回变量的地址。


</div>
</details>


## Q2.3 Go 中的指针的意义是什么？

<details>
<summary>答案</summary>
<div>

**意义一：省内存**

当你往一个函数传递参数时，若该参数是一个值类型的变量，则在调用函数时，会将原来的变量的值拷贝一遍。

假想每次传参都用数组，那么每次数组都要被复制一遍。如果数组大小有 100万，在64位机器上就需要花费大约 800W 字节，即 8MB 内存。这样会消耗掉大量的内存。

**意义二：易编码**

写了一个函数来实现更新某对象里的一些数据，在值类型的变量中，若不使用指针，则函数需要重新返回一个更新过的全新对象。

而有了指针，则可以不用返回。

</div>
</details>


## Q3、 Go 允许多个返回值吗？

<details>
<summary>答案</summary>
<div>

允许

```go
func swap(x, y string) (string, string) {
   return y, x
}

func main() {
   a, b := swap("A", "B")
   fmt.Println(a, b) // B A
}
```


</div>
</details>

## Q4、 Go 有异常类型吗？

<details>
<summary>答案</summary>
<div>

对错误和异常做一个解释
-   **错误**：指的是可能出现问题的地方出现了问题，比如打开一个文件时失败，这种情况在人们的意料之中 ；
-   **异常**：指的是不应该出现问题的地方出现了问题，比如引用了空指针，这种情况在人们的意料之外。

Go 没有异常类型，只有错误类型（Error），

当一个函数运行出错的时候，除了返回该功能函数的结果外，还应该返回一个 error 类型的值。

若该值为 nil 则表示，函数正常运行结束，反之，则函数运行异常。

```go
f, err := os.Open("test.txt")
if err != nil {
    log.Fatal(err)
}
```
一个函数要是想返回错误，通常会使用返回值来表示异常状态，它很像 C语言中的错误码，可逐层返回，直到被处理。

Go 语言中虽然没有异常的概念，但是却有更为恐怖的 panic ，由于有了 recover，在一定程度上， panic 可以类比做异常。

Golang错误和异常（panic）是可以互相转换的：

1.  **错误转异常**：比如程序逻辑上尝试请求某个URL，最多尝试三次，尝试三次的过程中请求失败是错误，尝试完第三次还不成功的话，失败就被提升为异常了。
2.  **异常转错误**：比如panic触发的异常被recover恢复后，将返回值中error类型的变量进行赋值，以便上层函数继续走错误处理流程。


</div>
</details>


## Q5、 Go 中的 rune 和 byte 有什么区别？

<details>
<summary>答案</summary>
<div>
一个字符串是由若干个字符组合而成的，比如 `hello`，就由 5 个字符组成。

在 Go 中字符类型有两种，分别是：

1.   byte 类型：字节，是 uint8 的别名类型
2.   rune 类型：字符，是 int32 的别名类型

byte 和 rune ，虽然都能表示一个字符，但 byte 只能表示 ASCII 码表中的一个字符（ASCII 码表总共有 256 个字符），数量远远不如 rune 多。

rune 表示的是 Unicode字符中的任一字符，而我们都知道，Unicode 是一个可以表示世界范围内的绝大部分字符的编码，这张表里几乎包含了全世界的所有的字符，当然中文也不在话下。

能表示的字符更多，意味着它占用的空间，也要更大，所占空间是 4个 byte 的大小。

下面以一段代码来验证一下他们的占用空间的差异

```go
var a byte = 'A'
var b rune = 'B'
fmt.Printf("a 占用 %d 个字节数\n", unsafe.Sizeof(a))
fmt.Printf("b 占用 %d 个字节数\n",unsafe.Sizeof(b))

// output
a 占用 1 个字节数
b 占用 4 个字节数
```

</div>
</details>










## Q5、 什么是协程（Goroutine）

<details>
<summary>答案</summary>
<div>

Goroutine 是与其他函数或方法同时运行的函数或方法。 Goroutines 可以被认为是轻量级的线程。 与线程相比，创建 Goroutine 的开销很小。 Go应用程序同时运行数千个 Goroutine 是非常常见的做法。


</div>
</details>

## Q6、 如何高效地拼接字符串

<details>
<summary>答案</summary>
<div>

Go 语言中，字符串是只读的，也就意味着每次修改操作都会创建一个新的字符串。如果需要拼接多次，应使用 `strings.Builder`，最小化内存拷贝次数。

```go
var str strings.Builder
for i := 0; i < 1000; i++ {
    str.WriteString("a")
}
fmt.Println(str.String())
```


</div>
</details>

## Q7、 什么是 rune 类型

<details>
<summary>答案</summary>
<div>

ASCII 码只需要 7 bit 就可以完整地表示，但只能表示英文字母在内的128个字符，为了表示世界上大部分的文字系统，发明了 Unicode， 它是ASCII的超集，包含世界上书写系统中存在的所有字符，并为每个代码分配一个标准编号（称为Unicode CodePoint），在 Go 语言中称之为 rune，是 int32 类型的别名。

Go 语言中，字符串的底层表示是 byte (8 bit) 序列，而非 rune (32 bit) 序列。例如下面的例子中 `语` 和 `言` 使用 UTF-8 编码后各占 3 个 byte，因此 `len("Go语言")` 等于 8，当然我们也可以将字符串转换为 rune 序列。

```go
fmt.Println(len("Go语言")) // 8
fmt.Println(len([]rune("Go语言"))) // 4
```


</div>
</details>

## Q8、 如何判断 map 中是否包含某个 key ？

<details>
<summary>答案</summary>
<div>

```go
if val, ok := dict["foo"]; ok {
    //do something here
}
```

`dict["foo"]` 有 2 个返回值，val 和 ok，如果 ok 等于 `true`，则说明 dict 包含 key `"foo"`，val 将被赋予 `"foo"` 对应的值。


</div>
</details>

## Q9、 Go 支持默认参数或可选参数吗？

<details>
<summary>答案</summary>
<div>
Go 语言不支持可选参数（python 支持），也不支持方法重载（java支持）。
</div>
</details>

## Q10、 defer 的执行顺序

<details>
<summary>答案</summary>
<div>

- 多个 defer 语句，遵从后进先出(Last In First Out，LIFO)的原则，最后声明的 defer 语句，最先得到执行。
- defer 在 return 语句之后执行，但在函数退出之前，defer 可以修改返回值。

例如：

```go
func test() int {
	i := 0
	defer func() {
		fmt.Println("defer1")
	}()
	defer func() {
		i += 1
		fmt.Println("defer2")
	}()
	return i
}

func main() {
	fmt.Println("return", test())
}
// defer2
// defer1
// return 0
```

这个例子中，可以看到 defer 的执行顺序：后进先出。但是返回值并没有被修改，这是由于 Go 的返回机制决定的，执行 return 语句后，Go 会创建一个临时变量保存返回值，因此，defer 语句修改了局部变量 i，并没有修改返回值。那如果是有名的返回值呢？

```go
func test() (i int) {
	i = 0
	defer func() {
		i += 1
		fmt.Println("defer2")
	}()
	return i
}

func main() {
	fmt.Println("return", test())
}
// defer2
// return 1
```

这个例子中，返回值被修改了。对于有名返回值的函数，执行 return 语句时，并不会再创建临时变量保存，因此，defer 语句修改了 i，即对返回值产生了影响。

</div>
</details>


## Q11、 如何交换 2 个变量的值？

<details>
<summary>答案</summary>
<div>

```go
a, b := "A", "B"
a, b = b, a
fmt.Println(a, b) // B A
```


</div>
</details>



## Q12、 Go 语言 tag 的用处？

<details>
<summary>答案</summary>
<div>

tag 可以理解为 struct 字段的注解，可以用来定义字段的一个或多个属性。框架/工具可以通过反射获取到某个字段定义的属性，采取相应的处理方式。tag 丰富了代码的语义，增强了灵活性。

例如：

```go
package main

import "fmt"
import "encoding/json"

type Stu struct {
	Name string `json:"stu_name"`
	ID   string `json:"stu_id"`
	Age  int    `json:"-"`
}

func main() {
	buf, _ := json.Marshal(Stu{"Tom", "t001", 18})
	fmt.Printf("%s\n", buf)
}
```

这个例子使用 tag 定义了结构体字段与 json 字段的转换关系，Name -> `stu_name`, ID -> `stu_id`，忽略 Age 字段。很方便地实现了 Go 结构体与不同规范的 json 文本之间的转换。 



</div>
</details>

## Q13、 如何判断 2 个字符串切片（slice) 是相等的？

<details>
<summary>答案</summary>
<div>

go 语言中可以使用反射 `reflect.DeepEqual(a, b)` 判断 a、b 两个切片是否相等，但是通常不推荐这么做，使用反射非常影响性能。

通常采用的方式如下，遍历比较切片中的每一个元素（注意处理越界的情况）。

```go
func StringSliceEqualBCE(a, b []string) bool {
    if len(a) != len(b) {
        return false
    }

    if (a == nil) != (b == nil) {
        return false
    }

    b = b[:len(a)]
    for i, v := range a {
        if v != b[i] {
            return false
        }
    }

    return true
}
```


</div>
</details>

## Q14、 字符串打印时，`%v` 和 `%+v` 的区别

<details>
<summary>答案</summary>
<div>

`%v` 和 `%+v` 都可以用来打印 struct 的值，区别在于 `%v` 仅打印各个字段的值，`%+v` 还会打印各个字段的名称。

```go
type Stu struct {
	Name string
}

func main() {
	fmt.Printf("%v\n", Stu{"Tom"}) // {Tom}
	fmt.Printf("%+v\n", Stu{"Tom"}) // {Name:Tom}
}
```

但如果结构体定义了 `String()` 方法，`%v` 和 `%+v` 都会调用 `String()` 覆盖默认值。


</div>
</details>

## Q15、 Go 语言中如何表示枚举值(enums)

<details>
<summary>答案</summary>
<div>

通常使用常量(const) 来表示枚举值。

```go
type StuType int32

const (
	Type1 StuType = iota
	Type2
	Type3
	Type4
)

func main() {
	fmt.Println(Type1, Type2, Type3, Type4) // 0, 1, 2, 3
}
```

参考 [What is an idiomatic way of representing enums in Go? - StackOverflow](https://stackoverflow.com/questions/14426366/what-is-an-idiomatic-way-of-representing-enums-in-go)


</div>
</details>

## Q16、 空 struct{} 的用途

<details>
<summary>答案</summary>
<div>

使用空结构体 struct{} 可以节省内存，一般作为占位符使用，表明这里并不需要一个值。

```go
fmt.Println(unsafe.Sizeof(struct{}{})) // 0
```

比如使用 map 表示集合时，只关注 key，value 可以使用 struct{} 作为占位符。如果使用其他类型作为占位符，例如 int，bool，不仅浪费了内存，而且容易引起歧义。

```go
type Set map[string]struct{}

func main() {
	set := make(Set)

	for _, item := range []string{"A", "A", "B", "C"} {
		set[item] = struct{}{}
	}
	fmt.Println(len(set)) // 3
	if _, ok := set["A"]; ok {
		fmt.Println("A exists") // A exists
	}
}
```

再比如，使用信道(channel)控制并发时，我们只是需要一个信号，但并不需要传递值，这个时候，也可以使用 struct{} 代替。

```go
func main() {
	ch := make(chan struct{}, 1)
	go func() {
		<-ch
		// do something
	}()
	ch <- struct{}{}
	// ...
}
```

再比如，声明只包含方法的结构体。

```go
type Lamp struct{}

func (l Lamp) On() {
        println("On")

}
func (l Lamp) Off() {
        println("Off")
}
```


</div>
</details>





<hr/>


## Q17、 Go语言make和new关键字的区别及实现原理
<details>
<summary>答案</summary>
<div>
    Go语言中的 new 和 make 主要区别如下：

  * make 只能用来分配及初始化类型为 slice、map、chan 的数据。new 可以分配任意类型的数据；
  * new 分配返回的是指针，即类型 *Type。make 返回引用，即 Type；
  * new 分配的空间被清零。make 分配空间后，会进行初始化；
  * new 只分配内存，而 make 只能用于 slice、map 和 channel 的初始化

    1. new
        在Go语言中，new 函数描述如下：
        
        // The new built-in function allocates memory. The first argument is a type,
        // not a value, and the value returned is a pointer to a newly
        // allocated zero value of that type.
        func new(Type) *Type
        
        从上面的代码可以看出，new 函数只接受一个参数，这个参数是一个类型，并且返回一个指向该类型内存地址的指针。
        同时 new 函数会把分配的内存置为零，也就是类型的零值。
        new 函数，它返回的永远是类型的指针，指针指向分配类型的内存地址。
        
    2. make
       make 也是用于内存分配的，但是和 new 不同，它只用于 chan、map 以及 slice 的内存创建，而且它返回的类型就是这三个类型本身，而不是他们的指针类型，
       因为这三种类型就是引用类型，所以就没有必要返回他们的指针了。

       make 函数只用于 map，slice 和 channel，并且不返回指针。
       如果想要获得一个显式的指针，可以使用 new 函数进行分配，或者显式地使用一个变量的地址。

</div>
</details>

## Q18、 为什么传参使用切片而不使用数组
<details>
<summary>答案</summary>
<div>

	Go里面的数组是值类型，切片是引用类型。

	值类型的对象在做为实参传给函数时，形参是实参的另外拷贝的一份数据，对形参的修改不会影响函数外实参的值

	而引用类型，则没有这个拷贝的过程，实参与形参指向的是同一块内存地址

	由此我们可以得出结论：

	把第一个大数组传递给函数会消耗很多内存，采用切片的方式传参可以避免上述问题。切片是引用传递，所以它们不需要使用额外的内存并且比使用数组更有效率。

	那么你肯定要问了，数组指针也是引用类型啊，也不一定要用切片吧？

	确实，传递数组指针是可以避免对值进行拷贝的内存浪费。
</div>
</details>


## Q19、 引用类型与指针，有什么不同
<details>
<summary>答案</summary>
<div>

	Go里面的数组是值类型，切片是引用类型。

	值类型的对象在做为实参传给函数时，形参是实参的另外拷贝的一份数据，对形参的修改不会影响函数外实参的值

	而引用类型，则没有这个拷贝的过程，实参与形参指向的是同一块内存地址

	由此我们可以得出结论：

	把第一个大数组传递给函数会消耗很多内存，采用切片的方式传参可以避免上述问题。切片是引用传递，所以它们不需要使用额外的内存并且比使用数组更有效率。

	那么你肯定要问了，数组指针也是引用类型啊，也不一定要用切片吧？

	确实，传递数组指针是可以避免对值进行拷贝的内存浪费。
</div>
</details>
