Solidity学习笔记

# 基础部分

## 简介

### 一、智能合约

​	智能合约是一种特殊的协议，运行在区块链上。合约内含代码函数，可以与其他合约进行交互、决策、存储及转账等

### 二、智能合约的性质

1. 不可变性：（immutable）
1. 可追踪
1. 允许在没有第三方的情况下进行交易

### 三、执行机制

1. 节点的一侧是交易(Transaction)，另一侧是EVM
2. Block是顺序生成的(广播、共识、机制)，Transaction也是顺序生成
3. Transaction被确认之后，触发EVM的执行。

### 四、开发环境

开发工具：truffle、hardhat

框架：合约打包、模拟器、测试框架

## 合约的基本构成

### 一、合约的基本组成

| 合约构成单元 | 名词解释                                        | 备注 |
| :----------: | ----------------------------------------------- | ---- |
|   状态变量   | 合约的数据                                      |      |
|     函数     | 合约的行为                                      |      |
|  函数修饰器  | 相当于Java中AOP。多用拦截函数的访问控制、校验等 |      |
|  事件event   | 日志，是一种数据支持化存储                      |      |
|   构造函数   | 合约初始化的时候会调用，即部署的时候            |      |

#### 合约的账号

账号：外部账号和内部账号

1. 外部账号：钱包地址。

   非对称密码系统主要有RSA、Ecllipse Curve(椭圆曲线)。

   区块链中用的就是椭圆曲线加密算法。钱包账号就是椭圆曲线的公钥。

   Solidity中address，20个字节160位的数据类型

2. 合约地址：20个字节160位的数据类型。没有公私钥对，地址指向的就是一个合约。

   内部账号：也就是合约账号



#### 合约的状态变量

1. 变量声明的一般形式

   ```
   类型 [可见性] 变量名;
   举例：uint public x;
   ```

2. 变量的可见性

   | 可见性关键字 | 可见范围                                      | 备注                                             |
   | ------------ | --------------------------------------------- | ------------------------------------------------ |
   | public       | 完全可见                                      | 虽然可见，但是只读不能写。需要写，再加方法实现。 |
   | private      | 对本合约可见、其他不可见                      |                                                  |
   | internal     | 对本合约及子合约可见，相当于Java中的protected | **变量默认可见性：internal**                     |

​	当变量的可见性为public时，编译器编译是会为其自动生成get的同名方法。

​	合约外部：其他合约、链下的钱包地址

#### 合约的函数

1. 函数的一般形式

   ```
   function 函数名 ([参数])[可见性][交易相关][...]returns (返回值){...}
   ```

2. 函数的可见性

   | 可见性关键字 |                        可见范围                         |            备注            |
   | :----------: | :-----------------------------------------------------: | :------------------------: |
   |    public    |             合约外部、本合约、子合约均可见              | **函数默认可见性为public** |
   |   private    |                 本合约可见，其他不可见                  |                            |
   |   internal   | 对继承子合约可见，其他不可见（类似于Java中的protected） |           双方都           |
   |   external   |         相当于public，但是其执行机制有较大区别          |                            |

   ​	external和public的区别

3. 交易属性

   ​	默认：什么都不写，交易默认是写操作：全网广播、共识确认。（如果不是写操作，意味着区块链状态没有变化）

   ​	view：合约状态读操作。被修饰的函数，无法进行写操作，否则IDE报错。

   ​	pure：与合约状态无关的函数。也叫纯函数（）

   ​		纯函数：跟系统状态无关，只跟输入有关的函数

   ​		纯函数的作用：解决区块链的授信问题。如果交给链下存在不信任的问题

   ```
   //这就是一个纯函数
   function add(uint a,uint b)internal pure returns(uint){
   	return a+b;
   }
   ```

4. 构造函数

   ```
   //可以带参数，但是没有返回值
   constructor(){
   }
   ```

   在0.6以前，使用【**合约名字】**作为构造函数的名字，而不是constructor

5. 函数修饰器

   1. modifier，大部分常见场景主要用来作为访问控制

      ```
      modifier tooLarge(uint _x){
      	require(_x <= 1000,"too large");  //判断条件，也可以写作：if(_x <= 1000) revert("too large");
      	_; //执行所修饰的函数体
      }
      
      function setX(uint _x) public tooLarge(_x){   //tooLarge 修饰setX函数
      	x = _x;
      }
      ```

   2. 等价使用require==revert

      1. require(_x <= 1000,"too large"); 可以写为if(_x <= 1000) revert("too large");

#### 合约的事件event

​	定义事件event

```
event Xchange(uint oldValue,uint newValue);
```

​		发出事件event

```
emit Xchange(oldValue,newValue);
```

​		事件的结构体（待补充）



### 二、Solidity的数据类型

#### 整形

​	主要包括带符号整形和无符号整形，uint/int 以8位为步长递增，最大256位。

​	对于整形X可以使用type(x).min、type(x).max获取其最大值和最小值

| 类型 | 长度（位） | 有无符号 |
| ---- | ---------- | -------- |
| uint | 256        | 有       |
|      |            |          |
|      |            |          |

​		整形的最大长度256，意味着以太坊虚拟机EVM是256位的机器，可以表达的精度足够高。

​		**Solidity低版本对整形操作**，如果溢出的话会进行取模操作。uint 8   x = 255; x++;  结果是：0。使用SafeMath解决。

​					高版本：会抛出异常；SafeMath对于高版本已经没有意义。

​		**别名**：uint 即 uint256;  int 即 int256

#### 定长数组

​		**bytes1到bytes32**

​		**可通过下标访问元素**

```
				bytes32 array;
				byte b array[1];
				
```

​		**可通过.length访问数组长度**

​		**别名：**byte即bytes1

#### address

​	**address就是账号的地址**

​			账号地址主要分为外部账号地址EOA、contract合约账号

​					外部账号EOA：External Owner Account，以太坊的外部账号。

​							EOA都是可以接收转账的address payable

​					合约账号有可能不能转账

​							什么样的合约不能接收转账？待整理

​	**地址有两种形式**

​		**address：**保存一个20个字节的地址

​		**address payable：**可支付地址，有成员函数transfer和send

​	**地址的转换**

​		address payable 到 address 的隐式转换

​		反过来必须显式转换。

​		地址还可以显示转换为uint160、bytes20

```
function addressSimple() public view returns (bytes20) {
	address me = msg.sender;
	bytes20 b  = bytes20(me);
	return b;
}
```



#### 合约类型

- ​	每一个contract定义都有他自己的类型
- ​	合约类型可以隐式的转换为他的父合约（类似Java多态）
- ​	合约可以显式的转换为address

```
        MyContract c ;
        address addr = address(c);
```

- ​	合约类型不支持任何运算符
- ​	声明一个合约类型的局部变量，则可以调用该合约的函数
- ​	可以使用new关键字创建一个新合约

#### 枚举类型

- ​		枚举是一种用户自定义类型

```
		enum ActionChoices {GoLeft,GoRight} 
```

- ​		枚举的选项从0开始的无符号整形表示，底层是用无符号的8位整形表示。
- ​		枚举类型可以与整形显式相互转换，不允许隐式转换。
- ​		从整形显式转换枚举，会在运行时检查整数是否在枚举范围内，否则会导致异常
- ​		枚举需要至少一个成员变量，默认值是第一个成员变量，只能多于256个成员。
- ​		由于枚举类型不属于[ABI]的一部分，因此对于所有来自Solidity的外部调用，getChoices()方法签名会自动生成getChoices() returns (unit8)



#### 映射（mapping）

​		类比Java中的map

​		声明形式：mapping(_KeyType => _ValueType)

​				_KeyType：可以是任务基本类型，包括bytes和string，但不包括自定义类型，比如：合约、枚举、映射、结构体

​				_ValueType：任何类型，包括mapping类型本身

```
		mapping(address => uint) public balances;
        function update (uint newBalance) public{
        	balances[msg.sender] = newBalance;
        }
```

​		可以作为状态变量或者函数内部的存储，也可以作为库函数的参数。但是**不可以做共有函数的参数或者返回值**

​		只能做合约的成员变量，不能做合约函数的局部变量。不能在函数体中声明

​		声明为public的映射，也会自动创建一个getter函数，_KeyType作为参数，_ValueType作为返回值

​		mapping无法遍历

​		

#### 数组（待整理）

#### 变长字节数据与字符串（待整理）

#### 结构体（待整理）

#### 数据存储的位置（待整理）

#### 函数的上下文变量

​		外部账号调用函数时，函数对背后对应着交易

​		内部账号(即合约)调用一个合约时，也是用一个类似于transaction的数据结构去调用的，这个结构叫做message

​		msg：

|            |         解释          | 备注 |
| :--------: | :-------------------: | :--: |
|    msg     |                       |      |
| msg.sender | 调用者，是address类型 |      |
|  msg.data  |         数据          |      |
| msg.value  |  用于转账的金额的值   |      |



#### 其他

### 三、其他



## web3js访问合约

​		示例代码：

```
	function getOwner(){
		var contractAddress = "0x";
		var owner_abi = "xxxx应用二进制调用接口";
		var ownercontract = new web3.eth.Contract(owner_abi,contractAddress);
		ownercontract.methods.getOwner().call(function(err,res){
			if(err){
				console.log(err)
				return
			}
			alert("The owner is :" + res);
		});
	}
```



# 其他部分
