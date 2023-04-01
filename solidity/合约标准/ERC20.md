# ERC20代币标准

## what

ERC-20 标准是在2015年11月份推出的，使用这种规则的代币，表现出一种通用的和可预测的方式。
ERC-20 标准规定了各个代币的基本功能，非常方便第三方使用，在开发人员的编程下，5 分钟就可以发行一个 ERC-20 代币。因为它可以快速发币，而且使用又方便，因此空投币和空气币基本上就是利用 ERC-20 标准开发的。基于 ERC-20 标准开发的同种代币价值都是相同的，它们可以进行互换。

## how

//这是

```text
contract ERC20 {

    //返回token的总供应量
    uint256 public totalSupply;

    //用于查询某个账户的账户余额
    function balanceOf(address who) constant public returns (uint256);

    //发送 _value 个 token 到地址 _to
    function transfer(address to, uint256 value) public returns (bool);

    //从地址 _from 发送 _value 个 token 到地址 _to
    function transferFrom(address from, address to, uint256 value) public returns (bool);
    
    //允许 _spender 多次取回您的帐户，最高达 _value 金额； 如果再次调用此函数，它将用 _value 的当前值覆盖的 allowance 值。
    function approve(address spender, uint256 value) public returns (bool);

    //返回 _spender 仍然被允许从 _owner 提取的金额。
    function allowance(address owner, address spender) constant public returns (uint256);

    //当 tokens 被转移时触发。
    event Transfer(address indexed from, address indexed to, uint256 value);

    //当任何成功调用 approve(address _spender, uint256 _value) 后，必须被触发。
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
