# 汇编

## 一个简单的汇编合约代码

```text
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
            //加载0x40处的指针
            let free_pointer := mload(0x40)
            //保存值
            mstore(free_pointer, est)
            //堆栈上的数据无法直接返回，需要存储在内存中，才能返回
            return(free_pointer, 32)
        }
    }
}

```

## EVM的操作码 Ethereum Virtual Machine Opcodes

```text
00	01	02	03	04	05	06	07	08	09	0A	0B	-	-	-	-
10	11	12	13	14	15	16	17	18	19	1A	1B	1C	1D	-	-
20	-	-	-	-	-	-	-	-	-	-	-	-	-	-	-
30	31	32	33	34	35	36	37	38	39	3A	3B	3C	3D	3E	3F
40	41	42	43	44	45	46	47	48	-	-	-	-	-	-	-
50	51	52	53	54	55	56	57	58	59	5A	5B	-	-	-	-
60	61	62	63	64	65	66	67	68	69	6A	6B	6C	6D	6E	6F
70	71	72	73	74	75	76	77	78	79	7A	7B	7C	7D	7E	7F
80	81	82	83	84	85	86	87	88	89	8A	8B	8C	8D	8E	8F
90	91	92	93	94	95	96	97	98	99	9A	9B	9C	9D	9E	9F
A0	A1	A2	A3	A4	-	-	-	-	-	-	-	-	-	-	-
B0	B1	B2	-	-	-	-	-	-	-	-	-	-	-	-	-
-	-	-	-	-	-	-	-	-	-	-	-	-	-	-	-
-	-	-	-	-	-	-	-	-	-	-	-	-	-	-	-
-	-	-	-	-	-	-	-	-	-	-	-	-	-	-	-
F0	F1	F2	F3	F4	F5	-	-	-	-	FA	-	-	FD	-	FF
```


## 常见的EVM操作码

### 操作 all data

CALLDATALOAD, CALLDATASIZE, CALLDATACOPY

### 操作 stack

PUSH1, DUP1, SWAP1, POP

### 操作 memory

MLOAD, MSTORE, MSTORE8

### 操作 storage

SLOAD, SSTORE

## 常见opcodes

|uint8|Mnemonic|Stack Input|Stack Output|Expression|Notes|
|:--:|:--:|:--:|:--:|:--:|:--:|
|1|1|1|1|1|1|
|1|1|1|1|1|1|




