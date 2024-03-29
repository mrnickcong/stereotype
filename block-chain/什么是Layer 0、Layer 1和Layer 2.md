# 什么是Layer 0、Layer 1和Layer 2

## 区块链的架构

区块链具有六层架构：数据层、网络层、共识层、激励层、合约层和应用层

数据层和网络层是区块链的基本架构，也是整个区块链系统的最底层。

基本架构之上，共识层、激励层、合约层和应用层共同构成了区块链的协议部分。

Layer 0又称数据传输层，对应OSI模型的底层，主要涉及区块链和传统网络之间的结合问题。

Layer 1扩容方案又称链上扩容，指在区块链基层协议上实现的扩容解决方案。

Layer 2扩容方案又称链下扩容，指不改变区块链底层协议和基础规则，通过状态通道、侧链等方案提高交易处理速度。

从区块链的六层架构说起
2009年1月，神秘极客中本聪在一台位于赫尔辛基的服务器上生成了比特币区块，目前，比特币已经发展成为一个在全球拥有数万节点，总市值超过1万亿美元的庞大系统。比特币完美解决了数字世界中的价值表示问题，也给人类来带了区块链技术。如果我们仔细分析比特币系统的结构，可以根据功能将之分为5层，分别是数据层、网络层、共识层、激励层和应用层。随后，凭借智能合约迅速崛起的以太坊又给区块链系统设立了新的范式，在系统的激励层和应用层之间加上了一个合约层。让我们从下到上仔细说明。

其中数据层和网络层是区块链的基本架构，也是整个区块链系统的最底层。

数据层（Data Layer）既包括每个区块的数据结构，如Merkle树，也包括各个不同区块是如何首尾相接、连接成链的。此外，数据层还包括为保证区块链不可篡改特性所采用的的哈希加密算法、非对称加密算法。区块链数据层可以视作一个具有分布式特征、不可篡改特性的数据库。这个分布式的数据库需要由系统的所有节点共同维护，这也就引出了区块链的网络层。

网络层（Netwrok Layer）则描述区块链所有节点构成的一个巨大的P2P网络。在这一分布式、点对点的网络中，一旦某一节点创造出了新的区块，就会通过广播机制将信息传输给附近的数个节点（传播机制，Transmission Mechanism）。其他节点在完成对区块的验证之后（Authentication Mechanism），又会再一次将数据传输给其他节点。最终，整个系统的大多数节点都完成对区块的验证之后，区块也就正式连接到了区块链上。

在基本架构之上，共识层、激励层、合约层和应用层共同构成了区块链的协议部分（Protocol Layer）。

共识层（Consensus Layer）主要包括区块链的共识机制算法。在区块链网络上，原本毫无关系的诸多节点正是通过共识算法达成彼此间的统一，维护数据层数据的一致性的。目前，较为常见的共识机制有比特币所采用的的工作量证明（Proof of Work, PoW），以太坊2.0所采用的权益证明（Proof of Stake, PoS），EOS所采用的委托权益证明（Delegated Proof of Stake, DPoS）等。共识机制是区块链技术的核心创新之一，显著影响了系统的安全性和运行效率。此外，共识算法还是区块链进行社区治理的主要手段。

激励层（Actuator Layer）主要包括区块链的发行机制（Issuing Mechanism）和分配机制（Distribution Mechanism）。通过激励机制的设计，系统中的节点将会自发地维护整个区块链网络的安全性。比如在比特币PoW机制中，新发行的比特币将会分配给打包节点的矿工。达成共识的方式接近“多劳多得”，算力越大的节点越有可能成功打包区块，也就能获取记账权。而在某些激励机制中，利用权力作恶的节点还会被系统惩罚。

比特币创造性的将经济激励机制融入到算法之中，形成了矿工通过算力竞争获取记账权，在维护了交易系统的同时发行了新的代币，新发代币又成为了分配给矿工的激励，由此形成了一个稳定、安全的闭环。在这一过程中，比特币作为电子现金的功能也得以实现。让我们继续向顶层前进。

合约层（Contract Layer）主要包括各种脚本代码、算法和只能合约，也是区块链实现诸多高级功能的基础。在区块链中，真正实现了所谓的”code is law“（代码即法则），合约算法一旦激活，就会不可避免地按照原有设置运行下去，而无需任何第三方的干预或推动。此外，由于智能合约具有图灵完备性，合约层还具有可编程性，这使得整个区块链网络具有了类似虚拟机的性质。

应用层（Application Layer）则是整个区块链系统的最上层，包含了该区块链的各种应用场景。对于比特币区块链来说，比特币这一具有完整发行、转账、记账功能电子现金系统便构成其“应用层”；而对于以太坊这样可编程的区块链来说，诸多高级功能、诸多DApp共同构成其应用层。

Layer 0, Layer 1和Layer 2
区块链系统的六个层次，在结构上都密不可分，共同实现了区块链的功能。再回到本文一开始所说的扩容问题，业界一般会参考通信界的开放式系统互联通信参考模型（Open System Interconnection Reference Model，OSI），将区块链系统的六个层次重新划入三个Layer，有底到顶分别为Layer 0、Layer 1和Layer 2。

其中，Layer 0又称数据传输层，对应OSI模型的底层，主要涉及区块链和传统网络之间的结合问题。而对应的Layer 0扩容方案，则是指那些不改变区块链构造，并保留其原有生态规则的性能提升方案。由于不会对区块链本身造成影响，Layer 0扩容方案具有较强的通用性，同时，Layer 0扩容方案也与Layer 1/2扩容方案相兼容，能够共同发挥作用，乘数倍的提高区块链网络的性能。在网络底层协议中，尚有很多影响性能的问题值得进行优化，目前已有的Layer 0扩容技术方向有BDN（分发网络）、QUIC UDP协议等。

除此之外，波卡也常被称为Layer 0级别的区块链。这是因为波卡主网作为中继链，只起到为各大平行链提供安全性和彼此间的互操作性的作用。而在波卡的基础上，可以通过插槽链接类似于以太坊这样的Layer 1区块链，比如同样支持Solidity语言的Moonbeam链。

Layer 1对应六层模型中的数据层、网络层、共识层、激励层。对于大多数加密货币而言，Layer 1都是其有且仅有一条的公链，所有交易结算都发生于其上。Layer 1扩容方案又称链上扩容，指在区块链基层协议上实现的扩容解决方案。一般需要修改区块链的区块容量、区块生成时间、共识机制等固有属性以提高交易能力。具体来说，比特币的扩块升级是增大了每一个比特币区块的容量，从而可以容纳更多数目的交易；比特币SegWit则减少了平均单笔交易所占用的空间，使得每一区块可以容纳更多数目交易；升级到DPoS也可以在牺牲一定程度去中心化和安全性的条件下获得更好的性能。但受到物理、经济等因素限制，Layer 1扩容的效率是有极限的。

关于Layer 1扩容的原理和局限性，请参考我们此前的博客文章：
V神反对马斯克的文章到底说了什么？扩容能让狗狗币成为“人民的货币”吗？

Layer 2则对应区块链的合约层和应用层，Layer 2扩容方案又称链下扩容，指不改变区块链底层协议和基础规则，通过状态通道、侧链等方案提高交易处理速度。Layer 2在主链外部进行扩容，与Layer 1是一个承接互补的关系，即Layer 2是构建在底层区块链之上的基础架构，为区块链提供更好的可扩展性、可用性以及隐私性。比起Layer 1追求安全性和去中心化，Layer 2追求的是极致的效率和性能。目前，常见的Layer 2解决方案有Side Chain，Plasma，状态通道，Rollup等。