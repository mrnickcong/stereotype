# Solidity学习笔记

# 第一部分：入门简介

# 第二部分：基础部分

## 简介

### 一、智能合约

​	智能合约（Smart contract ）是一种旨在以信息化方式传播、验证或执行合同的计算机协议。

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

​		**事件的结构体（待补充）**



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

​		**只能做合约的成员变量，不能做合约函数的局部变量。不能在函数体中声明**

​		声明为public的映射，也会自动创建一个getter函数，_KeyType作为参数，_ValueType作为返回值

​		**mapping无法遍历**

​		

#### 数组

- ​		数组的声明

  ```
  	固定长度为K的元素为T的数组：T[K]
  	动态数组：T[]
  	使用new在内存中基于运行时创建动态长度的数组，这种数组不可以用push改变大小
  ```

- ​		性质：

  - 数组可以在声明时指定长度，也可以动态的调整长度

  - 数组通过下标访问，从0开始。越界访问会导致assert异常

  - 数组元素的类型可以是任何类型，包括映射或结构体。其中映射只能存储在storage中

  - public的数组状态变量的getter函数，参数是下标数字

  - 方法.push()、.push(value)可用于在数组末尾附加一个新元素，其中.push()函数附加一个零初始化元素并返回对他的引用（仅对storage中的数组）

    

```
使用举例：
contract Simple{
	string[] private array;
	
	function arraySimple() public view {
		array.push("hello");
		uint length =  array.length; //结果为：1
		string value = array[0]; //结果为：hello
	}
	
	function f(uint length) public pure {
		uint[] memory a = new uint[](7);
		bytes memory b  = new bytes(length);
		
		assert(a.length==7);
		assert(a.length==length);
		
		a[6] = 8;
	}

}

```



#### 变长字节数据与字符串（bytes与string）

​		他们都是数组，不是值类型

​		Solidity没有字符串操作函数，可以使用第三方库操作

​		可以将string转换bytes，转换时数据内容本身不会拷贝

​		bytes类似于byte[]，但是它在calldata和memory中连续存储，不会按照每32字节一个单元来存储

​		

#### 结构体struct

​		结构体为开发者自定义结构对象，可以作为状态变量存储，也可以在函数中作为局部变量存在

​		结构体类型可以作为元素用在映射和数组中，结构体的成员变量也可以是映射和数组

​		使用举例：

```
	struct Person{
		uint age;
		string name;
	}
	
	Person private _person;
	
	function structExample () public {
		Person memory p = Person(18,"Nick");
		_person = p;
	}
	
	function setName(string calldata name) external {
		_person.name = name;
	}
```



#### 数据存储的位置（待补充）

​		数据存储有三种位置：内存(memory)、存储(storage)、调用数据(calldata)

​		calldata是不可修改的，非持久的函数参数存储区域，效果类似memory

​		calldata是external函数的参数所必须指定的位置，也可以用于其他变量

​		在存储和内存之间赋值，会创建一份独立的拷贝

​		在内存之间赋值只创建引用，从存储到本地之间的赋值也只创建引用。其他存储的赋值会进行拷贝

​		**使用举例**

```
（待补充）
```



#### 函数的上下文变量

​		外部账号调用函数时，函数对背后对应着交易

​		内部账号(即合约)调用一个合约时，也是用一个类似于transaction的数据结构去调用的，这个结构叫做message

​		msg：

|  内置函数  |         解释          | 备注 |
| :--------: | :-------------------: | :--: |
|    msg     |                       |      |
| msg.sender | 调用者，是address类型 |      |
|  msg.data  |         数据          |      |
| msg.value  |  用于转账的金额的值   |      |



#### 关键字：constant、immutable（待整理）

​			constant：静态变量

​			immutable：赋值后不可改变

immutable：不占用存储槽，编译的时候，嵌入字节码；

constant：不占用存储槽，编译时是一个占位符，runtime执行构造函数的时候替换占位符，嵌入字节码；

#### 合约的继承：is

​			相当于Java中的extend和implement

#### 其他

### 三、其他

## 合约函数的性质

### 一、合约函数的调用	

​			合约内部调用

​			合约之间直接调用

​			通过接口合约调用

​					调用者必须持有被调用者的合约地址

​					调用者自定义一个接口，其中的函数的签名signature应与被调用合约相应的函数相同

​					将地址重载为接口，调用函数

### 二、函数签名、method ID 、函数选择器

#### 如何获得函数选择器

solidity合约内部：

```
abi.encodewithsignther()

IERC721Receiver.onERC721Received.selector
```



### 三、函数的重载

### 四、函数的运行时上下文

- 函数调用的运行时上下文环境变量主要有三个：block、transaction、message
- 其性质分别如下：
  - 一次外部账号对合约的调用，可能引发一系列合约之间的调用，即：外部账号EOA调用合约、合约之间的调用
  - 一次外部账号的调用，对应着同一个block、transaction
  - 直接被外部账号调用的合约里的函数，其block、transaction、message是相同的。
  - 合约内部之间的函数调用，其block、transaction、message是不发生变化的。
  - 合约之间相互调用时：block、transaction是相同的，但是会产生新的message
  - transaction和internal transaction。前者指的是EOA的一次调用，直到结束。后者指的是合约之间的调用	

- 示意图如下 processon上

### 五、函数的动态调用

#### 			call函数

​			Solidity函数动态调用机制只要是依靠call函数实现，起作用类似于Java语言的反射机制

​					call是address的方法，参数为 bytes calldata，其返回值为（boolean success, bytes data）

​					需要校验(require、revert)call的返回值是否为TRUE，忽视返回值，程序不会终止交易，会造成严重后果

​					calldata的前四个字节是函数选择器selector，剩下的是参数编码

​					使用方法：可以通过abi的库函数来获取calldata

​								calldata = abi.encodeWithSignature(signature,params)

​								其中calldata为solidity保留关键字，不允许声明为变量名。

​								signature为函数签名，函数签名的参数不能为别名，比如：uint不行，应该是uint256

​			合约函数动态调用方法示例代码**（待整理）**

![](D:\space\interview\images\合约函数动态调用方法示例代码.png)

合约函数动态调用方法示例代码(带返回值)

![](D:\space\interview\images\合约函数动态调用方法示例代码(带返回值).png)



#### fallback函数

​		是一个特殊函数，调用一个不存在的函数的时候，会只执行fallback函数。

​		在转账功能中有重要作用（被payable修饰的fallback函数）

​		proxy模式代理合约中有重要作用，配合delegatecall使用，支持合约的升级

​		fallback函数都是external的，address.call{value;}()可以出发fallback函数，但是address.send()无法触发fallback函数

​		

```
	//事件
	event Log(string func, uint gas);

    // Fallback function must be declared as external.
    fallback() external payable {
        // send / transfer (forwards 2300 gas to this fallback function)
        // call (forwards all of the gas)
        emit Log("fallback", gasleft());
    }
    
    //fallback也可以指定输入参数和返回结果
     fallback(bytes calldata data) external payable returns (bytes memory) {
        (bool ok, bytes memory res) = target.call{value: msg.value}(data);
        require(ok, "call failed");
        return res;
    }

```



### 六、Gas

##### 	什么是gas	（待整理）

##### 	为什么要有gas	（待整理）

​			

##### 	gas与gas price

- ​			实际的gas完全由执行逻辑决定，有变化的是gas价格，由transaction来决定

- ​			交易发起者可以设定gas消耗的上限gasLimit，交易成功：剩余的gas返回；交易失败：已经用的gas不会返回，剩余的gas返回。

- ​			合约之间的函数调用可以设置gasLimit，调用者控制gas消耗

##### 	**gas优化（进阶部分）**



### 七、函数调用总结

#### 	mapping的遍历

##### 			为什么需要遍历？

​					分红场景：需要知道每个账号分红的量；质押投票场景：需要知道账号的质押的量；

##### 			如何遍历？

​					solidity中的mapping不能遍历，如果需要遍历，需要自己手动实现

​					实现思路：借助数组实现，代码示例如下

```

```

#### pure函数不规范调用

​			address.call()调用的任何函数，都会产生交易，消耗gas。即使调用的函数是pure修饰的函数。技术上是允许的，没有意义

#### 合约内部函数call调用不规范调用

​		合约内部函数一般直接调用即可，不会产生新的message。

​		但是使用call跨合约调用函数，EVM会把他当做两个合约之间的调用。msg.sender会变化，从EOA账号变为当前合约账号。msg.data也会变化

#### external和public

​		external和public修饰的成员变量在可见性上是一样的

​		合约内部调用external方法，需要使用this，意味着：要底层要使用call()方式执行，会产生新的上下文。

​		因此需要明确的设计原则：

​				1. 合约内部函数之间的调用，应避免产生新的上下文； 

​				2. 如果external关键字修饰的函数需要被内部调用，应将其变为public，避免使用this关键字来调 用。

#### delegatecall复杂调用

​		调用者A通过delegatecall调用B合约，被调用者B又通过call调用了自己B的函数，会发生什么？调用结果：交易失败

​		调用者A通过call调用B合约，被调用者B又通过call调用了自己B的函数，会发生什么？调用结果：交易成功

​		

## 转账

### 一、转账发起

​	第一种转账方式：通过address的内置函数实现转账

​		**前提**：智能合约中必须定义了**`payable`**的**`fallback`**函数。

|            方法            | 是否指定gas数量 |   gas数量    |        执行异常处理        |
| :------------------------: | :-------------: | :----------: | :------------------------: |
|    address.send(amount)    |       否        |   默认2300   | 返回FALSE，手动require处理 |
|  address.transfer(amount)  |     不支持      |   默认2300   |     抛出异常，交易终止     |
| address.call.value(amount) |       是        | 可自定义指定 | 返回FALSE，手动require处理 |

​	**注意：**

​			如果使用address.send(amount)、address.transfer(amount)进行转账，被调用的合约必须实现receive()函数 receive() external payable{}

​			receive是专门支持上面两个函数的，但是要求gas不超过2300（设置2300，为了防止重入攻击），2300仅能支持emit一个事件，做其他操作会超出gasLimit，转账会失败。因此一般不用这种方式，一般直接mint或者fallback里mint

​	第二种转账方式：**触发合约中不存在的函数或者空方法**

​		**前提**：智能合约中必须定义了**`payable`**的**`fallback`**函数。

​		**原理**：根据TVM特性，调用合约中的空方法或者不存在的函数时，TVM会自动调用`fallback`来执行，借助`payable`的`fallback`函数，用户就可以完成指定数量的TRX/TRC10转账。由于智能合约的执行都是在TVM里，所以最终这些转账都会被记录到合约的存储里。

​	**具体过程**：使用`triggersmartcontract`触发合约中的空方法或者不存在的函数。TRX的数量通过`call_value`指定，TRC10的数量通过`call_token_value`指定。

​	**局限性：**如果`fallback`函数不是`payable`的，无法使用此功能

​	**总结：**

​		这两种方式都有一个共同的前提：智能合约中必须定义了**`payable`**的**`fallback`**函数。但是现实中，由于各种原因，很多已经部署的智能合约没有定义`payable`的`fallback`函数，对于这种情况应该如何处理呢？本文稍后会讲到如何处理此类合约。

​		使用`send`函数有许多危险的地方，如果调用堆栈的深度达到1024（参考前文讲述的调用[堆深度限制](https://xz.aliyun.com/t/3316#toc-8)）或者将gas值使用完，则send函数会返回调用失败。所以为了满足以太币转账的安全性，我们需要对send的返回值进行检查，或者我们干脆直接使用`transfer`来代替`send`函数。

而我们在使用`send`或`transfer`函数的时候，需要注意定义`Fallback`函数，不定义回退函数将抛出异常并返回Ether。

### 二、转账接收

​		当转账接收者为合约时：

​				只需要写一个支付函数就可以

```
		function pay()payable{ }
```

​				当把代币转入合约时，如果合约没有办法取出，则代币将永久的存储在合约中，无法撤销了

​		当转账接收方为账户时：

​				那么发送方可以是账户也可以是合约，两者的区别在于有没有传递msg.value。没有传递则为合约转账户，反之为账户转账户。

​		



合约可以存储货币

​		合约与合约之间、合约与EOA之间可以转账

​		多签钱包：钱存储在合约中

​		转账与函数调用（照片待整理）

payable修饰函数

​		一个函数被payable修饰表示该函数可以接收转账

​		调用时加上调用选项：{gas[gaslimit]:<gas>,value:<value>} 其中value表示转账金额

​		如果一个函数被payable修饰了，但是value=0，则表示普通的函数调用

​		如果调用者value不为0且 函数无payable修饰，则无法进行转账，是编译报错还是转账失败？（待研究）

三种转账方式（照片待整理）开发过程中应该避免这三种方式的使用

​		其中address.transfer() 和 address.send() 是address的内置函数，默认gasLimit为2300，为了避免重入攻击。

​		receive需要去手动实现的一个函数

```
	event Log(string func, uint gas);

    // Fallback function must be declared as external.
    fallback() external payable {
        // send / transfer (forwards 2300 gas to this fallback function)
        // call (forwards all of the gas)
        emit Log("fallback", gasleft());
    }

    // Receive is a variant of fallback that is triggered when msg.data is empty
    receive() external payable {
        emit Log("receive", gasleft());
    }
```

​		指定gas(其实是gasLimit)表示超过gasLimit交易失败，如果不指定表示不设置上限gasLimit



## 存储布局（待整理-照片）

### 		什么是存储布局？

​				存储布局：storage layout 。指的是智能合约的状态变量、成员变量在存储空间的分布

### 		如何存储？

#### 					值类型存储

#### 					Mapping类型 与 动态数组 的存储

#### 					array存储

### 存储位置（待整理-照片）

​		声明一个memory的变量，该变量被赋值为一个storage的变量。会在memory中复制一份storage的数据，而不是直接使用引用

​		mapping都是storage的

​		calldata向memory拷贝数据。外部调用的参数，都是通过calldata传递过来的，如果这时将变量声明为calldata的，效率很高，不用拷贝数据

​		修改memory数据不会改变合约状态，修改storage数据会改变合约状态？不知道这句话对不对，待验证







## delegatecall（待整理-照片）

address.delegatecall(bytes calldata) 

delegatecall是将另一个合约的一个函数take over拿来当作自己合约内部函数来用; 

从函数执行上下文来看，形式上跨合约，实质上没有跨合约，拷贝了message，sender、 value不变，但data和gaslimit可以变化 

函数按照内存布局访问变量，delegatecall的调用者和被调用者合约应该具有兼容的 storage（一模一样？） layout 

思考题：delegatecall会有调用选项{gas：，value：}吗？ 

注意：msg.gas被gasleft()取代 

代码调试观察内部调用和delegatecall中msg.data的变化不同



delegatecall调用示例图

（画个图）

代码示例

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.8;

// NOTE: Deploy this contract first
contract B {
    // NOTE: storage layout must be the same as contract A
    uint256 public num;
    address public sender;
    uint256 public value;
    bytes public cdata;

    function setVars(uint256 _num) public payable {
        num = _num;
        sender = msg.sender;
        value = msg.value;
        cdata = msg.data;
    }
}

contract A {
    uint256 public num;
    address public sender;
    uint256 public value;
    bytes public cdata;

    function setVars(address _contract, uint256 _num) public payable {
        // A's storage is set, B is not modified.
        (bool success, bytes memory data) = _contract.delegatecall(
            abi.encodeWithSignature("setVars(uint256)", _num)
        );
    }

    function setNum(uint256 _num) public payable {
        num = _num;
        cdata = msg.data;
        sender = msg.sender;
        value = msg.value;
    }

    function setNumIndirect(uint256 _num) public {
        setNum(_num);
    }

    function setNumIndirectByCall(uint256 _num) public {
        address(this).call(abi.encodeWithSignature("setNum(uint256)", _num));
    }
}
```

### 



## 代理合约（待整理）

升级问题：

​			合约的不可变性，如果业务发生变化或者合约遭到攻击，也需要升级

​			升级需要特殊技术和模式支持

​			升级应不伤害去中心化和民主

代理模式：

​		**待整理**

## 合约升级（待整理）

​				接口合约到底能不能部署？待验证

## 库（待整理）

### 什么是库函数library

### using

​	是一种语法糖

​	让库的方法跟他所操作的第一个参数绑定，成为第一个参数的数据类型的方法（这句话不太明白）

代码实例：

```

```

### internal与public

​		库函数被（待整理）

### storage参数

​		Library没有成员变量，可以允许参数为storage类型，但是普通合约不允许。为了让Library操作调用者的成员变量数据

### Library是否可以替代代理合约？

​		对Library在编译时完成，不是运行时的，无法动态升级

### 特点

1、当library含有public可见性函数时，需要部署为单独的合约

2、在部署合约的时候，通过linkref将library合约地址与该合约连接起来

# 第三部分：提高部分



## 一、继承

继承

多继承

## 二、汇编

## 三、重入攻击

重入攻击

重入攻击的防御

## 四、闪贷

## 五、OpenZipplin

#### meta transaction

#### delete（照片待整理）

基本认知：EVM是栈机器，没有堆。栈配合内存形成逻辑

delete只作用于memory、storage。

delete作用机制：将固定长度的数值设置为其默认值，动态数据类型长度设置为0

delete可以省gas

# 汇编

## 一、EVM

​				EVM是一个栈机，没有寄存器，所有的操作都是在栈上操作的。

opcode：机器码

#### 指令的运行环境：

堆栈

内存

storage

上下文：主要是指：block、transaction、message等上下文变量

# 继承

## 继承原则

*Inheritance must be ordered from “most base-like” to “most derived”.*

## 基本语法

is关键字  +  virtual override  ：父类virtual函数，子类override

多重继承中的超类顺序：most base-like 和 most derived 

most base-like 和 most derived：  

左侧更趋向于超类

```
/* Graph of inheritance
    A
   / \
  B   C
 / \ /
F  D,E

*/

contract A {
    function foo() public pure virtual returns (string memory) {
        return "A";
    }
}

// Contracts inherit other contracts by using the keyword 'is'.
contract B is A {
    // Override A.foo()
    function foo() public pure virtual override returns (string memory) {
        return "B";
    }
}
```



## 关键字

### virtual

可以被子类相同函数签名的函数来覆盖

### override

覆盖：函数的签名、可见性、函数参数列表、函数的交易属性(pure、view 、payable)、返回值参数

### is

is多重继承。继承层级相同的类，可以互换顺序。层级高的在左侧

```


```

### super



```
contract F is A, B {
    function foo() public pure override(A, B) returns (string memory) {
        return super.foo();
    }
}

//多重继承，super返回层级最低的父类
```

super在多重继承下，不是单纯意义的超类，而是C3线性化中的前驱，也就是super是调用前驱的方法。如下：（**不是太明白**）

```
/* Graph of inheritance
    A
   / \
  B   C
  \ /
   D
*/
//对于上面的继承关系，C3线性化 A -> B -> C -> D

contract C is A {
    function foo() public pure override(A, B) returns (string memory) {
    	//此处的super指的是B
        return super.foo();
    }
}

```

## 继承中的构造函数



## C3线性化

C3线性化：一种遍历继承关系图的算法（是一个有向无环图）

在存储布局、super调用、constructor执行顺序中启作用

![](D:\space\interview\solidity\C3线性化.png)

## 几个注意点

### 1、成员变量

子类与超类同名的成员变量：private允许，因为private只有自己合约可见；public、internal不可以，因为子类可见。

函数也是一样

即：子类不能声明与可见父类的成员变量的成员变量

event、modify都是默认平public的，function private 不起作用。因此：子类不能声明与父类同名的成员变量，除非继承









# 几个比较重要的opcode

mload：把内存中一个256位的信息加载进堆栈的头部，表达式：value = memory[offset:offset+32]

mstore：将值写入内存的指定位置。表达式：memory[offset:offset+32] = value



# 第四部分：其他部分

## 一、web3js访问合约

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

1、如何在一个函数内实现对合约地址与账户的同时转账呢？有些流动性矿池都会有相关设计

​		该函数能够实现将msg.value一半转给acount1,一半转给合约本身

```
    function pay()payable{
          acount1.transfer(msg.value / 2);  //msg.value是大数，除2后为整数
    }
    
    // 原理是：当msg.value大于实际转账value时，多余的ETH将会被转给合约。
```

## 二、hardhat的使用

脚本使用

脚本使用的注意问题

## 三、truffle的使用

```
const BN = require("bn.js")
const { sendEther, pow } = require("./util")
const { DAI, DAI_WHALE, USDC, USDC_WHALE, USDT, USDT_WHALE } = require("./config")

const IERC20 = artifacts.require("IERC20")
const TestUniswapFlashSwap = artifacts.require("TestUniswapFlashSwap")

contract("TestUniswapFlashSwap", (accounts) => {
  const WHALE = USDC_WHALE
  const TOKEN_BORROW = USDC
  const DECIMALS = 6
  const FUND_AMOUNT = pow(10, DECIMALS).mul(new BN(2000000))
  const BORROW_AMOUNT = pow(10, DECIMALS).mul(new BN(1000000))

  let testUniswapFlashSwap
  let token
  //相当于@Before
  beforeEach(async () => {
    token = await IERC20.at(TOKEN_BORROW)  //at表示引用合约
    testUniswapFlashSwap = await TestUniswapFlashSwap.new()  //new()部署一个新的合约

    await sendEther(web3, accounts[0], WHALE, 1)

    // send enough token to cover fee
    const bal = await token.balanceOf(WHALE)
    assert(bal.gte(FUND_AMOUNT), "balance < FUND")
    await token.transfer(testUniswapFlashSwap.address, FUND_AMOUNT, {
      from: WHALE,
    })
  })


	//单元测试，相当于@Test
  it("flash swap", async () => {
    const tx = await testUniswapFlashSwap.testFlashSwap(token.address, BORROW_AMOUNT, {
      from: WHALE,
    })

    for (const log of tx.logs) {
      console.log(log.args.message, log.args.val.toString())
    }
  })
})

```



## 四、可升级合约完整代码



```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.13;

interface ProxyInterface {
    function inc() external;

    function x() external view returns (uint256);
}

contract Proxy {
    bytes32 private constant implementationPosition =
        keccak256("org.zeppelinos.proxy.implementation");

    function upgradeTo(address newImplementation) public {
        address currentImplementation = implementation();
        setImplementation(newImplementation);
    }

    function implementation() public view returns (address impl) {
        bytes32 position = implementationPosition;
        assembly {
            impl := sload(position)
        }
    }

    function setImplementation(address newImplementation) internal {
        bytes32 position = implementationPosition;
        assembly {
            sstore(position, newImplementation)
        }
    }

    function _delegate(address _imp) internal virtual {
        assembly {
            // calldatacopy(t, f, s)
            // copy s bytes from calldata at position f to mem at position t
            calldatacopy(0, 0, calldatasize())
            // delegatecall(g, a, in, insize, out, outsize)
            // - call contract at address a
            // - with input mem[in…(in+insize))
            // - providing g gas
            // - and output area mem[out…(out+outsize))
            // - returning 0 on error and 1 on success
            let result := delegatecall(gas(), _imp, 0, calldatasize(), 0, 0)
            // returndatacopy(t, f, s)
            // copy s bytes from returndata at position f to mem at position t
            returndatacopy(0, 0, returndatasize())
            switch result
            case 0 {
                // revert(p, s)
                // end execution, revert state changes, return data mem[p…(p+s))
                revert(0, returndatasize())
            }
            default {
                // return(p, s)
                // end execution, return data mem[p…(p+s))
                return(0, returndatasize())
            }
        }
    }

    fallback() external payable {
        _delegate(implementation());
    }
}

contract V1 {
    uint256 public x;

    function inc() external {
        x += 1;
    }
}

contract V2 {
    uint256 public x;

    function inc() external {
        x += 1;
    }

    function dec() external {
        x -= 1;
    }
}

```

