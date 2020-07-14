title: yaml文件
author: hero576
tags:
  - python
categories:
  - programme
date: 2020-07-13 21:07:00
---
> yaml文件格式
<!--more-->

# yaml简介
- yaml是一种用来写配置文件的序列化语言，跟json有些像，yaml又称作json的超集，yaml的设计则是为了友好可读性，主要用于配置信息的书写，而json设计的目的则是为了简单和通用，主要用于存储数据和应用层数据通信使用。

# 语法
## 数据类型
- yaml主要有三种类型的数据原语，基于这三种数据原语可以组合出任何数据结构。

|名称|说明|官方表达|
|--|--|--|
|Maps|对象，键值对|[mappings](https://yaml.org/spec/1.2/spec.html#mapping//) (hashes/dictionaries)|
|Lists|数组|[sequences](https://yaml.org/spec/1.2/spec.html#sequence//) (arrays/lists)|
|Scales|纯量，不可分割的值|[scalars](https://yaml.org/spec/1.2/spec.html#scalar//) (strings/numbers)|

### 对象
```yaml
animal: pets
>>> { animal: 'pets' }
hash: { name: Steve, foo: bar } 
>>> { hash: { name: 'Steve', foo: 'bar' } }
```

### 数组
```yaml
- Cat
- Dog
- Goldfish
>>> [ 'Cat', 'Dog', 'Goldfish' ]
-
 - Cat
 - Dog
 - Goldfish
>>> [ [ 'Cat', 'Dog', 'Goldfish' ] ]
animal: [Cat, Dog]
>>> { animal: [ 'Cat', 'Dog' ] }
```
### 纯量
- 数值类型：字符串、整数、浮点数
```yaml
number: 12.30
>>> { number: 12.30 }
```

- 布尔值
```yaml
isSet: true
>>> { isSet: true }
```

- Null
```yaml
parent: ~ 
>>> { parent: null }
```

- 时间
```yaml
# 采用 ISO8601 格式
iso8601: 2001-12-14t21:59:43.10-05:00 
>>> { iso8601: new Date('2001-12-14t21:59:43.10-05:00') }
```

- 日期
```yaml
# 采用复合 iso8601 格式的年、月、日表示
date: 1976-07-31
>>> { date: new Date('1976-07-31') }
```

### 字符串详解
```yaml
# 字符串默认不使用引号表示。
str: 这是一行字符串
>>> { str: '这是一行字符串' }
# 如果字符串之中包含空格或特殊字符，需要放在引号之中。
str: '内容： 字符串' 
>>> { str: '内容: 字符串' }
#单引号和双引号都可以使用，双引号不会对特殊字符转义。
s1: '内容\n字符串'
s2: "内容\n字符串"
>>> { s1: '内容\\n字符串', s2: '内容\n字符串' }
# 单引号之中如果还有单引号，必须连续使用两个单引号转义。
str: 'labor''s day' 
>>> { str: 'labor\'s day' }
# 字符串可以写成多行，从第二行开始，必须有一个单空格缩进。换行符会被转为空格。
str: 这是一段
  多行
  字符串
>>> { str: '这是一段 多行 字符串' }
# 多行字符串可以使用|保留换行符，也可以使用>折叠换行。
this: |
  Foo
  Bar
that: >
  Foo
  Bar
>>> { this: 'Foo\nBar\n', that: 'Foo Bar\n' }
# +表示保留文字块末尾的换行，-表示删除字符串末尾的换行。
s1: |
  Foo
s2: |+
  Foo
s3: |-
  Foo
>>> { s1: 'Foo', s2: 'Foo\n', s3: 'Foo' }
# 字符串之中可以插入 HTML 标记。
message: |
  <p style="color: red">
    段落
  </p>
>>> { message: '<p style="color: red">\n  段落\n</p>\n' }
```

### 数据类型转换
- YAML 允许使用两个感叹号，强制转换数据类型。
```yaml
e: !!str 123
f: !!str true
>>> { e: '123', f: 'true' }
```

## 语法格式
- yaml文件大小写敏感
- 不允许使用用tab制表符号代替空格
- 使用缩进表示层级关系
- 缩进的空格数目不重要，只要相同层级的元素左侧对齐即可
- 字符串默认不使用引号表示
- 单引号之中如果还有单引号，必须连续使用两个单引号转义`'labor''s day'`

### 符号系统

|符号|符号|说明|
|--|--|--|
|`-`|破折号和空格|Lists集合|
|`:`|冒号和空格|Maps键值对|
|`#`|井号|注释，从这个字符一直到行尾，都会被解析器忽略|
|`---`|三个破折号|文档内容分隔线（多用于文档开始的地方）|
|`...`|三个冒号|表示文档的结束|
|`&`<br>`*`<br>`<<`||在`-`或`:`后加上`&`用来建立锚点（defaults）<br>使用`<<: *锚点名`直接将锚点数据插入到当前的数据中<br>`*锚点名`用来引用锚点。两者成对表达，像定义变量a，再引用变量a的关系，是一种重复项的替换。|
|`~`|波浪|null|
|`!!`|双感叹号|表示强制转换数据类型`e: !!str 123`|
|`|`|| 保留多行文本（保留换行符）|
|>|| 将多行拼接为一行|

### 引用
- 锚点`&`：建立锚点（defaults），别名`*`：用来引用锚点，`<<`表示合并到当前数据。

```yaml
defaults: &defaults
  adapter:  postgres
  host:     localhost
development:
  database: myapp_development
  <<: *defaults
# 等价于
defaults:
  adapter:  postgres
  host:     localhost
development:
  database: myapp_development
  adapter:  postgres
  host:     localhost
# 锚点
hr:
  - Mark McGwire
  # Following node labeled SS
  - &SS Sammy Sosa
rbi:
  - *SS # Subsequent occurrence
  - Ken Griffey
```

# python解析yaml
- 更多操作可参考[pyyaml官方文档](https://pyyaml.org/wiki/PyYAMLDocumentation)

|操作|说明|
|--|--|
|安装|`pip3 install pyyaml`|
|load|`yaml.load()`<br>`yaml.safe_load(YAML字符串或文件句柄)`|
|dump|`yaml.dump`(字典)：默认为flow流格式，即字典{b': {'c': 3, 'd': 4}}，会被转为b: {c: 3, d: 4}形式，可以使用default_flow_style=False关闭流模式|

- 由于yaml.load()支持原生Python对象，不安全，建议使用yaml.safe_load()

## 示例

```python
import yaml
yaml_str = '''
name: Cactus
age: 18
skills: 
  -
    - Python
    - 3
  -
    - Java
    - 5
has_blog: true
gf: ~
'''
dict_var = {'name': 'Cactus', 'age': 18, 'skills': [['Python', 3], ['Java', 5]], 'has_blog': True, 'gf': None}
# yaml字符串 -> 字典
print(yaml.safe_load(yaml_str)) 
# yaml文件 -> 字典
with open('demo.yaml', encoding='utf-8') as f:   # demo.yaml内容同上例yaml字符串 
    print(yaml.safe_load(f))
# 字典 -> yaml字符串或文件
print(yaml.dump(dict_var,))  # 转为字符串，使用默认flow流格式
with open('demo5.yaml', 'w', encoding='utf-8') as f:
    yaml.dump(dict_var, f, default_flow_style=False)  # 写入文件，不是用flow流格式
```