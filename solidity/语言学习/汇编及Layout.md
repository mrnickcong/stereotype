# 汇编及Layout

## Layout in Memory

Solidity reserves four 32-byte slots, with specific byte ranges (inclusive of endpoints) being used as follows:

- `0x00` - `0x3f` (64 bytes): scratch space for hashing methods   哈希函数的暂存区
- `0x40` - `0x5f` (32 bytes): currently allocated memory size (aka. free memory pointe)，即：自由内存指针
- `0x60` - `0x7f` (32 bytes): zero slot。暂存区，用于动态内存数组的初始化。
- 

## 常用的操作码

### Memory 操作

mload

mstore

call

return

delagatecall：没有value(当做内部调用)

calldata 

calldatacopy

returndatacopy

### Storage操作

sload

sstore

### 代码示例：

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Assembly {
    uint256 public number;

    function sstore(uint256 _number) public {
        assembly {
            //number.slot的值存储在栈上，所有的局部变量都在堆栈上
            sstore(number.slot, _number)
        }
    }

    function readData() public view returns (uint256) {
        assembly {
            //加载number.slot位置的值
            let est := sload(number.slot)
            //加载自由内存0x40处的指针
            let free_pointer := mload(0x40)
            //保存值
            mstore(free_pointer, est)
            //堆栈上的数据无法直接返回，需要存储在内存中，才能返回
            return(free_pointer, 32)
        }
    }
}

```



## 合约的Bytecode

合约有两种二进制码：Creation Bytecode和Runtime Bytecode

### Creation Bytecode

creation code的获取方式：type(ContractName).creationCode

### Runtime Bytecode

## 合约的部署机器码

### create和create2

#### create

creates a child contract

```
uint8：F5
stack input : value、offset、length
stack output : addr
Expression：addr = new memory[offset:offset+length].value(value)		
其中：value：是创建合约时可以创建一定量的币，这时构造应该是payable的
```

#### create2

creates a child contract with a deterministic address

```java
uint8：F0
stack input : value、offset、length、salt
stack output : addr
Expression：addr = new memory[offset:offset+length].value(value)
```

#### 区别

- 他们都是EVM部署的机器码

- create产生的地址都是随机的，但是create2可以为合约产生固定的地址，uniswap 的 factory 部署 pair 在主网和测试网地址是一样的原因



#### 示例代码

地址：https://solidity-by-example.org/app/create2/

```java
// This is the older way of doing it using assembly
contract FactoryAssembly {
    event Deployed(address addr, uint salt);

    // 1. Get bytecode of contract to be deployed
    // NOTE: _owner and _foo are arguments of the TestContract's constructor
    function getBytecode(address _owner, uint _foo) public pure returns (bytes memory) {
        bytes memory bytecode = type(TestContract).creationCode;

        return abi.encodePacked(bytecode, abi.encode(_owner, _foo));
    }

    // 2. Compute the address of the contract to be deployed
    // NOTE: _salt is a random number used to create an address
    function getAddress(
        bytes memory bytecode,
        uint _salt
    ) public view returns (address) {
    
        //这就是create2固定hash地址的算法
        bytes32 hash = keccak256(
            abi.encodePacked(bytes1(0xff), address(this), _salt, keccak256(bytecode))
        );
        //取前20个字节160位，作为部署的地址address的值
        // NOTE: cast last 20 bytes of hash to address
        return address(uint160(uint(hash)));
    }

    // 3. Deploy the contract
    // NOTE:
    // Check the event log Deployed which contains the address of the deployed TestContract.
    // The address in the log should equal the address computed from above.
    function deploy(bytes memory bytecode, uint _salt) public payable {
        address addr;

        /*
        NOTE: How to call create2

        create2(v, p, n, s)
        create new contract with code at memory p to p + n
        and send v wei
        and return the new address
        where new address = first 20 bytes of keccak256(0xff + address(this) + s + keccak256(mem[p…(p+n)))
              s = big-endian 256-bit value
        */
        assembly {
            addr := create2(
                callvalue(), // wei sent with current call
                // Actual code starts after skipping the first 32 bytes 因为前32字节是length
                add(bytecode, 0x20), /偏移32字节，0x20是16进制，32个字节
                mload(bytecode), // Load the size of code contained in the first 32 bytes
                _salt // Salt from function arguments
            )
            //检查运行时地址是不是0，不是0部署成功
            if iszero(extcodesize(addr)) {
                revert(0, 0)
            }
        }

        emit Deployed(addr, _salt);
    }
}
```


## 其他

## 其他



交易中：gasLimit、value、to、data

合约部署的时候：没有to、有data(creationcode)

合约调用的时候：有to、有data()

EOA账户调用：数据格式同下

合约之间调用：在message中calldata->abi.encodeWithsinger("")



## 写操作事件

一个合约方法是写操作，即使定义了返回值，链下也没法获得返回值，需要通过时事件的方式监听

Web3.js通过await不能获得合约的返回值。

但是合约之间、或者内部的调用可以获得函数的返回值。



## 记录碎的点

1、所有的变量都在堆栈上；参数的引用指针也在堆栈上

2、memory以字节为存储单位，storage以slot为存储单位

3、offset指的是一个字节length区后面的data区

一个bytes由：length区(第一个32字节，16进制0x20)+data区构成

## Function Signature & Function Selector

函数签名和函数选择器

Function 的完整字串實際上也就是所謂的 Function Signature，而哈希過後得到的 ABI Byte String 便是 Function Selector

示例代码：

```go
方法： someFunction(uint _myUint1, address _someAddr)
函数签名：someFunction(uint256,address)
函数选择器：bytes4(keccak256("someFunction(uint256,address)"))
```



## 简历

熟练掌握solidity开发、部署、测试流程。

熟练掌握truffle、hardhat等开发工具；

熟悉ERC20、ERC721、ERC1155等标准，熟悉Openzepeli等第三方合约库；

掌握代理合约的原理及使用

深入理解EVM，熟悉EVM的底层实现机制、内存布局及slot机制；

熟悉assembly内联汇编指令在solidity中的使用；

了解uniswap源码和设计原则

了解slither静态分析工具的使用。
