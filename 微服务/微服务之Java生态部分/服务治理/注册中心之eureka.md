# 注册中心之eureka架构剖析

- [注册中心之eureka架构剖析](#注册中心之eureka架构剖析)
  - [Eureka架构](#eureka架构)
    - [Eureka注册发现基础架构](#eureka注册发现基础架构)
    - [Eureka原理架构](#eureka原理架构)
      - [Renew: 服务续约](#renew-服务续约)
      - [Cancel：服务剔除](#cancel服务剔除)
  - [eureka的服务注册、服务发现和健康检查](#eureka的服务注册服务发现和健康检查)
    - [服务注册 Eureka Server](#服务注册-eureka-server)
    - [服务发现 Eureka Client](#服务发现-eureka-client)
  - [Eureka自我保护](#eureka自我保护)
  - [Eureka缺陷](#eureka缺陷)

## Eureka架构

### Eureka注册发现基础架构

![Eureka注册发现基础架构](../../images/Eureka注册发现基础架构.png)

### Eureka原理架构

![Eureka原理架构图](../../images/Eureka原理架构图.png)

#### Renew: 服务续约

- Eureka Client 会每隔 30 秒发送一次心跳来续约。
- 通过续约来告知 Eureka Server 该 Eureka Client 运行正常，没有出现问题。
- 默认情况下，如果 Eureka Server 在 90 秒内没有收到 Eureka Client 的续约，Server 端会将实例从其注册表中删除，此时间可配置，一般情况不建议更改。

#### Cancel：服务剔除

- 如果Eureka Client在注册后，既没有续约，也没有下线(服务崩溃或者网络异常等原因)，那么服务的状态就处于不可知的状态，不能保证能够从该服务实例中获取到回馈，所以需要服务剔除此方法定时清理这些不稳定的服务，该方法会批量将注册表中所有过期租约剔除，
- 剔除是定时任务，默认60秒执行一次。延时60秒，间隔60秒
- 剔除的限制：
  - 1.自我保护期间不清除。
  - 2.分批次清除。

## eureka的服务注册、服务发现和健康检查

Eureka包含两个组件：Eureka Server和Eureka Client

### 服务注册 Eureka Server

- Eureka Server提供服务注册服务，
- 各个节点启动后，会在Eureka Server中进行注册，
- 这样EurekaServer中的服务注册表中将会存储所有可用服务节点的信息，服务节点的信息可以在界面中直观的看到。

### 服务发现 Eureka Client

- Eureka Client是一个java客户端，用于简化与Eureka Server的交互，
- 客户端同时也就是一个内置的、使用轮询(round-robin)负载算法的负载均衡器

## Eureka自我保护

![Eureka自我保护](../../images/Eureka自我保护.png)

## Eureka缺陷

由于集群间的同步复制是通过HTTP的方式进行，基于网络的不可靠性，集群中的Eureka Server间的注册表信息难免存在不同步的时间节点，不满足CAP中的C(数据一致性)
