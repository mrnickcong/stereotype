# Redis分布式锁

- [Redis分布式锁](#redis分布式锁)
  - [Redis分布式锁使用场景](#redis分布式锁使用场景)
  - [Redis分布式锁的实现方案](#redis分布式锁的实现方案)
    - [方案一：SETNX + EXPIRE](#方案一setnx--expire)
    - [方案二：SETNX + value值是（系统时间+过期时间）](#方案二setnx--value值是系统时间过期时间)
    - [方案三：使用Lua脚本(包含SETNX + EXPIRE两条指令)](#方案三使用lua脚本包含setnx--expire两条指令)
    - [方案四：SET的扩展命令（SET EX PX NX）](#方案四set的扩展命令set-ex-px-nx)
    - [方案五：SET EX PX NX  + 校验唯一随机值,再释放锁](#方案五set-ex-px-nx---校验唯一随机值再释放锁)
    - [方案六: 开源框架:Redisson](#方案六-开源框架redisson)
    - [方案七：多机实现的分布式锁Redlock](#方案七多机实现的分布式锁redlock)
  - [Redis分布式锁的八大坑](#redis分布式锁的八大坑)
    - [非原子操作](#非原子操作)
    - [Redis如何保证不释放别人的锁](#redis如何保证不释放别人的锁)

## Redis分布式锁使用场景

它是控制分布式系统之间互斥访问共享资源的一种方式

## Redis分布式锁的实现方案

- 方案一：SETNX + EXPIRE
- 方案二：SETNX + value值是（系统时间+过期时间）
- 方案三：使用Lua脚本(包含SETNX + EXPIRE两条指令)
- 方案四：SET的扩展命令（SET EX PX NX）
- 方案五：SET EX PX NX  + 校验唯一随机值,再释放锁
- 方案六: 开源框架:Redisson
- 方案七：多机实现的分布式锁Redlock

### 方案一：SETNX + EXPIRE

即先用setnx来抢锁，如果抢到之后，再用expire给锁设置一个过期时间，防止锁忘记了释放。

SETNX 是SET IF NOT EXISTS的简写.日常命令格式是SETNX key value，如果 key不存在，则SETNX成功返回1，如果这个key已经存在了，则返回0。

```java
if（jedis.setnx(key_resource_id,lock_value) == 1）{ //加锁
    expire（key_resource_id，100）; //设置过期时间
    try {
        do something  //业务请求
    }catch(){
　　}
　　finally {
       jedis.del(key_resource_id); //释放锁
    }
}
```

- 这个方案中，setnx和expire两个命令分开了，不是原子操作。
- 如果执行完setnx加锁，正要执行expire设置过期时间时，进程crash或者要重启维护了，那么这个锁就会一直存在不会释放，别的线程永远获取不到锁啦。

### 方案二：SETNX + value值是（系统时间+过期时间）

把过期时间放到setnx的value值里面，如果加锁失败，再拿出value值校验一下即可

```java
long expires = System.currentTimeMillis() + expireTime; //系统时间+设置的过期时间
String expiresStr = String.valueOf(expires);

// 如果当前锁不存在，返回加锁成功
if (jedis.setnx(key_resource_id, expiresStr) == 1) {
        return true;
} 
// 如果锁已经存在，获取锁的过期时间
String currentValueStr = jedis.get(key_resource_id);

// 如果获取到的过期时间，小于系统当前时间，表示已经过期
if (currentValueStr != null && Long.parseLong(currentValueStr) < System.currentTimeMillis()) {

     // 锁已过期，获取上一个锁的过期时间，并设置现在锁的过期时间（不了解redis的getSet命令的小伙伴，可以去官网看下哈）
    String oldValueStr = jedis.getSet(key_resource_id, expiresStr);
    
    if (oldValueStr != null && oldValueStr.equals(currentValueStr)) {
         // 考虑多线程并发的情况，只有一个线程的设置值和当前值相同，它才可以加锁
         return true;
    }
}
        
//其他情况，均返回加锁失败
return false;
}
```

- 解决了方案一，发生异常锁得不到释放的场景，
- 过期时间是客户端自己生成的（System.currentTimeMillis()是当前系统的时间），必须要求分布式环境下，每个客户端的时间必须同步。
- 如果锁过期的时候，并发多个客户端同时请求过来，都执行jedis.getSet()，最终只能有一个客户端加锁成功，但是该客户端锁的过期时间，可能被别的客户端覆盖
- 该锁没有保存持有者的唯一标识，可能被别的客户端释放/解锁

### 方案三：使用Lua脚本(包含SETNX + EXPIRE两条指令)

使用Lua脚本来保证原子性（包含setnx和expire两条指令）

lua脚本如下：

```java
if redis.call('setnx',KEYS[1],ARGV[1]) == 1 then
   redis.call('expire',KEYS[1],ARGV[2])
else
   return 0
end;
```

加锁代码如下：

```java
String lua_scripts = "if redis.call('setnx',KEYS[1],ARGV[1]) == 1 then" +
            " redis.call('expire',KEYS[1],ARGV[2]) return 1 else return 0 end";   
Object result = jedis.eval(lua_scripts, Collections.singletonList(key_resource_id), Collections.singletonList(values));
//判断是否成功
return result.equals(1L);
```

### 方案四：SET的扩展命令（SET EX PX NX）

Redis的SET指令扩展参数！（SET key value[EX seconds][PX milliseconds][NX|XX]），它也是原子性的

`SET key value[EX seconds][PX milliseconds][NX|XX]`

- NX :表示key不存在的时候，才能set成功，也即保证只有第一个客户端请求才能获得锁，而其他客户端请求只能等其释放锁，才能获取。
- EX seconds :设定key的过期时间，时间单位是秒。
- PX milliseconds: 设定key的过期时间，单位为毫秒
- XX: 仅当key存在时设置值

伪代码demo如下：

```java
if（jedis.set(key_resource_id, lock_value, "NX", "EX", 100s) == 1）{ //加锁
    try {
        do something  //业务处理
    }catch(){
　　}
　　finally {
       jedis.del(key_resource_id); //释放锁
    }
}
```

这个方案还是可能存在问题：

- 问题一：锁过期释放了，业务还没执行完。锁无法续约，导致临界区的业务代码非串行执行。
- 问题二：锁被别的线程误删。临界区业务代码没有执行完成，锁被其他线程释放

### 方案五：SET EX PX NX  + 校验唯一随机值,再释放锁

既然锁可能被别的线程误删，那我们给value值设置一个标记当前线程唯一的随机数，在删除的时候，校验一下，不就OK了嘛。

伪代码如下：

```java
if（jedis.set(key_resource_id, uni_request_id, "NX", "EX", 100s) == 1）{ //加锁
    try {
        do something  //业务处理
    }catch(){
　　}
　　finally {
       //判断是不是当前线程加的锁,是才释放===>注意此处非原子性操作
       if (uni_request_id.equals(jedis.get(key_resource_id))) {
        jedis.del(lockKey); //释放锁
        }
    }
}
```

在这里，判断是不是当前线程加的锁和释放锁不是一个原子操作。如果调用jedis.del()释放锁的时候，可能这把锁已经不属于当前客户端，会解除他人加的锁。

为了更严谨，一般也是用lua脚本代替。lua脚本如下：

```java
if redis.call('get',KEYS[1]) == ARGV[1] then 
   return redis.call('del',KEYS[1]) 
else
   return 0
end;
```

该方案存在的问题：还是可能存在锁过期释放，业务没执行完的问题

### 方案六: 开源框架:Redisson

![Redisson底层原理图](./images/Redisson%E5%BA%95%E5%B1%82%E5%8E%9F%E7%90%86%E5%9B%BE.png)

只要线程一加锁成功，就会启动一个watch dog看门狗，它是一个后台线程，会每隔10秒检查一下，如果线程1还持有锁，那么就会不断的延长锁key的生存时间。因此，Redisson就是使用Redisson解决了锁过期释放，业务没执行完问题。

### 方案七：多机实现的分布式锁Redlock

前面六种方案都只是基于单机版的讨论，还不是很完美。其实Redis一般都是集群部署的：

![多机实现的分布式锁Redlock+Redisson](./images/多机实现的分布式锁Redlock+Redisson.png)

如果线程一在Redis的master节点上拿到了锁，但是加锁的key还没同步到slave节点。恰好这时，master节点发生故障，一个slave节点就会升级为master节点。线程二就可以获取同个key的锁啦，但线程一也已经拿到锁了，锁的安全性就没了。
为了解决这个问题，Redis作者 antirez提出一种高级的分布式锁算法：Redlock。Redlock核心思想是这样的：

搞多个Redis master部署，以保证它们不会同时宕掉。并且这些master节点是完全相互独立的，相互之间不存在数据同步。同时，需要确保在这多个master实例上，是与在Redis单实例，使用相同方法来获取和释放锁。

**红锁并非是一个工具，而是redis官方提出的一种分布式锁的算法**

我们假设当前有5个Redis master节点，在5台服务器上面运行这些Redis实例。

![Redis集群分布式锁](./images/Redis集群分布式锁.png)

RedLock的实现步骤:如下

1. 获取当前时间，以毫秒为单位。
2. 按顺序向5个master节点请求加锁。客户端设置网络连接和响应超时时间，并且超时时间要小于锁的失效时间。（假设锁自动失效时间为10秒，则超时时间一般在5-50毫秒之间,我们就假设超时时间是50ms吧）。如果超时，跳过该master节点，尽快去尝试下一个master节点。
3. 客户端使用当前时间减去开始获取锁时间（即步骤1记录的时间），得到获取锁使用的时间。当且仅当超过一半（N/2+1，这里是5/2+1=3个节点）的Redis master节点都获得锁，并且使用的时间小于锁失效时间时，锁才算获取成功。（如上图，10s> 30ms+40ms+50ms+4m0s+50ms）
如果取到了锁，key的真正有效时间就变啦，需要减去获取锁所使用的时间。
如果获取锁失败（没有在至少N/2+1个master实例取到锁，有或者获取锁时间已经超过了有效时间），客户端要在所有的master节点上解锁（即便有些master节点根本就没有加锁成功，也需要解锁，以防止有些漏网之鱼）。

简化下步骤就是：

1. 按顺序向5个master节点请求加锁
2. 根据设置的超时时间来判断，是不是要跳过该master节点。
3. 如果大于等于三个节点加锁成功，并且使用的时间小于锁的有效期，即可认定加锁成功啦。
如果获取锁失败，解锁！

Redisson实现了redLock版本的锁，有兴趣的小伙伴，可以去了解一下哈~

## Redis分布式锁的八大坑

![Redis分布式锁的八大坑](./images/Redis分布式锁的八大坑.png)

### 非原子操作

### Redis如何保证不释放别人的锁