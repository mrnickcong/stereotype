# 使用OpenZeppelin编写可升级的智能合约

[使用OpenZeppelin编写可升级的智能合约](https://learnblockchain.cn/article/4282)

透明代理和UUPS

透明代理：
透明代理模式有一个缺点： gas 成本。每个调用都需要额外的从存储中加载admin地址

UUPS：通用可升级代理
将升级逻辑放在实现合约中，而不是在代理本身中。

可升级合约特点

1. 逻辑合约必须实现 `contract MyLogicV1 is Initializable, UUPSUpgradeable, OwnableUpgradeable`
2. 逻辑合约没有构造函数，初始化数据在initialize中实现
3. 逻辑合约的存储不能变化