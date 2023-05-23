# BDD框架之JBeHave

[文章来源](https://blog.51cto.com/stringyc/1395506)

- BDD框架之JBeHave
  - [框架简介](#框架简介)
  - [实现步骤](#框架实现步骤)

## 框架简介

JBehave是一个行为驱动开发(BDD:Behaviour-Driven Development)的开源测试框架。BDD是测试驱动开发(TDD:Behaviour-Driven Development)和验收测试驱动设计(acceptance-test driven design)的一个演变，BDD会使这些实践更容易被接受，无论是菜鸟还是专家。BDD将基于测试的驱动转变为基于行为的驱动，并且BDD本身也将作为一个设计的理念。

DD 目的是使开发过程更加接受，并且更加直观，能很快的使新人像专家一样工作。在BDD中, 行为代表规格说明和测试用例。我们可以在wiki上找出更多关于BDD开发的内容或者关于介绍BDD的内容。

## 框架实现步骤

JBehave 实现五步骤：

1. 书写story
2. 匹配step到Java类
3. 配置Stories
4. 运行Stories
5. 观察报告Reports