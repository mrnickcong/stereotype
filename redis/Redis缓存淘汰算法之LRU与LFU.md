# Redis缓存淘汰算法之LRU与LFU

- [Redis缓存淘汰算法之LRU与LFU](#redis缓存淘汰算法之lru与lfu)
  - [Redis键的过期删除与内存淘汰策略](#redis键的过期删除与内存淘汰策略)
  - [Redis缓存淘汰策略](#redis缓存淘汰策略)
  - [LRU算法与LFU算法](#lru算法与lfu算法)
    - [LRU算法](#lru算法)
      - [LRU算法简介](#lru算法简介)
      - [LRU算法的一般实现](#lru算法的一般实现)
        - [1. 链表实现简单LRU](#1-链表实现简单lru)
        - [2. HashMap和双向链表实现高性能LRU](#2-hashmap和双向链表实现高性能lru)
        - [3. 继承LinkedHashMap实现LRU](#3-继承linkedhashmap实现lru)
    - [LFU算法](#lfu算法)
      - [LFU算法简介](#lfu算法简介)
      - [LFU算法的一般实现](#lfu算法的一般实现)
    - [LRU算法与LFU算法的区别](#lru算法与lfu算法的区别)

## Redis键的过期删除与内存淘汰策略

- 内存淘汰策略：内存使用到达上限，基于LRU算法与LFU算法进行删除
- 过期删除策略：通过定期删除+惰性删除两者结合的方式进行过期删除

## Redis缓存淘汰策略

当内存达到极限时，Redis就要开始利用回收策略对内存进行回收释放。

回收的配置在 redis.conf 中填写，如下：

```text
maxmemory 1073741824
maxmemory-policy noeviction
maxmemory-samples 5
```

1. maxmemory： 指定了内存使用的极限，以字节为单位。当内存达到极限时，他会尝试去删除一些键值。
2. maxmemory-policy：指定删除的策略。Redis提供了如下几种缓存淘汰策略的取值
   1. noeviction：当内存使用超过配置的时候（如SET、LPUSH 等等命令）会返回错误，不会驱逐任何键。
   2. allkeys-lru：加入键的时候，如果过限，首先通过LRU算法驱逐最久没有使用的键
   3. volatile-lru：加入键的时候如果过限，首先从设置了过期时间的键集合中驱逐最久没有使用的键
   4. allkeys-random：加入键的时候如果过限，从所有key随机删除
   5. volatile-random：加入键的时候如果过限，从过期键的集合中随机驱逐
   6. volatile-ttl：从配置了过期时间的键中驱逐过期时间最近 (TTL 最小)的键
   7. volatile-lfu：从所有配置了过期时间的键中驱逐使用频率最少的键
   8. allkeys-lfu：从所有键中驱逐使用频率最少的键
3. maxmemory-samples ：指定了在进行删除时的键的采样数量。
   1. LRU 和 TTL 都是近似算法，所以可以根据参数来进行取舍，到底是要速度还是精确度。
   2. 默认值一般填 `5`。
   3. `10` 的话已经非常近似正式的 LRU 算法了，但是会多一些 CPU 消耗；
   4. `3` 的话执行更快，然而不够精确。

上述说到的缓存淘汰策略中，带lru后缀的，就是采用Redis LRU算法的策略，带有lfu后缀的策略，就是采用Redis LFU算法的策略。

## LRU算法与LFU算法

### LRU算法

#### LRU算法简介

- LRU算法（Least recently used）
- 其核心思想是`如果数据最近被访问过，那么将来被访问的几率也更高`。
- 它经常使用在内存/缓存空间不足的场景，以便在受限时舍弃掉不常用的数据。

#### LRU算法的一般实现

1. 链表实现简单LRU
2. HashMap和双向链表实现高性能LRU
3. 继承LinkedHashMap实现LRU

##### 1. 链表实现简单LRU

使用链表，可以实现最简单的LRU算法：

1. 维护一个定长的链表
2. 当一个新的key被访问时
   1. 如果这个key不存在链表中，那么新key插入到链表头部；
   2. 如果这个key存在链表中，那么将这个key移到链表头部；
3. 当链表满的时候，如果还有新的key要插入，则将链表尾部的key丢弃。

- 实现了最简单的LRU算法
- 这种实现的性能不是很好，查询一个key是否存在链表中，以及在链表中的具体位置的时间复杂度是O(n)
- 这在数据数量巨大的场景下是灾难的。

![LRU算法之链表实现](./images/LRU算法之链表实现.png)

##### 2. HashMap和双向链表实现高性能LRU

链表实现的LRU算法瓶颈主要在定位一个key在链表中位置的消耗。

为了规避这个代价，我们可以引入在查询和定位方面具有极高优势的HashMap来作为互补，整体的设计思路是，可以使用 HashMap 存储 key，而HashMap的Value指向双向链表实现的LRU的 Node 节点。这样可以做到save和get的时间都是 O(1)。

![LRU算法之HashMap和双向链表高性能实现](./images/LRU算法之HashMap和双向链表高性能实现.png)

假如我们预设链表的大小是3，下图展示了LRU链表在存储和访问过程中的变化。为了简化图复杂度，图中没有展示HashMap部分的变化，仅仅演示了上图LRU双向链表的变化。

![LRU算法之HashMap和双向链表实现-数据存储变化](./images/LRU算法之HashMap和双向链表实现-数据存储变化.png)

##### 3. 继承LinkedHashMap实现LRU

LinkedHashMap底层就是用的HashMap加双链表实现的，而且本身已经实现了按照访问顺序的存储（也就是其put方法会将最近访问的数据放到表头）。

![LRU算法之LinkedHashMap实现](./im2)

此外，LinkedHashMap中本身就实现了一个方法removeEldestEntry，用于在每次数据发生变更时（put和get）判断是否需要移除最不常读取的数，方法默认是直接返回false，不会移除元素（也正因此，LinkedHashMap是无限长的）。所以为了将其改造为一个定长且会自动移除队尾数据的链表，需要重写removeEldestEntry方法，即当缓存满后就移除最不常用的数。

public class LRUCache<K, V> extends LinkedHashMap<K, V> {

    private final int CACHE_SIZE;

    // 这里就是传递进来最多能缓存多少数据
    public LRUCache(int cacheSize) {
        // 设置一个hashmap的初始大小，最后一个true指的是让linkedhashmap按照访问顺序来进行排序，最近访问的放在头，最老访问的就在尾
        super((int) Math.ceil(cacheSize / 0.75) + 1, 0.75f, true);
        CACHE_SIZE = cacheSize;
    }

    @Override
    protected boolean removeEldestEntry(Map.Entry eldest) {
        // 当map中的数据量大于指定的缓存个数的时候，就自动删除最老的数据
        return size() > CACHE_SIZE;
    }





### LFU算法

#### LFU算法简介

- LFU算法（Least Frequently used），两者看起来很相似，但我们要明确其

#### LFU算法的一般实现

- 使用双哈希表实现高性能LFU
- 
### LRU算法与LFU算法的区别

- LRU是按访问时间排序，发生淘汰的时候，把访问时间最旧的淘汰掉。
- LFU是按频次排序，一个数据被访问过，把它的频次+1，发生淘汰的时候，把频次低的淘汰掉

