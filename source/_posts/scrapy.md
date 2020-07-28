title: scrapy
author: hero576
tags:
  - python
categories:
  - programme
date: 2020-07-26 13:10:00
---
> scrapy记录

<!--more-->

# 简介
## 功能
- 持久化
- 数据下载
- 数据解析
- 分布式

## 安装

|安装||
|--|--|
| 执行安装命令|linux: `pip3 install scrapy`<br>windows `python -m pip install -U pip`<br>`pip intsall lxml`<br>`pip install wheel`<br>`wget https://download.lfd.uci.edu/pythonlibs/w3jqiv8s/Twisted-20.3.0-cp37-cp37m-win_amd64.whl`<br>`pip install Twisted-20.3.0-cp37-cp37m-win_amd64.whl`<br>`pip install pywin32`<br>`pip install scrapy`<br>|
| 检测是否安装成功|终端执行`scrapy`|

- 遇到
```bash
from cryptography.hazmat.bindings._openssl import ffi, lib
ImportError: DLL load failed: 找不到指定的程序。
```

- 需要重新安装openssl
```bash
 pip uninstall pyopenssl
 pip uninstall cryptography
 pip install pyopenssl
 pip install cryptography
```

## 简单使用

|简单使用||
|-|-|
|创建项目|`scrapy startproject 项目名称`|
|创建任务|`cd 项目名称`<br>`scrapy genspider 任务名称 www.baidu.com`|
|执行任务|`scrapy crawl 任务名称`|
|pycharm调试|`from scrapy import cmdline``cmdline.execute("scrapy crawl dmoz".split())`|

## 交互模式

- 进入交互模式：`scrapy shell quotes.toscrap.com`
- 执行命令：`quotes = response.css('.quote')`

## 指令汇总

|global指令||
|-|-|
|startproject|创建项目|
|genspider|创建spider|
|settings|获取配置信息|
|runspider|执行spider|
|shell|命令行交互模式|
|fetch|简单抓取某个网页|
|view|把网页保存成文件，再用浏览器打开|
|version|scrapy版本信息|

|项目指令||
|-|-|
|crawl|执行spider的方法|
|check|检查代码是否有错误|
|list|所有spider名称|
|edit|编辑spider|
|parse||
|bench|测试，运行的性能|

# 设置
- 设置settings.py文件

|设置示例||
|-|-|
|设置log级别|	`LOG_LEVEL = "WARNING”   # DEBUG INFO WARN ERROR FATAL`|
|项目名称|	`BOT_NAME=默认： 'scrapybot' 这将用于默认情况下构造 User-Agent，也用于日志记录。当您使用startproject命令创建项目时，它会自动填充您的项目名称。`|
|启用中间件	|设置请求头	`USER_AGENT = 'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2883.87 Safari/537.36'`|
|是否遵守robots协议，默认是遵守|	`ROBOTSTXT_OBEY = True`|
|设置并发请求的数量，默认是16个|`CONCURRENT_REQUESTS`|
|下载延迟，默认无延迟|`DOWNLOAD_DELAY`|
|开启cookie，每次请求带上前一次cookie，默认开启|`COOKIES_ENABLED`|
|开启cookie显示|`COOKIES_DEBUG`|
|设置默认请求头|`DEFAULT_REQUEST_HEADERS`|
|爬虫中间件，设置过程和管道相同|`SPIDER_MIDDLEWARES`|
|下载中间件|`DOWNLOADER_MIDDLEWARES`|


# 选择器
- response.selector类

|选择器|说明|示例|
|-|-|-|
|xpath|response.xpath方法的返回结果是一个类似list的类型，其中包含的是selector对象，操作和列表一样，但是有一些额外的方法|`response.selector.xpath("//title/text()")`|
|css||`response.selector.css("title::text")`|
|简写模式||`response.xpath("//title/text()").extract_first()`<br>`response.css("title::text").extract_first()`|
|迭代查找||`response.xpath("//title/text()").css("title::text")`|
|获取属性||`response.xpath("//title").css("img::attr(src)")`|
|防止报错，设置默认提取||`response.xpath("//title/text()").extract_first(default='')`|
|获取内容|extract() 返回一个包含有字符串的列表<br>extract_first() 返回列表中的第一个字符串，列表为空没有返回None|``|
|正则表达式匹配||`response.re("name:'(.*?)'")`<br>`response.re_first("name:'(.*?)'")`|

# 执行器
- scrapy.spider.Spider基类

|执行器|说明|示例|
|-|-|-|
|name|	spider名字，是任务的唯一标识|
|start_urls|	URL列表，起始。要实现不停的爬取，需要重写start_url，设置while、True，然后将don’t_filter设置为True|
|allowed_domains|	需要开启offsiteMiddleware，允许爬取的域名列表。需要抓取的、url地址必须属于allowed_domains,但是start_urls中的url地址没有这个限制
|crawler	|提供对spider核心组件的访问|
|setting	|控制核心插件的配置|
|from_crawler	|可以通过它获取全局配置|
|custom_settings|覆盖settings全局的配置|
|start_requests|	可以改写请求方式`Request(url="http://www.example.com",|cookies={'currency': 'USD', 'country': 'UY'})`|
|parse 	|回调函数，spider中的parse方法必须有返回值，需要通过yield返回可以传递给管道，只能返回request、baseitem、dict和none值<br>callback：默认是parse处理的<br>headers：<br>meta：实现数据的传递，需要注意，传递字典和列表时，要用eepcopy操作，否则在后续调用时引用相同，会造成数据错乱。<br>error_back：设置错误处理的函数，决定执行的逻辑。<br>filter：请求过的url将被过滤，默认为过滤|
|log|	message<br>level|
|closed	||
|自定义参数	||
|POST请求formRequest	||


- response对象

|属性|说明|示例|
|-|-|-|
|response.body|	响应体，也就是html代码，默认是byte类型|
|response.body_as_unicode|	响应体|
|response.copy|	|
|response.css|	|
|response.encoding|	|
|response.flags|	|
|response.follow|	补全地址|
|response.headers|	响应头|
|response.meta|	|
|response.replace|	|
|response.requests.headers|	当前响应的请求头|
|response.request.url|	当前响应对应的请求的url地址|
|response.selector|	|
|response.status|	|
|response.text|	|
|response.url|	当前响应的url地址|
|response.urljoin|	|
|response.xpath|	|


- spider对象

|属性|说明|示例|
|-|-|-|
|spider.close|||
|spider.crawler|||
|spider.custom_settings|||
|spider.from_crawler|||
|spider.handles_request|||
|spider.log|
|spider.logger|
|spider.make_requests_from_url|
|spider.name|
|spider.parse|
|spider.set_crawler|
|spider.settings|
|spider.start_requests|
|spider.start_urls|
|spider.update_settings|



- request对象
  - `yield scrapy.Request(url,callback,meta)`

|属性|说明|示例|
|-|-|-|
|request.body|||
|request.callback|||
|request.cookies|设定cookie信息|`scrapy.Request(cookies=cookie)`|
|request.copy|||
|request.dont_filter|||
|request.encoding|||
|request.errback|||
|request.flags|||
|request.headers|||
|request.meta|||
|request.method|||
|request.priority|||
|request.replace|||
|request.url|||

- FormRequest对象
- 用于post请求`scrapy.FormRequest(url=url,formdata=data,callback=self.parse)`

|属性|说明|示例|
|-|-|-|
|formdata|post数据||


# item和管道
## item
- item类型对象
  - 用于定义要存储的数据结构
```python
class XXXItem(scrapy.Item):
    author = scrapy.Field()
    content = scrapy.Field()
```

  - 在spider获取的数据建立XXXItem对象，并对author和content赋值，yield即可传递给管道。
```python
item = XXXItem()
item['author'] = "1"
item['content'] = "1"
yield item
```

## pipeline
- pipeline对象
  - pipeline中process_item的方法必须有，否则item没有办法接受和处理；
  - 有多个pipeline的时候，process_item的方法必须return item,否则后一个pipeline取到的数据为None值
  - process_item方法接受item和spider，其中spider表示当前传递item过来的spider
  - 使用之前需要在settings中开启，优先级：0~1000
  - pipeline在settings中能够开启多个，实现不同的pipeline可以处理不同爬虫的数据，不同的pipeline能够进行不同的数据处理的操作，比如一个进行数据清洗，一个进行数据的保存

|pipeline||
|-|-|
|process_item|	返回两种：1、item；2、dropitem|
|open_spider|	spider开启调用一次|
|close_spider|	spider关闭调用一次|
|from_crawler|	获取项目配置信息|

```python
class XXXPipline:
    fp = None
    def open_spider(self,spider):
        self.fp = open("./xxx.txt",'w')
    def process_item(self,item,spider):
        author = item["author"]
        content = item["content"]
        self.fp.write("{}:{}\n".format(author,content))
        return item
    def close_spider(self,spider):
        self.fp.close()
```

## ImagePipeline
- 在settings.py中设置文件保存的目录
```python
IMAGES_STORE = 'datas'
```

- 设置图片pipeline，还有保存的名字
```python
from scrapy.pipeline import ImagesPipline
class XXXImagePipline(ImagesPipeline):
    def get_media_requests(self,item,info):
        return [Request(x,meta={'item':item}) for x in item['image_url']]
    def file_path(self,request,response=None,info=None):
        item = request.meta["item"]
        path = item['title']item['image_urls'].index(request.url)+'.jpg'
        return path
    def item_completed(self,results,item,info):
      return item
```

|ImagesPipeline||
|-|-|
|get_media_requests|根据图片地址，进行图片数据请求|
|file_path|指定图片存储的路径|
|item_completed|返回item，给下一个pipeline|


# 中间件
## 爬虫中间件
- spiderMiddleware

## 下载中间件
- 位于引擎和下载器之间，下载中间件可以拦截所有请求，以及所有请求返回相应对象。
- 功能：UA修改，Proxy修改；篡改相应


|ImagesPipeline||
|-|-|
|from_crawler||
|process_request|拦截请求<br>输入request,spider<br>需要返回数据<br>None：返回none不会对框架产生影响，继续向后执行中间件，<br>Response：返回Response，不在调用其他中间件的request方法，而调用process_response方法<br>request：返回request，则会把request重新放入项目队列中去，循环往复的调度<br>raise|
|process_response|拦截相应<br>输入：request、response、spider<br>返回<br>Response：返回Response，不会对框架产生影响，继续向后执行中间件<br>request：返回request，则会把request重新放入项目队列中去，循环往复的调度<br>raise|
|process_exception|拦截异常<br>输入：request、response、spider<br>返回：<br>None：返回None不会对框架产生影响，继续向后执行中间件<br>Response：返回Response，不在调用其他中间件的request方法，而调用process_response方法，成功的放回<br>request：返回request，则会把request重新放入项目队列中去，循环往复的调度，相当于重新发送请求<br>raise|


# scrapy分布式
- 项目路径：[gitHub.com/rolando/srcapy-redis](https://gitHub.com/rolando/srcapy-redis)

- scrapy-redis是scrapy的组件，通过redis与调度器连接，存放调度器传递过来的指纹，存放request对象，默认存放redispipe保存的数据。
- 增量式爬虫，基于request对象的增量、基于数据的增量
- 实现request对象的指纹去重，使用sha1，加密请求方法、url地址、请求体得到16进制字符串。指纹存在redisset中，判断redis.sadd()存在则返回0

## 安装
```bash
pip3 install scrapy_redis
```

## 实现增量式爬虫

|增量爬虫设置||
|-|-|
|更改调度器	|`SCHEDULER = "scrapy_redis.scheduler.Scheduler"`|
|去重	|`DUPEFILTER_CLASS="scrapy_redis.dupefilter.RFPDupeFilter"`|
|调度器持久化存储|`SCHEDULER_PERSIST = True`不清空redis指纹，建议为false|
|redis连接	|`REDIS_URL="redis://root:user@ip:port"`|
|清空之前的所有指纹，重新爬取	|`SCHEDULER_FLUSH_ON_START=True`|

- settings代码如下
```python
DUPEFILTER_CLASS = "scrapy_redis.dupefilter.RFPDupeFilter"
SCHEDULER = "scrapy_redis.scheduler.Scheduler"
SCHEDULER_PERSIST = True
ITEM_PIPELINES = {
    'example.pipelines.ExamplePipeline': 300,
    'scrapy_redis.pipelines.RedisPipeline': 400,
}
REDIS_URL = "redis://127.0.0.1:6379"
 #或者使用下面的方式
 # REDIS_HOST = "127.0.0.1"
 # REDIS_PORT = 6379
```

- scrapy-redis将下面三个内容在redis中储存：
  1. Scheduler队列：存放待请求的request对象，过程是pop操作，获取一个去除一个。
  2. 指纹集合：存放已经进入Scheduler队列的request对象的指纹，指纹默认由请求方法、url和请求体组成
  3. item信息：只有开启RedisPipeline才会存入。


## 分布式爬虫RedisSpider
- 配置跟增量相同
- 实现代码
```python
from scrapy_redis.spiders import RedisSpider
class MySpider(RedisSpider):
    """Spider that reads urls from redis queue (myspider:start_urls)."""
    name = 'myspider_redis'
redis_key = 'myspider:start_urls'
# 手动指定allow_domains=[‘baidu.com’]
    def __init__(self, *args, **kwargs):
        # Dynamically define the allowed domains list.
        domain = kwargs.pop('domain', '')
        self.allowed_domains = filter(None, domain.split(','))
        super(MySpider, self).__init__(*args, **kwargs)
    def parse(self, response):
        return {
            'name': response.css('title::text').extract_first(),
            'url': response.url,
        }
```

- 增加了一个redis_key的键，没有start_urls，因为分布式中，如果每台电脑都请求一次start_url就会重复
- 多了__init__方法，该方法不是必须的，可以手动指定allow_domains


## 分布式爬虫RedisCrawlSpider
```python
from scrapy.spiders import Rule
from scrapy.linkextractors import LinkExtractor
from scrapy_redis.spiders import RedisCrawlSpider
class MyCrawler(RedisCrawlSpider):
    """Spider that reads urls from redis queue (myspider:start_urls)."""
    name = 'mycrawler_redis'
    redis_key = 'mycrawler:start_urls'
    rules = (
        # follow all links
        Rule(LinkExtractor(), callback='parse_page', follow=True),
    )
    def __init__(self, *args, **kwargs):
        # Dynamically define the allowed domains list.
        domain = kwargs.pop('domain', '')
        self.allowed_domains = filter(None, domain.split(','))
        super(MyCrawler, self).__init__(*args, **kwargs)
    def parse_page(self, response):
        return {
            'name': response.css('title::text').extract_first(),
            'url': response.url,
        }
```

## scrapy分布式部署
- 项目路径：[gitHub.com/scrapy/srcapyd](https://GitHub.com/scrapy/srcapyd)

### 安装	
```bash
pip3 install scrapyd==1.2.0a1
pip3 install scrapyd_client
```

### 配置和使用

|分布式部署||
|-|-|
|配置远程主机|`---scrapy.cfg----`<br>`[deploy]`<br>`url = http://47.95.229.68:6800/addversion.json`<br>`project = taobao`
|执行部署|	scrapyd-deploy|
|远程调度API|访问`http://scrapyd.readthedocs.io/en/lasted/api.html`|
|查看所有上传项目状态	|`curl http://47.95.229.68:6800/listprojects.json`|
|项目版本	|`curl http://47.95.229.68:6800/listversions.json?project=taobao`|
|远程任务启动|	`curl http://47.95.229.68:6800/schedule.json -d project=taobao -d spider=goods`请求多次可以开启多个进程|
|查看任务状态|	`curl http://47.95.229.68:6800/listjobs.json?project=taobao`  #可以查看job代号|
|任务取消	|`curl http://47.95.229.68:6800/cancel.json -d project=taobao -d job=job代号`|

### api封装
- 对scrapyd的封装，在python中就可以调度项目
- 项目地址：[GitHub.com/djm/python-srcapyd-api](https://GitHub.com/djm/python-srcapyd-api)

- 安装：`pip3 install python-srcapyd-api`


# crawlspider
- crawlspider是scrapy的另一种模板，可以自动从response中提取所有的满足规则的url地址，自动的构造自己requests请求，发送给引擎。

## 使用

|使用||
|-|-|
|创建任务|`scrapy genspider –t crawl 任务名 www.xxx.cn`|

- 注意点
  1. crawlspider中不能再有以parse为名字的方法，这个方法被用来实现基础url提取功能
  2. 一个Rule对象接收很多参数，包含LinkExtractor、callback、和follow
  3. 不指定callback，如果follow为true，满足该rule的url还会继续请求
  4. 如果多个rule都满足某一个url，则会从rules中选择第一个进行操作

## spider

|spider||
|-|-|
|rules|是一个元组或者是列表，包含的是Rule对象
|Rule|表示规则，其中包含LinkExtractor,callback和follow
|LinkExtractor|连接提取器，可以通过正则或者是xpath来进行url地址的匹配
|callback |表示经过连接提取器提取出来的url地址响应的回调函数，可以没有，没有表示响应不会进行回调函数的处理（有需要数据提取时设置True）|
|follow|表示进过连接提取器提取的url地址对应的响应是否还会继续被rules中的规则进行提取，True表示会，Flase表示不会（有需要url提取时设置True）|

```python
class xxxxSpider(CrawlSpider):
    name = 'xxx'
    allowed_domains = ['xxxx.cn']
    start_urls = ['http://xxxx.cn/']

    rules = (
        Rule(LinkExtractor(allow=r'Items/'), callback='parse_item', follow=True),
      Rule(LinkExtractor(restrict_xpath='//div/li'), callback='parse_item', follow=True),
    )       # 注意：正则匹配的?是有特殊用途的，所以url中?要/?处理
    def parse_item(self, response):
        i = {}
        #使用xpath进行数据的提取或者url地址的提取
        return i
```

|LinkExtractor||
|-|-|
|allow(re)|满足正则表达式的url会被提取，如果为空，则全部匹配|
|deny(re)|满足正则表达式，则一定不提取，优先级高于allow|
|allow_domains(domain)|会被提取的domains|
|deny_domains(domain)|不会被提取的domains|
|restrict_xpaths|使用xpath表达式，和allow共同作用过滤链接|

|Rule||
|-|-|
|link_extractor|linkextract对象用于定义需要提取的方法|
|callback|获得连接后，指定回调函数|
|follow(bool)|指定改规则的response是否需要跟进|
|process_links|指定该spider中那个函数将会被调用，从link_extractor中获取到链接列表时将会调用该函数，该方法主要用来过滤url|
|process_request|指定该spider中哪个函数将会被调用，改规则提取到每个request时都会调用该函数，用于过滤request|


