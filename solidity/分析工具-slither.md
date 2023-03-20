# Slither

## 一、概述

Slither是一个用Python 3编写的智能合约静态分析框架（源码），提供如下功能：

自动化漏洞检测。提供超30多项的漏洞检查模型，模型列表详见：https://github.com/crytic/slither#detectors
自动优化检测。Slither可以检测编译器遗漏的代码优化项并给出优化建议。
代码理解。Slither能够绘制合约的继承拓扑图，合约方法调用关系图等，帮助开发者理解代码。
辅助代码审查。用户可以通过API与Slither进行交互。

## 二、Slither运行流程

Slither的工作方式如下：
1、智能合约源码经过solc编译后得到Solidity抽象语法树（AST）作为Slither的输入。
2、经过information recovery（数据整合），Slither生成合约的继承图，控制流图（CFG）以及合约中函数列表。
3、经过SlithIR转换，Slither将合约代码转换为SlithIR，一种内部表示语言，目的是通过简单的API实现高精度分析，支持污点和值的跟踪，从而支持检测复杂的模型。
4、在代码分析阶段，Slither运行一组预定义的分析，包括合约中变量、函数的依赖关系；变量的读写和函数的权限控制。
5、经过Slither的核心处理之后，就可以提供漏洞检测、代码优化检测和代码理解输出等。
论文——DIO:10.1109/WETSEB.2019.00008

## 三、安装

由于 Solidity 编译器版本有很多，有些sol文件使用的是老版的 Solidity 编译器版本，故需要进行版本的切换，将solc版本卸载，再重装所需要的版本是耗费时间并且显得很蠢。而solc-select是在 Solidity 编译器版本之间快速切换的工具，就很方便。安装solc-select之前，请先卸载本机的solc
————————————————
版权声明：本文为CSDN博主「想躺平的小陈」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_44217936/article/details/123240460