#Redis学习笔记
***

##数据结构
###公共命令
    - EXISTS: EXISTS mykey 检测mykey是否存在
    - DEL: DEL mykey 删除mykey, 可以同时删除多个Key
    - TYPE: type mykey 查看mykey对应值的类型
    - EXPIRE: expire key 5 5s后key值过期,将会被自动删除
    - PERSIST: persist key 取消对某个键的过期设置, 不删除Key的情况下移除过期
    - TTL: ttl key  检测一个键过期剩余的时间
    - KEYS: keys pattern 匹配符合模式的key
            *,?,[~~]
    - FLUSHDB: 删除当前数据库中的所有key
    - RANDOMKEY: 随即返回一个key
    - RENAME: 重命名

###String
 说明: 字符键值对键和值的最大长度都是   512M, 键可以是二进制数据
 操作: 
    - SET: set mykey somevalue   设置一个键值对
           set mykey somevalue  nx   如果mykey存在则设置失败
           set mykey somevalue  xx   如果mykey不存在则设置失败
    - INCR: incr counter 将counter看做一个整数,原子增一
    - INCRBY: incr counter 10  原子的给counter增加10
    - DECR:
    - DECRBY:
    - GETSET: GETSET mykey somevalue 获取mykey的值并设置新值
    - MGET: mget key1 key2 一次获取多个值
    - MSET: mset key1 value1 key2 value2 一次设置多个值


        
###List
 说明: 使用链表实现
 操作:
    - RPUSH  
    - LPUSH
    - LLEN
    - LRANGE
    - LPOP
    - RPOP
    - LTRIM ltrim mylist 0 2 保留0-2的数据,删除其它数据
    - BLPOP: blpop tasks 5 阻塞5s等待tasks中有新数据 LPOP的阻塞版本
    - BRPOP
    - RPOPLPUSH
    - BRPOPLPUSH
###Set
 操作:
    - SADD
    - SREM
    - SISMEMBER
    - SMEMBERS
    - SUNION
    - SPOP
###Sorted Set
操作:
    - ZAD
    - ZRANGE
###Map
 操作:
    - HSET
    - HGETALL
    - HMSET
    - HGET
    - HINREBY
    - HDEL
###Bitmap
 操作:
    - SETBIT setbit key 10 1
    - GETBIT
    - BITPOS
    - BITOP
    - BITCOUN

slaveof ip port  当前redis成为指定ip和port的redis的slave
