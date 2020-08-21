title: elasticsearch
author: hero576
date: 2020-07-30 19:02:54
tags:
---
> elasticsearch

<!--more-->

# 环境配置

## elasticsearch
### 依赖
- 是java开发的，需要java环境，1.8.0以上。
- nodejs

- 安装完成后启动，elasticsearch默认在localhost:9200建立服务。

## kibana
- 用于数据的统计、操作

- 安装完成后启动，kibana默认在localhost:5601建立服务。


## ik
- [下载地址](https://github.com/medcl/elasticsearch--analysis-il/release)
- 中文分词，放在elasticsearch中plugins里。


# kibana操作指令
## 增删改查

### GET
- GET获取数据
```
#获取所有数据
GET /*
# 中文分词
GET /_analyze
{
    "analyzer":"ik_max_word",
    "text":"我是"
}
#查看索引
GET _cat/indices?v
#查询-restful风格
GET test1/type1/_search?q=age:12
#查询-构建方式
GET test1/type1/_search
{
    "query":{
        "match":{
            "age":12
        }
    }
}
```


### PUT
- 没有插入数据，否则更新，字段全部变为当前数据
```
# 索引/类型/uuid
PUT test1/type1/1
{
    "name":"key",
    "age":12
}
# 查询
GET test1/type1/1
```

### POST
- 通过`_update`，只修改指定字段
```
POST test1/type1/1/_update
{
    "age":13
}
# 查询
GET test1/type1/1
```

### DELETE
```
DELETE /test1/type1/1
DELETE /test1
```

## 构造查询
- 构造查询可以实现复杂查询需求

### 全部查询
```
GET goods/fruit/_search
{
    "query":{
        "match_all":{}
    }
}
```

### 排序查询
- 只能选择可排序的属性：日期，数字

```
GET goods/fruit/_search
{
    "query":{
        "match":{
            "name":"pingguo"
        }
    },
    "sort":[
        {
            "price":"desc"
        }
    ]
}
```

### 分页查询
```
GET goods/fruit/_search
{
    "query":{
        "match":{
            "name":"pingguo"
        }
    },
    "sort":[
        {
            "price":"desc"
        }
    ],
    "from":1,
    "size":2
}
```

### 指定内容查询
```
GET goods/fruit/_search
{
    "query":{
        "match_all":{}
    },
    "_source":"name"
}
```

- 指定多个
```
GET goods/fruit/_search
{
    "query":{
        "match_all":{}
    },
    "_source":
    [
        "name",
        "age"
    ]
}
```

### bool查询
- must：多个条件相互与
- should：多个条件相互或
- must_not：多个条件非

```
GET goods/fruit/_search
{
    "query":{
        "bool":{
            "must":[
                {
                    "match":{
                        "name":"pingguo"
                    }
                },
                {
                    "match":{
                        "producer":"zhongguo"
                    }
                }
            ]
        }
    }
}
```

### 条件过滤查询
- filter属于bool查询内
- gt/gte/lt/lte
- 对于should做过滤条件，那么返回的结果中，_score的值和是否匹配到should有关，0代表不匹配。一般建议使用must

```
GET goods/fruit/_search
{
    "query":{
        "bool":{
            "must":[
                {
                    "match":{
                        "name":"pingguo"
                    }
                }
            ],
            filter:{
                "range":{
                    "price":{
                        "gt":100
                    }
                }
            }
        }
    }
}
```

### 查询列表数据
- 多个词检索，使用空格分隔

```
GET goods/fruit/_search
{
    "query":{
        "match":{
            "tags":"haochi lv"
        }
    }
}
```


### 短语检索
- 相比于match的方式，在检索内容中的精度更高，必须是匹配的词语
```
GET goods/fruit/_search
{
    "query":{
        "match_phrase":{
            "name":"pingguo"
        }
    }
}
```

### 短语前缀
- 类似于mysql： where name like pingguo%
```
GET goods/fruit/_search
{
    "query":{
        "match_phrase_prefix":{
            "name":"pingguo"
        }
    }
}
```

### 高亮检索
- 前置标签和后置标签分开写

```
GET goods/fruit/_search
{
    "query":{
        "match":{
            "name":"pingguo"
        }
    },
    "highline":{
        "pre_tags":"<b style='color:red'>",
        "post_tags":"</b>",
        "fields":{
            "name":{}
        }
    }
}
```


### 聚合函数
- avg、mean、sum、min、max
- 可以设置size:0使结果不显示数据，仅显示聚合结果

```
GET goods/fruit/_search
{
    "from":0,
    "size":10,
    "aggs":{
        "avg_xxx":{  # 查询结果别名
            "avg":{
                "field":"price"
            }
        }
    }
}
```

- 带查询条件的聚合函数
```
GET goods/fruit/_search
{
    "from":0,
    "size":10,
    "query":{
        "bool":{
            "must":[
                {
                    "match":{
                        "name":"pingguo"
                    }
                }
            ],
            filter:{
                "range":{
                    "price":{
                        "gt":100
                    }
                }
            }
        }
    }
    "aggs":{
        "avg_xxx":{  # 查询结果别名
            "avg":{
                "field":"price"
            }
        }
    }
}
```

- 分段聚合函数
```
GET goods/fruit/_search
{
    "from":0,
    "size":10,
    "aggs":{
        "avg_xxx":{  # 查询结果别名
            "range":{
                "field":"price",
                "ranges":[
                    {
                        "from":0,
                        "to":50
                    }
                ]
            },
            aggs:{
                "sum_price":{
                    "sum":{
                        "field":"price"
                    }
                }
            }
        }
    }
}
```

# mapping
- mapping的作用就是指定当前字段的类型和大小
- mapping查询
```
GET goods/fruit/_mapping
```

- mapping创建使用
```
PUT my_index1
{
    "mappings":{
        "xxx":{   #别名
            "dynamic":false,
            "properties":{
                "myname":{
                    "type":"text",
                    "copy_to":"full_name",
                    "index":"true"  # 设置索引
                },
                "age":{
                    "type":"long"
                    "index":"false"  # 设置不创建索引，查询会报错
                },
                "full_name":{
                    "type":"text"
                }
            }
        }
    }
}
# 放入数据
PUT my_index1/xxx/1
{
    "myname":"rrr",
    "age":123,
    "gender":"male"
}
```

- 配置`"dynamic":false`时，可插入非定义字段，不会建立索引，下面语句查询结果为空
- 配置`"dynamic":true`时，可插入非定义字段，会动态建立索引
- 配置`"dynamic":strict`时，不可插入非定义字段，会报错
```
GET goods/fruit/_search
{
    "query":{
        "match":{
            "gender":"male"
        }
    }
}
```

- 配置`"copy_to":"full_name"`指定字段赋值给full_name字段，full_name会包含有自己的值和拷贝的值

- 设置是否添加索引`"index":"false"`


# lk分词
- 建立字段，添加分析器
- `ik_max_word`是按照词来分，不配置时使用默认分析器，会按照字来分
- `ik_max_word`匹配的更多
- 可以使用title作为细颗粒的检索，content用分词的检索，效率会高一些。
```
PUT my_index1
{
    "mappings":{
        "xxx":{   #别名
            "dynamic":false,
            "properties":{
                "myname":{
                    "type":"text",
                    "analyzer":"ik_max_word"   
                }
            }
        }
    }
}
PUT my_index2
{
    "mappings":{
        "xxx":{   #别名
            "dynamic":false,
            "_all":{  # 为所有字段增加解析方式
                "analyzer":"ik_smart"   
            }
            "properties":{
                "myname":{
                    "type":"text"
                }
            }
        }
    }
}
```

- 查询
```
PUT my_index1/doc/1
{
    "myname":"非公开飞机"
}
GET my_index1/_search
{
    "query":{
        "match":{
            "myname":"飞机"
        }
    }
}
```

# python操作
- [官方文档](https://elasticsearch-py.readthedocs.io/en/master)

## 安装
```
pip install elasticsearch
```

## 链接

```
from elasticsearch import Elasticsearch
# 链接
es = Elasticsearch(['127.0.0.1:9200])
# 创建索引
es.indices.create(index="es_test_01",ignore=400)
```

## 基本操作
-  删除索引
```
es.indices.delete(index="es_test_01")
```

- 插入数据
```
es.index(index="es_test_01",doc_type="doc",id=1,body={"name":"dasf","age":123}) # 没有配置id，则会默认配置随机id
```

- 查询
```
res = es.get(index="es_test_01",doc_type="doc",id=1)
res = es.search(index="es_test_01",doc_type="doc",body={"query":{"match_all":{}}})
```

-  删除数据
```
es.delete(index="es_test_01",doc_type="doc",id=1)
es.delete(index="es_test_01",doc_type="doc",body={"query":{"match_all":{}}})
```


