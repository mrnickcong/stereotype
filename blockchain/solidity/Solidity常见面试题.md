# Solidity常见面试题

## 基础部分

## Solidity

### 问：Solidity是静态类型的还是动态类型的语言？

答：它是静态类型语言，这意味着类型在编译时是已知的。

### 问：Solidity中与Java“Class”类似的是什么？

答：合约。

### 问：什么是合约实例？

答：合约实例是区块链上已部署的合约。

### 问：请说出Java和Solidity之间的一些区别

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

答：还可以使用HTTP协议导入其他合约文件，例如从Github导入：import “http://github.com/owner/repo/path_to_file”;

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
函数修改器（Modifier）类似AOP中的拦截器，提供了修改函数执行流程的机会，一般用来做验证和检查。其中“_”用来将控制流返还给被修改的函数
几个重要的修改器：


payable，接收以太的函数必需加上


view或pure，表示函数不会改变以太坊状态


事件提供了让外部应用了解合约状态变化的途径，一般使用流程是：


合约内部发出事件


外部应用利用web3监听事件


可见性：


external，仅适用于函数，表示其可被外部合约或交易调用，但不能被内部调用。


public
函数缺省的可见性，可被内部调用和通过消息调用，
状态变量，EVM会为其自动产生getter


internal，函数和状态变量可被当前合约和其子合约调用
状态变量的缺省可见性


private，函数和状态变量仅被当前合约调用


合约支持多重继承。
EVM提供了4种数据位置用来存放数据：


storage，持久化，存储于整个以太坊


memory，函数的本地内存，非持久化


calldata，函数入参，非持久化


stack，EVM的调用栈


规则：


状态变量：storage


external函数入参：calldata


函数入参：memory


函数局部变量：
引用类型，缺省为storage，但可被覆盖
值类型，memory，不可被覆盖
mapping类型，指向外部的状态变量


状态变量之间赋值，将产生独立副本，即相互更改不受引用。


storage和memory变量之间相互赋值，总产生独立副本。


memory变量之间赋值
值类型，产生独立副本
引用类型，指向同一地址


由于合约执行是有成本的，需要警惕循环语句。
对于多重继承的合约，需要明确指明顺序，如：
contract X {}contract A is X {}contract C is A, X {}
复制代码fallback函数没有函数名，无法直接调用，但在两个情况下会被触发：


合约中无任何函数匹配调用者发过来的请求时


合约接收以太时，此时，fallback函数需使用payable


由于其无法被外部调用，EVM限制其只能最多消耗2300的gas，若超过，则fallback函数失败。因此，记得要测试合约的fallback函数是否会超过这个限制。
并且，fallback是安全事故的高发地，需要对其进行必要的安全相关的测试。
接口和抽象合约跟Java中的接口和抽象类差别不大，库（library）是一段可复用的代码，在调用它的合约上下文内执行：


它不能用状态变量


不能继承或被继承


不能接收以太


合约抛出异常之后，状态回滚，当前有3种方式：


require(表达式)，若表达式为false，则抛出异常，未使用的gas退回


适合验证函数的入参


assert(表达式)，同上，但未使用的gas不会退回，将全部被消耗


适合验证内部状态


revert()，直接抛出异常





常见模式
鉴于以太坊应用的以下特点，编写solidity代码时需要非常小心：


执行消耗真金白银


合约公开可见，即使是private


合约不可篡改，一旦发布无法变更


常见的编码套路有：


对于支付，优先采用“取款”，而不是“转账”（即send或transfer），避免接收合约恶意fallback函数。


对于支付，采用CDI模式，避免重入问题。即：
检查 -> 更改本合约状态 ->支付。


善用Modifier进行权限控制。


使用mapping类型保存合约数据，甚至为了方便升级，单独分离出两类：
数据合约，仅包含mapping，保留操作mapping的函数，客观上类似数据表。
控制合约，仅包含逻辑控制，通过数据合约的接口操作数据。若逻辑有问题，只需升级本合约即可，数据仍然得以保留。


使用代理合约，参见这里（https://blog.zeppelin.solutions/proxy-libraries-in-solidity-79fbe4b970fd）。


最后，也是最省事的方式：使用成熟类库，如OpenZeppelin。关于Solidity的好东西，可以通过其Awesome List（https://github.com/bkrem/awesome-solidity）来了解。

作者：区块链社区HiBlock
链接：https://juejin.cn/post/6844903641480953869
来源：稀土掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。