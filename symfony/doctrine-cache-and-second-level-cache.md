# doctrine 的缓存以及二级缓存

**我不能保证此文档的正确性，但是如果发现问题会第一时间修改**

doctrine有专门的包管理缓存，`doctrine/cache`，官方文档参见 ： [doctrine cache](http://doctrine-orm.readthedocs.io/projects/doctrine-orm/en/latest/reference/caching.html) 

doctrine提供了多种缓存的实现，其中包括了'APC', 'Memcache', 'Memcached', 'Xcache', 'Redis', 甚至还有一个'ArrayCache'，该缓存不会在多个请求之间共享，因此可用于测试环境。

以上缓存实现和使用方式都可以在官网上找到，这里重点要介绍的是与doctrine ORM 集合的三种缓存策略，即 'Query Cache', 'Result Cache', 'Metadata Cache'0，这三种缓存策略symfony下的doctrine配置下都可以找到相对应的配置项来配置(参见 [symfony 文档](http://symfony.com/doc/current/reference/configuration/doctrine.html#caching-drivers))：

```
doctrine:
    orm:
        auto_mapping: true
        metadata_cache_driver: apc
        query_cache_driver:
            type: service
            id: my_doctrine_common_cache_service
        result_cache_driver:
            type: memcache
            host: localhost
            port: 11211
            instance_class: Memcache
```

下面详细介绍这三种缓存策略。

## Query Cache

query cache会将DQL转换为SQL的副本缓存起来，因为相同的DQL生成的sql都是一样的，因此可以减少从DQL解析生SQL的时间，官方说非常推荐在正式环境中使用。

## Result Cache

result cache会将每次从数据库查询的结果缓存起来，因此之后无需再从数据库获取数据，这应该就是我们以为的“真正意义上的缓存”吧，文档介绍了多种result cache的操作，比如设置cache id,过期时间等
但是目前没有在symfony的queryBuilder中看到，暂时略过。

## Metadata Cache

metadata使用orm的人都知道，而 metadata会将entity的定义, 即symfony中定义的entity文件缓存起来。
