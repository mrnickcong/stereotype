# Solidity语言简介

  Solidity 是一门面向合约的、为实现智能合约而创建的高级编程语言

  设计的目的是能在以太坊虚拟机（EVM）上运行

  Solidity 是静态类型语言，支持继承、库和复杂的用户定义类型等特性。



# Solidity语言基础

## 关键字

### 一、重点关键字

|  关键字  |     名称     |                            解释                            |      备注       |
| :------: | :----------: | :--------------------------------------------------------: | :-------------: |
| modifier | 函数的修改器 | 可以为一个函数控制其中逻辑。修改器为属性，可以继承和重写。 | 类似于Java的AOP |
|          |              |                                                            |                 |
|          |              |                                                            |                 |

函数的可见性

基础语（一）基本类型
类型名解释实例string字符串型"hello world"bool布尔类型true，falseint有符号整数型7，10，99，-100uint无符号整数型，不能为负22，66，89address地址，用于表示账户或者合约0x1234567890123456789012345678901234567890
注：uint8,uint16,uint24….uint256,以8位为步长，其中uint相当于uint256，int类似，相当于int256。
（二）引用类型
类型名解释备注Type[8]定长数组固定长度Type[]动态数组Length表示数量，push推入struct结构体结构体中可以包含大量的数据类型Mapping(type=>type)映射表Mapping(int=>address)bytes32字节数组
（三）存储方式
方式名解释备注storage成员变量永久保存在状态树中（付费）memory局部变量临时存储（值传递）calldata函数参数变量临时存储的一个数据位置
内置对象
（一）block
Block 在调用某个方法的时候，solidity会提供一个block的变量，把当前块的信息返回
方法名返回类型返回信息blockhash(uint blockNumber)bytes32指定区块的哈希值Block.coinbaseaddress当前区块的矿工地址Block.difficultyuint当前区块难度Block.limituint当前区块gas消耗限制Block.numberuint当前区块编号Block.timestampuint当前区块时间戳
Now是一个uint类型，指的是当前块的时间戳，与block.timestamp一样
（二）msg
msg 在调用某个方法的时候，solidity会传递一个msg的属性，用来传递消息
方法名返回类型返回信息msg.gasuint剩余的gasmsg.senderuint该消息的发送者msg.siguint数据的前四个字节msg.valueuint发送的消息的数量
（三）内建函数
Solidity，内置了一些函数用于高效计算solidity中的一些变量的计算
函数名返回类型返回信息addmod(uint x, uint y, uint k)uint计算(x + y)%kmulmod(uint x, uint y, uint k)uint计算(x * y)%kkeccak256(bytes memory s)bytes32计算数据的keccak256sha256(bytes memory s)bytes32计算数据的sha256ecrecover(bytes32 hash, uint8 v, bytes32 r, bytes32 s)addres恢复与公钥关联的地址（椭圆曲线算法）
（四）abi解码工具函数
合约应用程序二进制接口（ABI）是在以太坊生态系统中与合约进行交互的标准方法，既可以从区块链外部进行，也可以用于合约间的交互
函数名返回类型备注abi.decode(bytes memory encodedData, (...))(...)(uint a, uint[2] memory b, bytes memory c) = abi.decode(data, (uint, uint[2], bytes))abi.encode(...)bytes memory对给定的参数进行Abi编码abi.encodePacked(...)bytes memory对给定的参数进行A压缩编码abi.encodeWithSelector(bytes4 selector, ...)bytes memory从第二个开始对给定的参数进行ABI编码，并在给定的四字节选择器之前添加abi.encodeWithSignature(string memory signature, ...)bytes memory等价于abi.encodeWithSelector(bytes4(keccak256(bytes(signature)), ...)
注: keccak256(abi.encodePacked(a, b))是一种计算hash值的方式，比如对于一个函数，如果输入输出不同，最终的结果都不一样。
（五）内置调试函数
函数名备注assert(bool condition, string memory reason)自信某条件一定成立，用于安全设计使用；如果不成立，扣光所有gasrequire(bool condition, string memory reason)温和认定某条件成立，如果不满足，退回剩余的gasrevert(string memory reason)终止合约执行，并还原状态变更
注：
建议多用require，少用assert
Assert多用于判断非状态变量
Assert可以考虑放在函数结尾部分用于验证之前的操作结果正确
revert是主动退回gas，用于if/else判断后使用
（六）随机数
random = uint(keccak256(abi.encodePacked(msg.sender ,block.difficulty, now)));
复制代码
注：
任何随机数生成方法，生成的都是伪随机数。
Solidity是面向对象的，内部有this。







##
问题: 用solidity写一个ERC20的合约
------------------------------
回答: 
# 如何写一个ERC20的合约
## 介绍
ERC20是一个基于以太坊的标准，定义了一种可以在以太坊上流通的通用代币，用它可以实现跨链转移，在以太坊上进行交易等等。本文将介绍如何用solidity语言写一个ERC20的代币合约。
## 编写ERC20合约
ERC20定义了一系列的函数，编写ERC20合约时，需要将这些函数都实现。这些函数包括：
- `totalSupply()`：获取代币的总量
- `balanceOf()`：获取某个账户的余额
- `transfer()`：转移代币
- `approve()`：授权给某个账户转移代币
- `allowance()`：查询某个账户的授权余额
- `transferFrom()`：根据授权，转移代币

## 实现事件
在ERC20合约中，我们需要实现一些事件，这些事件可以被外部程序监听，以便查看代币的转移情况。这些事件包括：
- `Transfer()`：转移事件
- `Approval()`：授权事件

## 测试
编写完ERC20合约以后，我们需要对它进行测试，测试时，我们需要检查合约的功能是否正确，特别是转移和授权功能是否正常。

## 结论
ERC20是一种基于以太坊的标准，用它可以实现跨链转移，在以太坊上进行交易等等。本文介绍了如何用solidity语言实现一个ERC20的代币合约，包括编写ERC20合约、实现事件和测试。
