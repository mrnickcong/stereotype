# 汇编





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




