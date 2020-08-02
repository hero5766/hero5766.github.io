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
        "avg_xxx":{
            "avg":{
                "field":"price"
            }
        }
    }
}
```


# maping

# lk分词

# python操作


