#redis-py学习笔记
***
##1.连接池
pool = redis.ConnectionPool(host="localhost", port=6379, db=0)
r = redis.Redis(connection_pool=pool)

##2.连接
r = redis.Redis(unix_socket_path='/tmp/redis.sock')
pool = redis.ConnectionPool(connection_class=YourConnectionClass, your_arg='')

##3.反馈回调 回调函数用途
每一个redis实例都可以通过set_response_callback设置自定义回调函数, 

##4.线程安全
Redis的客户端实例是线程安全的, 命令执行从不修改客户端实例的内部状态

>1. SELECT 用来切换指定的数据库, 该操作不是线程安全的redis-py中没有实现
>2. PubSub和Pipleline实例不能在不同线程间共享

##5.Pipelines
Piplelines是Redis类的子类用来缓存多条命令到服务器作为一个请求, 能够极大的提升性能

`
    r = redis.Redis(...)
    r.set("bing", "baz")
    pipe = r.pipeline()
    pipe.set("foo", "bar")
    pipe.get("bing")
    pipe.execute()
`
pipelines能够包成缓存的命令能够作为一组命令原子的执行
`
    pipe = r.pipeline(transaction=False) #关闭事务
`

WATCH: 提供了在启动一个事务之前监控一个或多个键的能力, 如果在执行事务之前有
    键发生了改变, 那么整个事务将被取消, WatchError将被扔出
使用WATCH和pipleline实现一个原子操作
`
    with r.pipline() as pipe:
        while 1:
            try:
                pip.watch("key")
                current_value = pip.get("key")
                next_value = int(current_value) + 1
                pipe.multi()
                pipe.set("key", next_value)
                pipe.execute()
                break;
            except WatchError:
                continue

##6.Publish/Subscribe
`
    r = redis.StrictRedis()
    p = r.pubsub()
    p.subscribe("my-first-channel", "my-second-channel", ...)
    p.psubscribe("my-*", ...)
    p.get_message()
`

忽略订阅信息
r.pubsub(ignore_subscribe_messages=True)
