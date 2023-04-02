# Borabora项目

## 环境信息

### openzepelin

```text
// OpenZeppelin Contracts (last updated v4.7.0) (utils/Address.sol)
```

### bora合约源码版本

```text
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;
```

### golang版本

`0.12`

### NFT tokenuri

https://trade.borabora.ooo/api/ipfs/

## 功能实现

### 1、白名单签名授信

Solidity-链下签名签名-链上验证；链下使用 Ether.js 进行签名，链上使用 OpenZeppelin ECDSA 验证。

```text
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/utils/cryptography/ECDSA.sol";

contract Verification {
    address owner;
    using ECDSA for bytes32;

    constructor() {
        owner = msg.sender;
    }

    function isMessageValid(bytes memory _signature)
        public
        view
        returns (address, bool)
    {
        // 1. 对签名信息进行 abi 编码
        bytes memory abiEncode = abi.encodePacked("HelloWorld");

        // 2. 再进行 keccak256 Hash运算
        bytes32 messagehash = keccak256(abiEncode);
        
        // 3. 添加前缀，可以将计算出的以太坊特定的签名。这可以防止恶意 DApp 可以签署任意数据（例如交易）并使用签名来冒充受害者的滥用行为。
        bytes32 ethSignedMessageHash = ECDSA.toEthSignedMessageHash(messagehash);
        
        // 4. 从签名恢复地址
        address signer = ECDSA.recover(ethSignedMessageHash,_signature);

        if (owner == signer) {
            return (signer, true);
        } else {
            return (signer, false);
        }
    }
}
```

