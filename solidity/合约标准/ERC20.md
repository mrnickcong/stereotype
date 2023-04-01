# ERC20代币标准

这个文章写的好
https://learnblockchain.cn/article/4661

## what

ERC-20 标准是在2015年11月份推出的，使用这种规则的代币，表现出一种通用的和可预测的方式。
ERC-20 标准规定了各个代币的基本功能，非常方便第三方使用，在开发人员的编程下，5 分钟就可以发行一个 ERC-20 代币。因为它可以快速发币，而且使用又方便，因此空投币和空气币基本上就是利用 ERC-20 标准开发的。基于 ERC-20 标准开发的同种代币价值都是相同的，它们可以进行互换。

## how

//这是

```text
// SPDX-License-Identifier: MIT
//file IERC20.sol
pragma solidity ^0.8.0;

interface IERC20 {
    // 总发行量
    function totalSupply() external view returns (uint256);
    // 查看地址余额
    function balanceOf(address account) external view returns (uint256);
    /// 从自己帐户给指定地址转账
    function transfer(address account, uint256 amount) external returns (bool);
    // 查看被授权人还可以使用的代币余额
    function allowance(address owner, address spender) external view returns (uint256);
    // 授权指定帐户使用你拥有的代币
    function approve(address spender, uint256 amount) external returns (bool);
    // 从一个地址转账至另一个地址，该函数只能是通过approver授权的用户可以调用
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);

    /// 定义事件，发生代币转移时触发
    event Transfer(address indexed from, address indexed to, uint256 value);
    /// 定义事件 授权时触发
    event Approval(address indexed owner, address indexed spender, uint256 value);
}
```

# Q&A

## Q1：transfer 和 transferFrom 的区别

1. 都可以用户发送token
2. transfer 是自己调用，transferFrom 是别人调用。
    例如： 
    ​     A账号有10000个token代币,B账号没有token代币,C账号也没有token代币！A账号 委托 B账号 转给C账号 100个token代币 怎么来实现呢？
    ​     A账号 和 B账号建立一种委托关联,登录A账户执行approve(b,100)方法结果为：结果：allowance[A][B] = 100token
   在执行登录B账户执行transferFrom(A,C,100),这里的B就是委托账号发送者,gas从B扣,必须确保token数量小于allowance[A][B]
   其实就是A转入C,但是要经过B的账号来发送交易！

## Q2：ERC20的approve()函数被【抢先交易攻击】

值得一提的是ERC20的approve()函数存在安全隐患 (front-running attack)，并且该问题至今没有完全解决。可行的攻击场景如下：

1. Alice授权Bob可以挪用100个她的Token A. (tx1)
2. tx1被矿工确认后，Alice想把授权上限改为50个Token A. (tx2)
3. Bob探测到tx1已经确认，同时tx2还在pending状态，他给高额gas并调用transferFrom()函数直接在tx2被确认前从Alice账户转移了100个Token A. (tx3)
4. tx3先于tx2被确认，之后不久tx2也被确认，在Alice还没反应过来之前Bob立马再次调用transferFrom()又从Alice那转移了50个Token A。
5. 这样Bob一共从 Alice那转移了150个Token A，虽然Alice的本意是只希望授权50个给Bob挪用。

解决办法：

而且正如EIP issue里一个评论所提到的，一般用户调用approve(_spender, _value)的场景多是在信任_spender的前提下才会这么调用，而_spender多为交易所的智能合约，一般不会故意想要黑用户的币。然而这个历史遗留问题估计要等到下一版标准出来才有望彻底解决了。