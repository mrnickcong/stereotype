# Go语言面试题

## 第一、基础部分

### Go语言make和new关键字的区别及实现原理

    Go语言中的 new 和 make 主要区别如下：
    * make 只能用来分配及初始化类型为 slice、map、chan 的数据。new 可以分配任意类型的数据；
    * new 分配返回的是指针，即类型 *Type。make 返回引用，即 Type；
    * new 分配的空间被清零。make 分配空间后，会进行初始化；

    new 只分配内存，而 make 只能用于 slice、map 和 channel 的初始化

    1. new
        在Go语言中，new 函数描述如下：
        
        // The new built-in function allocates memory. The first argument is a type,
        // not a value, and the value returned is a pointer to a newly
        // allocated zero value of that type.
        func new(Type) *Type
        
        从上面的代码可以看出，new 函数只接受一个参数，这个参数是一个类型，并且返回一个指向该类型内存地址的指针。
        同时 new 函数会把分配的内存置为零，也就是类型的零值。
        new 函数，它返回的永远是类型的指针，指针指向分配类型的内存地址。
        
    2. sdf
       make 也是用于内存分配的，但是和 new 不同，它只用于 chan、map 以及 slice 的内存创建，而且它返回的类型就是这三个类型本身，而不是他们的指针类型，
       因为这三种类型就是引用类型，所以就没有必要返回他们的指针了。

       make 函数只用于 map，slice 和 channel，并且不返回指针。
       如果想要获得一个显式的指针，可以使用 new 函数进行分配，或者显式地使用一个变量的地址。







    

channel是同步的还是异步的？

## 第二、Go并发编程

