

# 区块链基础知识

下面是一个简单的智能合约

```go
// SPDX-License-Identifier: GPL-3.0   //源代码是根据GPL 3.0版本授权的
pragma solidity ^0.4.0;           //Solidity版本0.4.0以上，最高到0.5.0，但是不包含0.5.0
pragma solidity >=0.4.16 <0.9.0;  //告诉编译器源代码所适用的Solidity版本为>=0.4.16 及 <0.9.0
contract SimpleStorage {

    // 关键字“public”让这些变量可以从外部读取
    address public minter;
    mapping (address => uint) public balances;

    // 轻客户端可以通过事件针对变化作出高效的反应
    event Sent(address from, address to, uint amount);

    // 这是构造函数，只有当合约创建时运行
    constructor() {
        minter = msg.sender;
    }

    function mint(address receiver, uint amount) public {
        require(msg.sender == minter);
        balances[receiver] += amount;
    }

    // Errors allow you to provide information about
    // why an operation failed. They are returned
    // to the caller of the function.
    error InsufficientBalance(uint requested, uint available);


    function send(address receiver, uint amount) public {
        if (amount > balances[msg.sender])
            revert InsufficientBalance({
                requested: amount,
                available: balances[msg.sender]
            });

        balances[msg.sender] -= amount;
        balances[receiver] += amount;
        emit Sent(msg.sender, receiver, amount);
    }
    
}
```

## 代码解读

### address

`address public minter;`

这一行声明了一个可以被公开访问的 `address` 类型的状态变量。

`address` 类型是一个160位的值，且不允许任何算数操作。这种类型适合存储合约地址或外部人员的密钥对。

关键字 `public` 自动生成一个函数，允许你在这个合约之外访问这个状态变量的当前值。

如果没有这个关键字，其他的合约没有办法访问这个变量。

由编译器生成的函数的代码大致如下所示（暂时忽略 external 和 view）：

```go
    function minter() external view returns (address) { return minter; }
```

### event

`event Sent(address from, address to, uint amount);`

这行声明了一个所谓的“事件（event）”，它会在 `send` 函数的最后一行被发出。用户界面（当然也包括服务器应用程序）可以监听区块链上正在发送的事件，而不会花费太多成本。一旦它被发出，监听该事件的listener都将收到通知。而所有的事件都包含了 `from` ， `to` 和 `amount` 三个参数，可方便追踪交易。

为了监听这个事件，你可以使用如下JavaScript代码， Coin 是通过 [web3.js 创建的合约对象](https://learnblockchain.cn/docs/web3.js/web3-eth-contract.html) ， :

```go
Coin.Sent().watch({}, '', function(error, result) {
    if (!error) {
        console.log("Coin transfer: " + result.args.amount +
            " coins were sent from " + result.args.from +
            " to " + result.args.to + ".");
        console.log("Balances now:\n" +
            "Sender: " + Coin.balances.call(result.args.from) +
            "Receiver: " + Coin.balances.call(result.args.to));
    }
})
```

### error

[Errors](https://learnblockchain.cn/docs/solidity/contracts.html#errors) 用来向调用者描述错误信息。Error与 [revert 语句](https://learnblockchain.cn/docs/solidity/control-structures.html#revert-statement) 一起使用。 `revert` 语句无条件地中止执行并回退所有的变化，类似于 `require` 函数，它也同样允许你提供一个错误的名称和额外的数据，这些额外数据将提供给调用者(并最终提供给前端应用程序或区块资源管理器），这样就可以更容易地调试或应对失败。

任何人（已经拥有一些代币）都可以使用 `send` 函数来向其他人发送代币。如果发送者没有足够的代币可以发送， `if` 条件为真 `revert` 将触发失败，并通过 `InsufficientBalance` 向发送者提供错误细节。

## 区块链基础

对于程序员来说，区块链这个概念并不难理解，这是因为大多数难懂的东西 (挖矿, [哈希](https://en.wikipedia.org/wiki/Cryptographic_hash_function) ，[椭圆曲线密码学](https://en.wikipedia.org/wiki/Elliptic_curve_cryptography) ，[点对点网络（P2P）](https://en.wikipedia.org/wiki/Peer-to-peer) 等) 都只是用于提供特定的功能和承诺。你只需接受这些既有的特性功能，不必关心底层技术，比如，难道你必须知道亚马逊的 AWS 内部原理，你才能使用它吗？

### 交易/事务

区块链是全球共享的事务性数据库，这意味着每个人都可加入网络来阅读数据库中的记录。如果你想改变数据库中的某些东西，你必须创建一个被所有其他人所接受的事务。事务一词意味着你想做的（假设您想要同时更改两个值），要么一点没做，要么全部完成。此外，当你的事务被应用到数据库时，其他事务不能修改数据库。

举个例子，设想一张表，列出电子货币中所有账户的余额。如果请求从一个账户转移到另一个账户，数据库的事务特性确保了如果从一个账户扣除金额，它总被添加到另一个账户。如果由于某些原因，无法添加金额到目标账户时，源账户也不会发生任何变化。

此外，交易总是由发送人（创建者）签名。

这样，就可非常简单地为数据库的特定修改增加访问保护机制。在电子货币的例子中，一个简单的检查可以确保只有持有账户密钥的人才能从中转账。

### 区块

在比特币中，要解决的一个主要难题，被称为“双花攻击 (double-spend attack)”：如果网络存在两笔交易，都想花光同一个账户的钱时（即所谓的冲突）会发生什么情况？交易互相冲突？

简单的回答是你不必在乎此问题。网络会为你自动选择一条交易序列，并打包到所谓的“区块”中，然后它们将在所有参与节点中执行和分发。如果两笔交易互相矛盾，那么最终被确认为后发生的交易将被拒绝，不会被包含到区块中。

这些块按时间形成了一个线性序列，这正是“区块链”这个词的来源。区块以一定的时间间隔添加到链上 —— 对于以太坊，这间隔大约是17秒。

作为“顺序选择机制”（也就是所谓的“挖矿”）的一部分，可能有时会发生块（blocks）被回滚的情况，但仅在链的“末端”。末端增加的块越多，其发生回滚的概率越小。因此你的交易被回滚甚至从区块链中抹除，这是可能的，但等待的时间越长，这种情况发生的概率就越小。

注解

不能保证交易会包含在下一个区块或任何特定的未来区块中，因为这不是由交易的提交者决定，而是由矿工决定将交易包含在哪个区块中。

如果你要安排合约的未来的时间点调用，可以使用合约自动化工具或类似的oracle服务。





# 以太坊虚拟机

### 概述

以太坊虚拟机 EVM 是智能合约的运行环境。它不仅是沙盒封装的，而且是完全隔离的，也就是说在 EVM 中运行代码是无法访问网络、文件系统和其他进程的。甚至智能合约之间的访问也是受限的。


### 账户

以太坊中有两类账户（它们共用同一个地址空间）： **外部账户** 由公钥-私钥对（也就是人）控制； **合约账户** 由和账户一起存储的代码控制.

外部账户的地址是由公钥决定的，而合约账户的地址是在创建该合约时确定的（这个地址通过合约创建者的地址和从该地址发出过的交易数量计算得到的，也就是所谓的“nonce”）

无论帐户是否存储代码，这两类账户对 EVM 来说是一样的。

每个账户都有一个键值对形式的持久化存储。其中 key 和 value 的长度都是256位，我们称之为 **存储** 。

此外，每个账户有一个以太币余额（ **balance** ）（单位是“Wei”, `1 ether` 是 `10**18 wei`），余额会因为发送包含以太币的交易而改变。

### 交易

交易可以看作是从一个帐户发送到另一个帐户的消息（这里的账户，可能是相同的或特殊的零帐户，请参阅下文）。它能包含一个二进制数据（合约负载）和以太币。

如果目标账户含有代码，此代码会被执行，并以 payload 作为入参。

如果目标账户是零账户（账户地址为 `0` )，此交易将创建一个 **新合约** 。 如前文所述，合约的地址不是零地址，而是通过合约创建者的地址和从该地址发出过的交易数量计算得到的（所谓的“nonce”）。 这个用来创建合约的交易的 payload 会被转换为 EVM 字节码并执行。执行的输出将作为合约代码被永久存储。这意味着，为创建一个合约，你不需要发送实际的合约代码，而是发送能够产生合约代码的代码。

注解

在合约创建的过程中，它的代码还是空的。所以直到构造函数执行结束，你都不应该在其中调用合约自己函数。

### Gas

一经创建，每笔交易都收取一定数量的 **gas** ，必须由原始交易发起人（ `tx.orgin` ）支付。 EVM 执行交易时，gas 将按特定规则逐渐耗尽。 无论执行到什么位置，一旦 gas 被耗尽（比如降为负值），将会触发一个 out-of-gas 异常。当前调用帧（call frame）所做的所有状态修改都将被回滚。

Gas机制激励了对EVM执行时间的经济使用，同时也补偿了 EVM 执行者（即矿工）的工作。 由于每个区块有一个最大的Gas数量(区块 gas limit)，它也限制了验证一个区块所需的工作量。

**gas price** 是交易发送者设置的一个值，发送者账户需要预付的手续费= `gas_price * gas` 。如果交易执行后还有剩余， gas 会原路返还。 如果出现异常（exception），回退交易，已经用完的Gas就不会被退还。

由于EVM执行者可以选择是否包括交易。交易发送者不能通过设置一个低的Gas价格来滥用系统。

### 存储，内存和栈

以太坊虚拟机有 3 个区域用来存储数据： 存储（storage）, 内存（memory） 和 栈（stack）.

每个账户有一块持久化内存区称为 **存储** 。 存储是将256位字映射到256位字的键值存储区。 在合约中枚举存储是不可能的，且读存储的相对开销很高，修改存储的开销甚至更高。合约只能读写存储区内属于自己的部分。

第二个内存区称为 **内存** ，合约会试图为每一次消息调用获取一块被重新擦拭干净的内存实例。 内存是线性的，可按字节级寻址，但读的长度被限制为256位，而写的长度可以是8位或256位。当访问（无论是读还是写）之前从未访问过的内存字（word）时（无论是偏移到该字内的任何位置），内存将按字进行扩展（每个字是256位）。扩容也将消耗一定的gas。 随着内存使用量的增长，其费用也会增高（以平方级别）。

EVM 不是基于寄存器的，而是基于栈的，因此所有的计算都在一个被称为 **栈（stack）** 的区域执行。 	，每个元素长度是一个字（256位）。对栈的访问只限于其顶端，限制方式为：允许拷贝最顶端的16个元素中的一个到栈顶，或者是交换栈顶元素和下面16个元素中的一个。所有其他操作都只能取最顶的两个（或一个，或更多，取决于具体的操作）元素，运算后，把结果压入栈顶。当然可以把栈上的元素放到存储或内存中。但是无法只访问栈上指定深度的那个元素，除非先从栈顶移除其他元素。

### 指令集

EVM的指令集量应尽量少，以最大限度地避免可能导致共识问题的错误实现。所有的指令都是针对”256位的字（word）”这个基本的数据类型来进行操作。具备常用的算术、位、逻辑和比较操作。也可以做到有条件和无条件跳转。此外，合约可以访问当前区块的相关属性，比如它的编号和时间戳。

### 消息调用

合约可以通过消息调用的方式来调用其它合约或者发送以太币到非合约账户。消息调用和交易非常类似，它们都有一个源、目标、数据、以太币、gas和返回数据。事实上每个交易都由一个顶层消息调用组成，这个消息调用又可创建更多的消息调用。

合约可以决定在其内部的消息调用中，对于剩余的 **gas** ，应发送和保留多少。如果在内部消息调用时发生了out-of-gas异常（或其他任何异常），这将由一个被压入栈顶的错误值所指明。此时，只有与该内部消息调用一起发送的gas会被消耗掉。并且，Solidity中，发起调用的合约默认会触发一个手工的异常，以便异常可以从调用栈里“冒泡出来”。 如前文所述，被调用的合约（可以和调用者是同一个合约）会获得一块刚刚清空过的内存，并可以访问调用的payload——由被称为 calldata 的独立区域所提供的数据。调用执行结束后，返回数据将被存放在调用方预先分配好的一块内存中。 调用深度被 **限制** 为 1024 ，因此对于更加复杂的操作，我们应使用循环而不是递归。

### 委托调用/代码调用和库

有一种特殊类型的消息调用，被称为 **委托调用(delegatecall)** 。它和一般的消息调用的区别在于，目标地址的代码将在发起调用的合约的上下文中执行，并且 `msg.sender` 和 `msg.value` 不变。 这意味着一个合约可以在运行时从另外一个地址动态加载代码。存储、当前地址和余额都指向发起调用的合约，只有代码是从被调用地址获取的。 这使得 Solidity 可以实现”库“能力：可复用的代码库可以放在一个合约的存储上，如用来实现复杂的数据结构的库。

### 日志

有一种特殊的可索引的数据结构，其存储的数据可以一路映射直到区块层级。这个特性被称为 **日志(logs)** ，Solidity用它来实现 **事件(events)** 。

合约创建之后就无法访问日志数据，但是这些数据可以从区块链外高效的访问。因为部分日志数据被存储在 [布隆过滤器（Bloom filter)](https://en.wikipedia.org/wiki/Bloom_filter) 中，我们可以高效并且加密安全地搜索日志，所以那些没有下载整个区块链的网络节点（轻客户端）也可以找到这些日志。


### 合约创建

合约甚至可以通过一个特殊的指令来创建其他合约（不是简单的调用零地址）。创建合约的调用 **create calls** 和普通消息调用的唯一区别在于，负载会被执行，执行的结果被存储为合约代码，调用者/创建者在栈上得到新合约的地址。

### 失效和自毁

合约代码从区块链上移除的唯一方式是合约在合约地址上的执行自毁操作 `selfdestruct` 。合约账户上剩余的以太币会发送给指定的目标，然后其存储和代码从状态中被移除。移除一个合约听上去不错，但其实有潜在的危险，如果有人发送以太币到移除的合约，这些以太币将永远丢失。

警告

即使一个合约被 `selfdestruct` 删除，它仍然是区块链历史的一部分，可能被大多数以太坊节点保留。 因此，使用 `selfdestruct` 与从硬盘上删除数据是不同的。

注解

即便一个合约的代码中没有显式地调用 `selfdestruct` ，它仍然有可能通过 `delegatecall` 或 `callcode` 执行自毁操作。

如果要禁用合约，可以通过修改某个内部状态让所有函数无法执行，而是直接回退，这样也可以达到返还以太的目的。

### 预编译合约

有一小部分合约地址是特殊的。 在 `1` 和（包括） `8` 之间的地址范围包含了 “预编译的合约（precompiled contract）”，他们可以像其他合约一样被调用 但是他们的行为（和他们的Gas消耗）并不是被存储在该地址的EVM代码所定义(预编译合约它们不包含代码)。 而是在EVM执行环境本身中实现的。

不同的EVM兼容链可能使用一组不同的预编译的合约。也有可能是新的预编译合约在未来被添加到Ethereum主链中。 但你可以合理地期望它们总是在 `1` 和 [``](https://learnblockchain.cn/docs/solidity/introduction-to-smart-contracts.html#id22)0xffff``（包括）地址范围内。



# 语言描述

## 合约的创建

### 外部创建

### 内部创建

## 合约的结构

每个合约中可以包含 [状态变量](https://learnblockchain.cn/docs/solidity/structure-of-a-contract.html#structure-state-variables)、 [函数](https://learnblockchain.cn/docs/solidity/structure-of-a-contract.html#structure-functions)、 [函数 ](https://learnblockchain.cn/docs/solidity/structure-of-a-contract.html#structure-function-modifiers), [事件 Event](https://learnblockchain.cn/docs/solidity/structure-of-a-contract.html#structure-events), [错误(Errors)](https://learnblockchain.cn/docs/solidity/structure-of-a-contract.html#structure-errors), [结构体](https://learnblockchain.cn/docs/solidity/structure-of-a-contract.html#structure-struct-types) 和 [枚举类型](https://learnblockchain.cn/docs/solidity/structure-of-a-contract.html#structure-enum-types) 的声明，且合约可以从其他合约继承。

还有一些特殊的合约，如： [库](https://learnblockchain.cn/docs/solidity/contracts.html#libraries) 和 [接口](https://learnblockchain.cn/docs/solidity/contracts.html#interfaces).

| 合约构成单元 | 名词解释                                                     | 备注                                              |
| :----------: | ------------------------------------------------------------ | ------------------------------------------------- |
|   状态变量   | 合约的数据，状态变量是永久地存储在合约存储中的值             |                                                   |
|     函数     | 合约的行为                                                   |                                                   |
|  函数修饰器  | 可以用来以声明的方式修改函数语义。                           | 相当于Java中AOP。多用拦截函数的访问控制、校验等   |
|  事件event   | 事件是能方便地调用以太坊虚拟机日志功能的接口                 | 日志，是一种数据支持化存储                        |
|   构造函数   | 合约初始化的时候会调用，即部署的时候                         |                                                   |
|    结构体    | 结构体是可以将几个变量分组的自定义类型                       | `struct Voter {  uint weight; }`// 结构体         |
|   枚举类型   | 枚举可用来创建由一定数量的“常量值”构成的自定义类型           | `enum State { Created, Locked, InValid } // 枚举` |
|     异常     | Solidity 为应对失败，允许用户定义 `error` 来描述错误的名称和数据。 错误可以在 [revert statements](https://learnblockchain.cn/docs/solidity/control-structures.html#revert-statement) 中使用，跟用错误字符串相比， `error` 更便宜并且允许你编码额外的数据，还可以用 NatSpec 为用户去描述错误 |                                                   |

```go
以下代码remix编译通过
// SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.4;
// 没有足够的资金用于转账， 参数 `requested` 表示需要的资金，`available` 表示仅有的资金。
error NotEnoughFunds(uint requested, uint available);
contract Token {
    mapping(address => uint) balances;
    function transfer(address to, uint amount) public {
        uint balance = balances[msg.sender];
        if (balance < amount)
            revert NotEnoughFunds(amount, balance);
        balances[msg.sender] -= amount;
        balances[to] += amount;
        // ...
    }
}
```

## 类型

### 值类型

##### 值类型汇总

| 类型名称     | 关键字                                                      | 举例 | 备注                                   |
| ------------ | ----------------------------------------------------------- | ---- | -------------------------------------- |
| 布尔类型     | bool                                                        |      | 字面常量值：true和false，默认值：false |
| 整形         | 有符号整形：int8、int16、int32、int64、int128、int256       |      | int别名：int256。默认值：0             |
|              | 无符号整形：uint8、uint16、uint32、uint64、uint128、uint256 |      | uint的别名：uint256。默认值：0         |
| 定长字节数组 | bytes1、bytes2、bytes3、bytes4、........ bytes32            |      |                                        |
|              |                                                             |      |                                        |
|              |                                                             |      |                                        |

##### 特殊值类型

特殊值类型：address、合约类型、枚举类型

###### 枚举类型

举例：`enum ActionChoices { GoLeft, GoRight, GoStraight, SitStill }`

枚举需要至少一个成员,默认值是第一个成员，枚举不能多于 256 个成员。

数据表示与C中的枚举相同：选项从“0”开始的无符号整数值表示。

使用 `type(NameOfEnum).min` 和 `type(NameOfEnum).max` 你可以得到给定枚举的最小值和最大值。

对于 `enum` 类型, 默认值是第一个成员。

###### 函数类型

举例：

`function (<parameter types>) {internal|external} [pure|constant|view|payable] [returns (<return types>)]`

### 引用类型

#### 数组

举例：`uint[][5] memory x`

bytes和string类型的变量是特殊的数组，不允许使用 长度或者索引来访问，在calldata和memory中会连续存储，不是每32字节一个单元的存储方式。

其中bytes类似于：bytes1[]

#### 数组切片

数组切片是数组连续部分的视图，用法如：`x[start:end]` ， `start` 和 `end` 是 uint256 类型（或结果为 uint256 的表达式）。 `x[start:end]` 的第一个元素是 `x[start]` ， 最后一个元素是 `x[end - 1]` 。

`start` 和 `end` 都可以是可选的： `start` 默认是 0， 而 `end` 默认是数组长度。

目前数组切片，仅可使用于 calldata 数组.

```go
    function forward(bytes calldata payload) external {
        bytes4 sig = bytes4(payload[:4]);
    }
```



#### 结构体

1. 

#### 映射

映射类型在声明时的形式为 `mapping(KeyType => ValueType)`。

- 其中 `KeyType` 可以是任何基本类型，即可以是任何的内建类型， 可以是任务基本类型，包括bytes和string，但不包括自定义类型，比如：合约、枚举、映射、结构体
- 即除 `bytes` 和 `string` 之外的数组类型是不可以作为 `KeyType` 的类型的。

`ValueType` 可以是包括映射类型在内的任何类型。

映射只能是 存储storage 的数据位置，因此只允许作为状态变量 或 作为函数内的 存储storage 引用 或 作为库函数的参数。 它们不能用合约公有函数的参数或返回值。

这些限制同样适用于包含映射的数组和结构体。

```go
//声明方式举例
mapping(address => uint) public balances;

//嵌套映射
mapping (address => mapping (address => uint256)) private _allowances;
//ERC20中，_allowances 用来记录其他的账号，可以允许从其账号使用多少数量的币

    function approve(address spender, uint256 amount) public returns (bool) {
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }
```

映射本身是无法遍历的，即无法枚举所有的键

### 操作符

#### 	一般操作符

算术运算符

逻辑运算符

三元运算符是一个表达是形式： `<expression> ? <trueExpression> : <falseExpression>` 

#### 	delete操作符

​				delete a` 的结果是将 `a` 类型初始值赋值给 `a

​						对于整型变量来说，相当于 `a = 0`

​						对于静态数组来说，是将数组中的所有元素重置为初始值

​						对于动态数组来说，是将重置为数组长度为0的数组

​						对数组而言， `delete a[x]` 仅删除数组索引 `x` 处的元素，其他的元素和长度不变，这以为着数组中留出了一个空位

​						如果对象 `a` 是结构体，则将结构体中的所有属性(成员)重置。

​			但是：

​					`delete` 对整个映射是无效的。因为映射的键可以是任意的，通常也是未知的

​					因此在你删除一个结构体时，结果将重置所有的非映射属性（成员），这个过程是递归进行的。

​				当 `a` 是引用变量时，我们可以看到这个区别， `delete a` 它只会重置 `a` 本身，而不是更改它之前引用的值。

```go
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.4.0 <0.9.0;

contract DeleteLBC {
    uint data;
    uint[] dataArray;

    function f() public {
        uint x = data;
        delete x; // 将 x 设为 0，并不影响数据
        delete data; // 将 data 设为 0，并不影响 x，因为它仍然有个副本
        uint[] storage y = dataArray;
        delete dataArray;
        // 将 dataArray.length 设为 0，但由于 uint[] 是一个复杂的对象，y 也将受到影响，
        // 因为它是一个存储位置是 storage 的对象的别名。
        // 另一方面："delete y" 是非法的，引用了 storage 对象的局部变量只能由已有的 storage 对象赋值。
        assert(y.length == 0);
    }
}
```

### 类型转换
#### 隐士转转

```go
uint8 y;
uint16 z;
uint32 x = y + z;
```



#### 显示转换

```go
int8 y = -3;
uint x = uint(y);
```



#### 基本类型之间的转换

#### 基本类型与字面常量的转转

#### 类型转换注意事项

只有 `bytes20` 和 `uint160` 允许显式转换为 `address` 类型

### 值类型与引用类型的区别

1. Solidity 是一种静态类型语言，这意味着每个变量（状态变量和局部变量）都需要在编译时指定变量的类型。
2. “undefined”或“null”值的概念在Solidity中不存在，但是新声明的变量总是有一个 [默认值](https://learnblockchain.cn/docs/solidity/control-structures.html#default-value) ，具体的默认值跟类型相关。 要处理任何意外的值，应该使用 [错误处理](https://learnblockchain.cn/docs/solidity/control-structures.html#assert-and-require) 来恢复整个交易，或者返回一个带有第二个 `bool` 值的元组表示成功。
3. 值类型的变量将始终按值来传递。 也就是说，当这些变量被用作函数参数或者用在赋值语句中时，总会进行值拷贝
4. 引用类型可以通过多个不同的名称修改它的值

## 数据的位置与赋值

### 数据的位置

数据存储有三种位置： 内存memory 、 存储storage 以及 调用数据calldata 

- memory 即数据在内存中，因此数据仅在其生命周期内（函数调用期间）有效。不能用于外部调用。
- 存储storage 状态变量保存的位置，只要合约存在就一直存储．
- 调用数据calldata 用来保存函数参数的特殊数据位置，是一个只读位置。

其中：

调用数据calldata 是不可修改的、非持久的函数参数存储区域，效果大多类似 内存memory 。 主要用于外部函数的参数，但也可用于其他变量。

### 位置与赋值

数据位置不仅仅表示数据如何保存，它同样影响着赋值行为：

- 在 存储storage 和 内存memory 之间两两赋值（或者从 调用数据calldata 赋值 ），都会创建一份独立的拷贝。
- 从 内存memory 到 内存memory 的赋值只创建引用， 这意味着更改内存变量，其他引用相同数据的所有其他内存变量的值也会跟着改变。
- 从 存储storage 到本地存储变量的赋值也只分配一个引用。
- 其他的向 存储storage 的赋值，总是进行拷贝。 这种情况的示例如对状态变量或 存储storage 的结构体类型的局部变量成员的赋值，即使局部变量本身是一个引用，也会进行一份拷贝（译者注：查看下面 `ArrayContract` 合约 更容易理解）。



## 特殊变量和函数

在全局命名空间中已经存在了（预设了）一些特殊的变量和函数，他们主要用来提供关于区块链的信息或一些通用的工具函数。

可以把这些变量和函数理解为Solidity 语言层面的（原生） API 。

### 区块和交易属性

- `blockhash(uint blockNumber) returns (bytes32)`：指定区块的区块哈希 —— 仅可用于最新的 256 个区块且不包括当前区块，否则返回 0 。
- `block.basefee` (`uint`): 当前区块的基础费用，参考： ([EIP-3198](https://eips.ethereum.org/EIPS/eip-3198) 和 [EIP-1559](https://eips.ethereum.org/EIPS/eip-1559))
- `block.chainid` (`uint`): 当前链 id
- `block.coinbase` ( `address` ): 挖出当前区块的矿工地址
- `block.difficulty` ( `uint` ): 当前区块难度
- `block.gaslimit` ( `uint` ): 当前区块 gas 限额
- `block.number` ( `uint` ): 当前区块号
- `block.timestamp` ( `uint`): 自 unix epoch 起始当前区块以秒计的时间戳
- `gasleft() returns (uint256)` ：剩余的 gas
- `msg.data` ( `bytes` ): 完整的 calldata
- `msg.sender` ( `address` ): 消息发送者（当前调用）
- `msg.sig` ( `bytes4` ): calldata 的前 4 字节（也就是函数标识符）
- `msg.value` ( `uint` ): 随消息发送的 wei 的数量
- `tx.gasprice` (`uint`): 交易的 gas 价格
- `tx.origin` ( `address` ): 交易发起者（完全的调用链）

### ABI 编码及解码函数

- `abi.decode(bytes memory encodedData, (...)) returns (...)`: 对给定的数据进行ABI解码，而数据的类型在括号中第二个参数给出 。 例如: `(uint a, uint[2] memory b, bytes memory c) = abi.decode(data, (uint, uint[2], bytes))`
- `abi.encode(...) returns (bytes)`： [ABI](https://learnblockchain.cn/docs/solidity/abi-spec.html#abi) - 对给定参数进行编码
- `abi.encodePacked(...) returns (bytes)`：对给定参数执行 [紧打包编码](https://learnblockchain.cn/docs/solidity/abi-spec.html#abi-packed-mode) ，注意，可以不明确打包编码。
- `abi.encodeWithSelector(bytes4 selector, ...) returns (bytes)`： [ABI](https://learnblockchain.cn/docs/solidity/abi-spec.html#abi) - 对给定第二个开始的参数进行编码，并以给定的函数选择器作为起始的 4 字节数据一起返回
- `abi.encodeWithSignature(string signature, ...) returns (bytes)`：等价于 `abi.encodeWithSelector(bytes4(keccak256(signature), ...)`
- `abi.encodeCall(function functionPointer, (...)) returns (bytes memory)`: 使用tuple类型参数ABI 编码调用 `functionPointer` 。执行完整的类型检查, 确保类型匹配函数签名。结果和 `abi.encodeWithSelector(functionPointer.selector, (...))` 一致。

### bytes 成员函数

`bytes.concat(...) returns (bytes memory)`

### string 成员函数

`string.concat(...) returns (string memory)`

### 错误处理

Solidity 使用状态恢复异常来处理错误。这种异常将撤消对当前调用（及其所有子调用）中的状态所做的所有更改，并且还向调用者标记错误。

#### 用 `assert` 检查异常(Panic) 

#### 用`require` 检查错误(Error)

#### 用 `try/catch` 捕获异常

```go
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.8.1;

interface DataFeed { function getData(address token) external returns (uint value); }

contract FeedConsumer {
    DataFeed feed;
    uint errorCount;
    function rate(address token) public returns (uint value, bool success) {
        // 如果错误超过 10 次，永久关闭这个机制
        require(errorCount < 10);
        try feed.getData(token) returns (uint v) {
            return (v, true);
        } catch Error(string memory /*reason*/) {
            // This is executed in case revert was called inside getData and a reason string was provided.
            errorCount++;
            return (0, false);
        }  catch Panic(uint /*errorCode*/) {
            // This is executed in case of a panic,i.e. a serious error like division by zero or overflow. 
            // The error code can be used to determine the kind of error.
            errorCount++;
            return (0, false);
        } catch (bytes memory /*lowLevelData*/) {
            // This is executed in case revert() was used。
            errorCount++;
            return (0, false);
        }
    }
}
```

`try` 关键词后面必须有一个表达式，代表外部函数调用或合约创建（ `new ContractName()`）

Solidity 根据错误的类型，支持不同种类的捕获代码块：

- `catch Error(string memory reason) { ... }`: 如果错误是由 `revert("reasonString")` 或 `require(false, "reasonString")` （或导致这种异常的内部错误）引起的，则执行这个catch子句。
- `catch Panic(uint errorCode) { ... }`: 如果错误是由 panic 引起的（如： `assert` 失败，除以0，无效的数组访问，算术溢出等将执行这个catch子句。
- `catch (bytes memory lowLevelData) { ... }`: 如果错误签名不符合任何其他子句，如果在解码错误信息时出现了错误，或者如果异常没有一起提供错误数据。在这种情况下，子句声明的变量提供了对低级错误数据的访问。
- `catch { ... }`: 如果你对错误数据不感兴趣，你可以直接使用 `catch { ... }` (甚至是作为唯一的catch子句) 而不是前面几个catch子句。





### 地址成员

### 合约相关

- `this` (当前的合约类型)

  当前合约，可以显示转换为 [地址类型 Address](https://learnblockchain.cn/docs/solidity/types.html#address)。

- `selfdestruct(address payable recipient)`

  销毁合约，并把余额发送到指定 [地址类型 Address](https://learnblockchain.cn/docs/solidity/types.html#address)。请注意， `selfdestruct` 具有从EVM继承的一些特性：

 \- 接收合约的 receive 函数 不会执行。  - 合约仅在交易结束时才真正被销毁，并且 `revert` 可能会“撤消”销毁。

此外，当前合约内的所有函数都可以被直接调用，包括当前函数。

注解

### 类型信息

表达式 `type(X)` 可用于检索参数 `X` 的类型信息。 目前，此功能还比较有限( `X` 仅能是合约和整型)，但是未来应该会扩展。

用于合约类型 `C` 支持以下属性:

- `type(C).name`:

  获得合约名

- `type(C).creationCode`:

  获得包含创建合约字节码的内存字节数组。它可以在内联汇编中构建自定义创建例程，尤其是使用 `create2` 操作码。 不能在合约本身或派生的合约访问此属性。 因为会引起循环引用。

- `type(C).runtimeCode`

  获得合约的运行时字节码的内存字节数组。这是通常由 `C` 的构造函数部署的代码。 如果 `C` 有一个使用内联汇编的构造函数，那么可能与实际部署的字节码不同。 还要注意库在部署时修改其运行时字节码以防范定期调用（guard against regular calls）。 与 `.creationCode` 有相同的限制，不能在合约本身或派生的合约访问此属性。 因为会引起循环引用。

除上面的属性, 下面的属性在接口类型``I``下可使用:

- `type(I).interfaceId`:

  返回接口``I`` 的 `bytes4` 类型的接口 ID，接口 ID 参考： [EIP-165](https://learnblockchain.cn/docs/eips/eip-165.html) 定义的， 接口 ID 被定义为 `XOR` （异或） 接口内所有的函数的函数选择器（除继承的函数。

对于整型 `T` 有下面的属性可访问：

- `type(T).min`

  `T` 的最小值。

- `type(T).max`

  `T` 的最大值。

## 可见性

### 成员变量的可见性

成员变量的可见性：pubic、private、internal

编译器自动为所有 **public** 状态变量创建 getter 函数。

状态变量的默认可见性：internal

### 函数的可见性

函数的的可见性：pubic、private、internal、external

状态变量的默认可见性：

### 注意

1. 设置为 `private``或 ``internal`，只能防止其他合约读取或修改信息，但它仍然可以在链外查看到。
2. 合约变量在合约内部调用直接使用变量名称，如：`x=3`，如果使用this，如：`this.x=9`则表示使用外部调用方式访问变量
3. 由于 Solidity 有两种函数调用：外部调用则会产生一个 EVM 调用，而内部调用不会
4. external函数：外部可见性函数作为合约接口的一部分，意味着我们可以从其他合约和交易中调用。一个外部函数 `f` 不能从内部调用（即 `f` 不起作用，但 `this.f()` 可以）。



## 合约的函数

### 函数声明

```go
pragma solidity  >=0.4.16 <0.9.0;

contract C {
    function f(uint a) private pure returns (uint b) { return a + 1; }
    function setData(uint a) internal { data = a; }
    uint public data;
}
```

### 函数修改器

```go
contract Mutex {
    bool locked;
    modifier noReentrancy() {
        require(!locked, "Reentrant call."  );
        locked = true;
        _;
        locked = false;
    }

    // 这个函数受互斥量保护，这意味着 `msg.sender.call` 中的重入调用不能再次调用  `f`。
    // `return 7` 语句指定返回值为 7，但修改器中的语句 `locked = false` 仍会执行。
    function f() public noReentrancy returns (uint) {
        (bool success,) = msg.sender.call("");
        require(success);
        return 7;
    }
}
```



如果同一个函数有多个 修改器modifier，它们之间以空格隔开，修改器modifier 会依次检查执行。





## Constant 和 Immutable 状态变量

状态变量声明为 `constant` (常量)或者 `immutable` （不可变量），在这两种情况下，合约一旦部署之后，变量将不在修改。

对于 `constant` 常量, 他的值在编译器确定，而对于 `immutable`, 它的值在部署时确定。

与常规状态变量相比，常量和不可变量的gas成本要低得多。 对于常量，赋值给它的表达式将复制到所有访问该常量的位置，并且每次都会对其进行重新求值。 这样可以进行本地优化。

不可变变量在构造时进行一次求值，并将其值复制到代码中访问它们的所有位置。 对于这些值，将保留32个字节，即使它们适合较少的字节也是如此。 因此，常量有时可能比不可变量更便宜。

不是所有类型的状态变量都支持用 constant 或 `immutable` 来修饰，当前仅支持 [字符串](https://learnblockchain.cn/docs/solidity/types.html#strings) (仅常量) 和 [值类型](https://learnblockchain.cn/docs/solidity/types.html#value-types) .



## 状态可变性

### 状态

下面的语句被认为是修改状态：

1. 修改状态变量。
2. [产生事件](https://learnblockchain.cn/docs/solidity/contracts.html#events)。
3. [创建其它合约](https://learnblockchain.cn/docs/solidity/control-structures.html#creating-contracts)。
4. 使用 `selfdestruct`。
5. 通过调用发送以太币。
6. 调用任何没有标记为 `view` 或者 `pure` 的函数。
7. 使用低级调用。
8. 使用包含特定操作码的内联汇编。

下面的语句被认为是读取状态：

1. 读取状态变量。
2. 访问 `address(this).balance` 或者 `<address>.balance`。
3. 访问 `block`，`tx`， `msg` 中任意成员 （除 `msg.sig` 和 `msg.data` 之外）。
4. 调用任何未标记为 `pure` 的函数。
5. 使用包含某些操作码的内联汇编。

### view函数

可以将函数声明为 `view` 类型，这种情况下要保证不修改状态

多用于读取合约成员变量

### pure函数

函数可以声明为 `pure` ，在这种情况下，承诺不读取也不修改状态变量。

多用于纯计算的函数

## 特殊的函数

### receive函数

函数声明：

```go
receive() external payable { ... }
```

1. 一个合约最多有一个 `receive` 函数
2. 不需要 `function` 关键字，也没有参数和返回值并且必须是　`external`　可见性和　`payable` 修饰． 它可以是 `virtual` 的，可以被重载也可以有 修改器modifier 。
3. 一个没有定义 fallback 函数或　 receive 函数的合约，直接接收以太币（没有函数调用，即使用 `send` 或 `transfer`）会抛出一个异常， 并返还以太币
4. 所以如果你想让你的合约接收以太币，必须实现receive函数（使用 payable　fallback 函数不再推荐，因为payable　fallback功能被调用，不会因为发送方的接口混乱而失败）



### fallback函数

函数声明：

```go
fallback () external [payable]
或者
fallback (bytes calldata input) external [payable] returns (bytes memory output)
```

1. 一个合约可以最多有一个回退函数。
2. 没有　`function`　关键字。　必须是　`external`　可见性，它可以是 `virtual` 的，可以被重载也可以有 修改器modifier 。
3. 如果在一个对合约调用中，没有其他函数与给定的函数标识符匹配fallback会被调用． 或者在没有 [receive 函数](https://learnblockchain.cn/docs/solidity/contracts.html#receive-ether-function)　时，而没有提供附加数据对合约调用，那么fallback 函数会被执行。
4. 如果使用了带参数的版本， `input` 将包含发送到合约的完整数据（等于 `msg.data` ），并且通过 `output` 返回数据。
5. 如果回退函数在接收以太时调用，可能只有 2300 gas 可以使用

```go
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.6.2 <0.9.0;

contract Test {
    // 发送到这个合约的所有消息都会调用此函数（因为该合约没有其它函数）。
    // 向这个合约发送以太币会导致异常，因为 fallback 函数没有 `payable` 修饰符
    fallback() external { x = 1; }
    uint x;
}


// 这个合约会保留所有发送给它的以太币，没有办法返还。
contract TestPayable {
    uint x;
    uint y;

    // 除了纯转账外，所有的调用都会调用这个函数．
    // (因为除了 receive 函数外，没有其他的函数).
    // 任何对合约非空calldata 调用会执行回退函数(即使是调用函数附加以太).
    fallback() external payable { x = 1; y = msg.value; }

    // 纯转账调用这个函数，例如对每个空empty calldata的调用
    receive() external payable { x = 2; y = msg.value; }
}

contract Caller {
    function callTest(Test test) public returns (bool) {
        (bool success,) = address(test).call(abi.encodeWithSignature("nonExistingFunction()"));
        require(success);
        //  test.x 结果变成 == 1。

        // address(test) 不允许直接调用 ``send`` ,  因为 ``test`` 没有 payable 回退函数
        //  转化为 ``address payable`` 类型 , 然后才可以调用 ``send``
        address payable testPayable = payable(address(test));


        // 以下将不会编译，但如果有人向该合约发送以太币，交易将失败并拒绝以太币。
        // test.send(2 ether）;
    }

    function callTestPayable(TestPayable test) public returns (bool) {
        (bool success,) = address(test).call(abi.encodeWithSignature("nonExistingFunction()"));
        require(success);
        // 结果 test.x 为 1  test.y 为 0.
        (success,) = address(test).call{value: 1}(abi.encodeWithSignature("nonExistingFunction()"));
        require(success);
        // 结果test.x 为1 而 test.y 为 1.

        // 发送以太币, TestPayable 的 receive　函数被调用．

        // 因为函数有存储写入, 会比简单的使用 ``send`` or ``transfer``消耗更多的 gas。
        // 因此使用底层的call调用
        (success,) = address(test).call{value: 2 ether}("");
        require(success);

        // 结果 test.x 为 2 而 test.y 为 2 ether.

        return true;
    }

}
```

### 函数的重载

### 函数的签名

### 函数选择器

solidity合约内部：

```go
abi.encodewithsignther()
IERC721Receiver.onERC721Received.selector
```



## 

### 函数的运行时上下文


- 函数调用的运行时上下文环境变量主要有三个：block、transaction、message
- 其性质分别如下：
  - 一次外部账号对合约的调用，可能引发一系列合约之间的调用，即：外部账号EOA调用合约、合约之间的调用
  - 一次外部账号的调用，对应着同一个block、transaction
  - 直接被外部账号调用的合约里的函数，其block、transaction、message是相同的。
  - 合约内部之间的函数调用，其block、transaction、message是不发生变化的。
  - 合约之间相互调用时：block、transaction是相同的，但是会产生新的message
  - transaction和internal transaction。前者指的是EOA的一次调用，直到结束。后者指的是合约之间的调用

## 函数的调用

#### **call函数**

​      Solidity函数动态调用机制只要是依靠call函数实现，起作用类似于Java语言的反射机制

​          call是address的方法，参数为 bytes calldata，其返回值为（boolean success, bytes data）

​          需要校验(require、revert)call的返回值是否为TRUE，忽视返回值，程序不会终止交易，会造成严重后果

​          calldata的前四个字节是函数选择器selector，剩下的是参数编码

​          使用方法：可以通过abi的库函数来获取calldata

​                calldata = abi.encodeWithSignature(signature,params)

​                其中calldata为solidity保留关键字，不允许声明为变量名。

​                signature为函数签名，函数签名的参数不能为别名，比如：uint不行，应该是uint256

## 特别注意

1、对于每一个**外部函数**调用，包括 `msg.sender` 和 `msg.value` 在内所有 `msg` 成员的值都会变化。这里包括对库函数的调用。

2、当合约在链下被评估，而不是在一个区块所包含的交易的背景下被评估时，你不应该假定 block.* 和 tx.* 是指任何特定区块或交易。这些值是由执行合约的EVM实现提供的，可以是任意的。

3、基于可扩展因素，区块哈希不是对所有区块都有效。你仅仅可以访问最近 256 个区块的哈希，其余的哈希均为零。



## 转账

# 深入语言内部

继承

solidity是采用多重继承的方式构建

C3线性化

C3算法得名于它实现了一致性于三种重要特性：

- 基于一致性扩展的[优先图](https://zh.wikipedia.org/w/index.php?title=优先图&action=edit&redlink=1)，
- 局部优先原则（比如A继承B和C，C继承B，那么A读取父类方法，应该优先使用C的方法而不是B的方法），
- 单调性原则（即子类不改变父类的方法搜索顺序）。

https://zh.wikipedia.org/zh-hans/C3%E7%BA%BF%E6%80%A7%E5%8C%96
