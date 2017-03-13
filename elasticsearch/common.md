# 普通笔记

这里的笔记是简单无需分类的


## routing

这个不知翻译成“路由”到底好不好，因此，还是用routing来称呼吧。

下面简单写一些关于routing的说明，根据源文档翻译过来的，
原文链接: (routing)[https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-routing-field.html] 每一个指定了routing的文档会被指定到具体的分片中，根据以下规则指定文档所在的分片：

```
shard_num = hash(_routing) % num_primary_shards)
```

默认使用文档的id(_id)来当作routing的值


在创建文档的时候可以指定文档的routing:

```
PUT my_index/my_type/1?routing=user1&refresh=true 
{
  "title": "This is a document"
}
```

对于上诉文档，直接通过`GET my_index/my_type/1`, 是获取不到的, 必须指定routing: `GET my_index/my_type/1?routing=user1`, 但是可以通过search搜索得到：
`GET my_index/my_type/_search`


可以在创建mapping的时候给文档指定routing为必填项:
```
PUT my_index2
{
  "mappings": {
    "my_type": {
      "_routing": {
        "required": true 
      }
    }
  }
}
```
因此，当按一下方式创建文档时，则会报错：
```
PUT my_index2/my_type/1 
{
  "text": "No routing value provided"
}
```


我们并不能保证routing的唯一性，这个需要客户端来避免相同routing的发生
