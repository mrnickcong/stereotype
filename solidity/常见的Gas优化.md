# 常见的Gas优化

避免使用 public
如果某个接口既需要被外部访问，也需要被相同合约内的其它接口访问，那么接口实现可以这样：

// good
function f0() external {
	_f();
}
function _f() internal {}

// bad, expensive
function f0() public {}
访问 internal 方法只需要 JUMP 到对应的 OP 就可以，参数通过栈上的内存地址来传递。但是访问 pubic 方法需要进行参数的内存拷贝，如果参数较大带来的 gas 开销就比较高。

变量不需要进行初始化
solidity 中的变量都有默认值，因此初始化会浪费一点 gas。

uint256 hello = 0; //bad, expensive
uint256 world; //good, cheap
使用++i 而不是 i++
++i 是预递增操作，只需要两个步骤：

增加 i 的值
返回 i 的值
i++是后递增操作，需要 4 个步骤：

保存 i 的旧值
递增 i 的值，并存放到一个内存临时变量
返回 i 的旧值
将内存临时变量赋给 i

减少对外部合约地址的调用

Storage 优化

减少 Slot 的数量
在合约中对 Storage 的读写永远是 gas 消耗的大头，Storage 优化的思路之一就是尽可能减少 slot 的数量，比如

用一个 slot 尽可能的表示多个变量
将多个可以共享一个 slot 的变量紧挨在一起
每个 slot 都是 32 个字节（256 位），可以存放 8 个 uint32，而 uint32 常常用来表示时间戳（最大可以到 2106 年）。

减少 Slot 的读写
另一个优化思路是要减少 slot 的读写频率，比如一段业务逻辑中需要频繁的读一个 storage 变量，可以先将这个 storage 变量载入内存，内存的读写是比较便宜的。