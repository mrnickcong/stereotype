# Solidity常见面试题

## 基础部分

## Solidity

### 问：Solidity是静态类型的还是动态类型的语言？

答：它是静态类型语言，这意味着类型在编译时是已知的。

### 问：Solidity中与Java“Class”类似的是什么？

答：合约。

### 问：什么是合约实例？

答：合约实例是区块链上已部署的合约。

### 问：请说出Java和Solidity之间的一些区别。

答：Solidity支持多重继承，但不支持重载。

### 问：你必须在Solidity文件中指定的第一件事是什么？

答：Solidity编译器的版本，比如指定为^ 0.4.8。 这是必要的，因为这样可以防止在使用其他版本的编译器时引入不兼容性错误。

### 问：一个合约由什么组成？

答：主要构成元素有：存储变量（storage variables）、函数（functions）、事件（events）。

### 问：合约中有哪些类型的函数？

答：有构造函数、fallback函数、修改合约状态的函数和只读的constant函数。

### 问：如果我将多个合约定义放入单个Solidity文件中，我会得到什么错误？

答：将多个合约定义放入单个Solidity文件是完全正确的。

### 问：两个合约之间交互的方式有哪些？

答：一个合约可以调用另一个合约，也可以继承其他合约。

### 问：当你尝试使用部署一个包含多个合约的文件时会发生什么？

答：编译器只会部署该文件中的最后一个合约，而忽略所有其他合约。

### 问：如果我有一个大项目，我需要将所有相关的合约保存到一个文件中吗？

答：不需要。可以使用import语句导入其他合约文件，例如import “./MyOtherContracts.sol”;。

### 问：我只能导入本地合约文件吗？

答：还可以使用HTTP协议导入其他合约文件，例如从Github导入：import “http://github.com/owner/repo/path_to_file”;。

### 问：EVM的内存分成了哪些部分？

答：它分为存储（Storage）、内存（Memory）和回调数据（Calldata）。

- 请解释一下Storage
答：可以把它想象成一个数据库。 每个合约管理自己的Storage变量。 它是一个键-值数据库（256位键值）。
就每次执行使用的gas而言，在Storage上读取和写入的成本更高。
- 请解释一下Memory
答：这是一个临时存储区。 一旦执行结束，数据就会丢失。 可以在Memory上分配像数组和结构这样复杂的数据类型。
- 问：请解释一下Calldata 
答：可以把calldata视为一个调用堆栈。 它是临时的、不可修改的，用来存储EVM的执行数据。

### 问：哪些变量存储在Storage，那些变量存储在Memory？

答：状态变量和局部变量（它们是对状态变量的引用）存储在Storage区域， 函数参数位于Memory区域。

### 问：看看下面的代码，并解释代码的哪一部分对应于哪个内存区域

```
contract MyContract {
// part 1
uint count;
uint[] totalPoints;

function localVars(){
// part 2
uint[] localArr;
// part 3
uint[] memory memoryArr;
// part 4
uint[] pointer = totalPoints;
}
}
```

答：
第1部分 - Storage
第2部分 - Storage
第3部分 - Memory
第4部分 - Storage

### 问：这样做对吗

```
function doSomething(uint[] storage args) internal returns(uint[] storage data) {…}
```
答：可以，可以强制将函数的参数设置为Storage存储。 在这种情况下，如果没有传递存储引用，编译器会报错。

### 问：EVM调用和非EVM调用有什么区别？
答：EVM调用是对智能合约的方法的调用。它会触发智能合约方法的执行，并需要消耗Gas。非EVM调用是对公共的值的读取。它不需要消耗Gas。


### modifier