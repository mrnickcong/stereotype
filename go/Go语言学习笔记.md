# 第一部分：Go语言的数据类型

##

### 一、Go语言的数据类型

Go语言按照数据的存储方式可以分为：值类型和引用数据类型

  1. 值类型。也叫基本数据类型：数值类型、bool、string、数组、struct 结构体
     1. 值类型：变量直接存储值。值类型的数据存储在栈内存空间中，栈在函数调用完内存会被释放。
  2. 引用数据类型：指针、slice 切片、管道 chan、map、以及 interface
     1. 引用类型：变量存储的是一个地址，这个地址存储最终的值。引用数据类型的数据存储在堆内存空间中，通过 GC 回收。

也有种说法：
  复合数据类型：数组和结构体。是通过组合简单类型，来表达更加复杂的数据结构

### 二、值类型和引用数据类型

#### 值类型
  1. 数值类型：整形、浮点、其他
      1. 整型主要有 int 、int16、int32、int64、uint、uint8、uint16、uint32、uint64。如下表格
        1. uint8：无符号 8 位整型 (0 到 255)
        2. int8：有符号 8 位整型 (-128 到 127)
      2. 浮点数类型主要有 float32、float64、complex64、complex128
          1. float32：IEEE-754 32 位浮点型数
          2. float64：IEEE-754 64 位浮点型数
          3. complex64：32 位实数和虚数
          4. complex128：64 位实数和虚数
      3. 其他数字类型
        1. byte ：类似 uint8，代表了 ASCII 码的一个字符，也可以说是 ASCII 字符类型
        2. rune ：类似 int32，表示一个 Unicode 码点
        3. uintptr：无符号整型，用于存放一个指针
#### 引用数据类型



### 三、Go中*和&区别
   1. & 是取地址符号 , 即取得某个变量的地址 , 如 ; &a
   2. *是指针运算符 , 可以表示一个变量是指针类型 , 也可以表示一个指针变量所指向的存储单元 , 也就是这个地址所存储的值
```
package main

    import(
        "fmt"
    )


    func modify(a *int) {   // "*" 表示定义一个变量是指针类型, 这个变量叫指针变量
        *a = 10             // "*" 表示获取指针变量所指向的内存
    }

    func main() {
        a := 1
        var x *int    //定义指针变量
        x = &a        // &表示获取变量的地址
        modify(x)
        fmt.Println(a)   // 10
    }
```


# 第一部分：程序结构
### 一、Go的大小写问题
go中根据首字母的大小写来确定可以访问的权限。
无论是方法名、常量、变量名还是结构体的名称，如果首字母大写，则可以被其他的包访问；
如果首字母小写，则只能在本包中使用。
可以粗暴的理解为首字母大写是公有的，首字母小写是私有的。
类属性如果是小写开头，则其序列化会丢失属性对应的值，同时也无法进行Json解析。
### 二、声明
  
  声明语句定义了程序的各种实体对象以及部分或全部的属性。  
    
  Go语言主要有四种类型的声明语句：var、const、type和func，分别对应变量、常量、类型和函数实体对象的声明
  1. 变量的声明语法
  ```
  var 变量名字 类型 = 表达式
  ```
  2. 简短变量声明（自动推导类型）
   ```
   freq := rand.Float64() * 3.0
   ```
  3. 指针

### 三、指针
  
  一个变量对应一个保存了变量对应类型值的内存空间。
  
  一个指针的值是另一个变量的地址

  并不是每一个值都会有一个内存地址，但是对于每一个变量必然有对应的内存地址。通过指针，我们可以直接读或更新对应变量的值，而不需要知道该变量的名字（如果变量有名字的话）。

  如果指针名字为p，那么可以说“p指针指向变量x”，或者说“p指针保存了x变量的内存地址”。同时*p表达式对应p指针指向的变量的值。一般*p表达式读取指针指向的变量的值，这里为int类型的值，同时因为*p对应一个变量，所以该表达式也可以出现在赋值语句的左边，表示更新指针所指向的变量的值
  ```
    x := 1
    p := &x         // p, of type *int, points to x
    fmt.Println(*p) // "1"
    *p = 2          // equivalent to x = 2
    fmt.Println(x)  // "2"
  ```

  任何类型的指针的零值都是nil。如果p指向某个有效变量，那么p != nil测试为真。指针之间也是可以进行相等测试的，只有当它们指向同一个变量或全部是nil时才相等。
  ```
    var x, y int
    fmt.Println(&x == &x, &x == &y, &x == nil) // "true false false"
  ```
### 四、new函数
  
  另一个创建变量的方法是调用用内建的new函数。表达式new(T)将创建一个T类型的匿名变量，初始化为T类型的零值，然后返回变量地址，返回的指针类型为*T。
  ```
    p := new(int)   // p, *int 类型, 指向匿名的 int 变量
    fmt.Println(*p) // "0"
    *p = 2          // 设置 int 匿名变量的值为 2
    fmt.Println(*p) // "2"
  ```
  一般的：

  每次调用new函数都是返回一个新的变量的地址
  ```
    p := new(int)
    q := new(int)
    fmt.Println(p == q) // "false"
  ```
  当然也可能有特殊情况：如果两个类型都是空的，也就是说类型的大小是0，例如struct{}和 [0]int, 有可能有相同的地址（依赖具体的语言实现）
### 五、变量的声明周期
变量的生命周期指的是在程序运行期间变量有效存在的时间间隔。

对于成员变量和局部变量：

成员变量：生命周期和整个程序的运行周期是一致的

局部变量：局部变量的声明周期则是动态的。它们在函数每次被调用的时候创建。函数的参数变量和返回值变量都是局部变量。




### 包和工具

# 函数
### 1、函数的定义
```
func (recv receiver_type) methodName(parameter_list) (return_value_list) { 
    ... 
}
```
# 方法
# 接口
# 并发 gorounters 和 channel
# 测试
# 反射





# 其他：
### 1、gorm 报错：using unaddressable value
```
func GetByDictType(dictType int) (blogDictList []*models.BlogDict) {
	fmt.Println(dictType)
	db := core.GetDb()
    //使用&传递指针
	db.Where(" dict_type = ?", dictType).Find(&blogDictList)
	return blogDictList
}
```
### 2、golang 中string和int类型相互转换
```
golang中字符串和各种int类型之间的相互转换方式：

string转成int：
int, err := strconv.Atoi(string)
string转成int64：
int64, err := strconv.ParseInt(string, 10, 64)
int转成string：
string := strconv.Itoa(int)
int64转成string：
string := strconv.FormatInt(int64,10)

```

