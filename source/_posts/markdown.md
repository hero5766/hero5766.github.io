---
title: markdown语法
author: hero576
tags:
  - skills
categories:
  - common
date: 2020-06-04 22:34:00
---
> markdown用法简介
<!--more-->
## 标题

- 标题间没有横岗
```
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```

- 分级标题，= - 最少可以只写一个，兼容性一般

```
一级标题
======================
二级标题
---------------------
```

- TOC：根据标题生成目录，兼容性一般

```
[TOC]
```

## 字体
### 语义标记
|描述	|效果	|代码|
|--|--|--|
|斜体|	*斜体*|	`*斜体*`|
|斜体|	_斜体_|	`_斜体_`|
|加粗|	**加粗**|	`**加粗**`|
|加粗+斜体|	***加粗+斜体***|	`***加粗+斜体***`|
|加粗+斜体|	**_加粗+斜体_**|	`**_加粗+斜体_**`|
|删除线|	~~删除线~~|	`~~删除线~~`|

### 语义标签
|描述	|效果	|代码|
|--|--|--|
|斜体|	<i>斜体</i>|	`<i>斜体</i>`|
|加粗|	<b>加粗</b>|	`<b>加粗</b>`|
|强调|	<em>强调</em>|	`<em>强调</em>`|
|上标|	Z<sup>a</sup>|	`Z<sup>a</sup>`|
|下标|	Z<sub>a</sub>|	`Z<sub>a</sub>`|
|键盘文本|<kbd>Ctrl</kbd>|`<kbd>Ctrl</kbd>`|
|换行|  |  |


## 引用
- 在引用的文字前加>即可。引用也可以嵌套，如加两个>>三个>>>n个...
```
>这是引用的内容
>也是引用的内容
或者引用的内容
>>这是引用的内容
>>>>>>>>>>这是引用的内容
```

## 分割线
- 三个或者三个以上的 - 或者 * 都可以。
```
# ---
# ----
# ***
# *****
# 井号和空格是我防止格式变化的，实际中去掉就行
```

## 图片

- 普通图片
```
![图片alt](图片地址 ''图片title'')
# 图片alt就是显示在图片下面的文字，相当于对图片内容的解释。
# 图片title是图片的标题，当鼠标移到图片上时显示的内容。title可#加可不加
```

- 插入图片带有链接

```
[[图片上传失败...(image-f83b77-1542510791300)]](http://www.baidu.com){:target="_blank"}       // 内链式

[[图片上传失败...(image-4dc956-1542510791300)]][5]{:target="_blank"}                      // 引用式
[5]: http://www.baidu.com
```

## 超链接
```
[超链接名](超链接地址 "超链接title")
# title可加可不加
```

## 视频插入
- Markdown 语法是不支持直接插入视频的
- 普遍的做法是 插入HTML的 iframe 框架，通过网站自带的分享功能获取
```
<iframe height=498 width=510 src='http://player.youku.com/embed/XMjgzNzM0NTYxNg==' frameborder=0 'allowfullscreen'></iframe>
```
![iframe生成](/images/pasted-9.png)

- 第二是伪造播放界面，实质是插入视频图片，然后通过点击跳转到相关页面
```
[[图片上传失败...(image-49aefe-1542510791300)]](http://v.youku.com/v_show/id_XMjgzNzM0NTYxNg==.html?spm=a2htv.20009910.contentHolderUnit2.A&from=y1.3-tv-grid-1007-9910.86804.1-2#paction){:target="_blank"}
```


## 列表
### 无序列表
- 无序列表用 - + * 任何一种都可以

```
- 列表内容
+ 列表内容
* 列表内容
# 注意：- + * 跟内容之间都要有一个空格
```

### 有序列表
- 数字加点

```
1. 列表内容
2. 列表内容
3. 列表内容
# 注意：序号跟内容之间要有空格
```

### 列表嵌套
- 上一级和下一级之间敲三个空格即可

```
- 1   
  - 1.1
  
```

## 表格
### 普通表格
- `:` 代表对齐方式 , `:` 与 `|` 之间不要有空格，否则对齐会有些不兼容

```
表头|表头|表头
---|:--:|---:
内容|内容|内容
内容|内容|内容
# 第二行分割表头和内容。
# - 有一个就行，为了对齐，多加了几个
# 文字默认居左
# -两边加：表示文字居中
# -右边加：表示文字居右
# 注：原生的语法两边都要用 | 包起来。此处省略
a  | b | c  
:-:|:- |-:
    居中    |     左对齐      |   右对齐    
============|=================|=============
```

### 特殊表格
- 一般对合并单元格，以及其他特殊格式表格，markdown 是无能为力的，不过可以在线生成HTML代码 [Tables Generator](http://www.tablesgenerator.com) (国外的站)，使用HTML标签。

## 代码

- 单行代码：代码之间分别用一个反引号包起来
```
`代码内容`
```

- 代码块：代码之间分别用三个反引号包起来，且两边的反引号单独占一行
```key
(```)
  代码...
  代码...
  代码...
(```)
```


language 	|key|language 	|key|language 	|key|language 	|key|
-|-|-|-|-|-|-|-|
1C| 	1c|Mathematica| 	mathematica|Erlang REPL| 	erlang-replOracle| Rules| Language 	ruleslanguage
ActionScript| 	actionscript|Matlab| 	matlab|Erlang| 	erlang|Rust| 	rust|
Apache| 	apache|MEL (Maya Embedded Language) 	|mel|FIX| 	fix|Scala| 	scala|
AppleScript| 	applescript|Mercury| 	mercury|F#| 	fsharp|Scheme| 	scheme|
AsciiDoc| 	asciidoc|Mizar| 	mizar|G-code(ISO 6983) 	|gcode|Scilab| 	scilab|
AspectJ| 	asciidoc|Monkey| 	monkey|Gherkin| 	gherkin|SCSS| 	scss|
AutoHotkey| 	autohotkey|Nginx| 	nginx|GLSL| 	glsl|Smali| 	smali|
AVR| Assembler| 	avrasmNimrod| 	nimrod|Go| 	go|SmallTalk| 	smalltalk|
Axapta| 	axapta|Nix| 	nix|Gradle| 	gradle|SML| 	sml|
Bash| 	bash|NSIS| 	nsis|Groovy| 	groovy|SQL| 	sql|
BrainFuck| 	brainfuck|Objective C| 	objectivec|Haml| 	haml|Stata| 	stata|
Cap’n| Proto| 	capnprotoOCaml| 	ocaml|Handlebars| 	handlebars|STEP Part21(ISO 10303-21) 	|step21|
Clojure| REPL| 	clojureOxygene| 	oxygene|Haskell| 	haskell|Stylus| 	stylus|
Clojure| 	clojure|Parser 3| 	parser3|Haxe| 	haxe|Swift| 	swift|
CMake| 	cmake|Perl| 	perl|HTML| 	html|Tcl| 	tcl|
CoffeeScript| 	coffeescript|PHP| 	php|HTTP| 	http|Tex| 	tex|
C++| 	cppPowerShell| 	powershell|Ini file| 	ini|text| 	text/plain
C#| 	cs|Processing| 	processing|Java| 	java|Thrift| 	thrift|
CSS| 	css|Python’s profiler output 	|profile|JavaScript| 	javascript|Twig| 	twig|
D| 	d|Protocol Buffers| 	protobuf|JSON| 	json|TypeScript| 	typescript|
Dart| 	d|Puppet| 	puppet|Lasso| 	lasso|Vala| 	vala|
Delphi| 	delphi|Python| 	python|Less| 	less|VB.NET| 	vbnet|
Diff| 	diff|Q| 	q|Lisp| 	lisp|VBScript| in| HTML 	vbscript-html
Django| 	django|R| 	r|LiveCode| 	livecodeserver|VBScript| 	vbscript|
DOS.bat 	dos|RenderMan RIB| 	rib|LiveScript| 	livescript|Verilog| 	verilog|
Dust| 	dust|Roboconf| 	roboconf|Lua| 	lua|VHDL| 	vhdl|
Elixir| 	elixir|RenderMan| RSL| 	rslMakefile| 	makefile|Vim Script| 	vim|
ERB(Embedded Ruby) 	erbRuby| 	ruby|Markdown| 	markdown|Intel x86 Assembly 	|x86asm|
XL| 	xl|XML| 	xml|YAML| 	yml|

## 任务列表
- 兼容性一般 要隔开一行

```
这是文字……

- [x] 选项一
- [ ] 选项二  
- [ ]  [选项3]
```

## 表情
- [表情代码地址](https://www.webpagefx.com/tools/emoji-cheat-sheet/'GitHub')
![表情](/images/pasted-10.png)

## 样式
-  支持内嵌CSS样式
```
<p style="color: #AD5D0F;font-size: 30px; font-family: '宋体';">内联样式</p>
```

### 更改字体、大小、颜色
<font face="黑体">我是黑体字</font>
<font face="微软雅黑">我是微软雅黑</font>
<font face="STCAIYUN">我是华文彩云</font>
<font color=red>我是红色</font>
<font color=#008000>我是绿色</font>
<font color=Blue>我是蓝色</font>
<font size=5>我是尺寸</font>
<font face="黑体" color=green size=5>我是黑体，绿色，尺寸为5</font>
```
<font face="黑体">我是黑体字</font>
<font face="微软雅黑">我是微软雅黑</font>
<font face="STCAIYUN">我是华文彩云</font>
<font color=red>我是红色</font>
<font color=#008000>我是绿色</font>
<font color=Blue>我是蓝色</font>
<font size=5>我是尺寸</font>
<font face="黑体" color=green size=5>我是黑体，绿色，尺寸为5</font>
```


## 流程图
- 流程图分为两个部分： `定义参数` 然后 `连接参数`

   - 定义格式：`tag=>type: content:>url`

|格式名称|说明|
|--|--|
|tag|	标签 (可以自定义)|
|=>|		赋值|
|type|	类型 (6种类型)：start启动;end结束;operation程序;subroutine子程序;condition条件;inputoutput输出 |
|content|	描述内容 (可以自定义)|
|:>url|	链接与跳转方式 兼容性很差|

```
st=>start: 开始:>http://www.baidu.com[blank]  //示例
# `st=>start:` 开始 的`：`后面保持空格
```

  - 连接格式：`st->c1(yes,right)->c2(yes,right)->c3(yes,right)->io->e`
  - operation (程序); subroutine (子程序) ;condition (条件)，都可以在括号里加入连接方向。

|格式名称|说明|
|--|--| 
|->	|	连接|
|condition	|	条件|
|(布尔值,方向)|		如果满足向右连接，4种方向：right ，left，up ，down 默认为：down|
  
```
开始->判断条件1为no->判断条件2为no->判断条件3为no->输出->结束
```

```
(```flow)
st=>start: 开始
op=>operation: My Operation
cond=>condition: Yes or No?
e=>end
st->op->cond
cond(yes)->e
cond(no)->op
(```)
```

```
(```flow  )                   // 流程
st=>start: 开始|past:> http://www.baidu.com // 开始
e=>end: 结束              // 结束
c1=>condition: 条件1:>http://www.baidu.com[_parent]   // 判断条件
c2=>condition: 条件2      // 判断条件
c3=>condition: 条件3      // 判断条件
io=>inputoutput: 输出     // 输出
//----------------以上为定义参数-------------------------
//----------------以下为连接参数-------------------------
// 开始->判断条件1为no->判断条件2为no->判断条件3为no->输出->结束
st->c1(yes,right)->c2(yes,right)->c3(yes,right)->io->e
c1(no)->e                   // 条件1不满足->结束
c2(no)->e                   // 条件2不满足->结束
c3(no)->e                   // 条件3不满足->结束
(```)
```
![流程图](/images/pasted-13.png)


## 脚注

```
Markdown[^1]
[^1]: Markdown是一种纯文本标记语言        // 在文章最后面显示脚注
```

## 锚点
- 只有标题支持锚点， 跳转目录方括号后 保持空格
```
[公式标题锚点](#1)
### [需要跳转的目录] {#1}    // 方括号后保持空格
```

## 定义型列表
- 解释型定义
```
Markdown 
:   轻量级文本标记语言，可以转换成html，pdf等格式  //  开头一个`:` + `Tab` 或 四个空格
代码块定义
:   代码块定义……
        var a = 10;         // 保持空一行与 递进缩进
```


## 自动邮箱链接
```
<xxx@outlook.com>
```

## 时序图

```
(```sequence)
A->>B: 你好
Note left of A: 我在左边     // 注释方向，只有左右，没有上下
Note right of B: 我在右边
B-->A: 很高兴认识你
(```)
```
![时序图1](/images/pasted-12.png)

|符号|	含义|
|--|--|
|-|	实线|
|>|	实心箭头|
|--|	虚线|
|>>|	空心箭头|

```
    (```sequence)
    起床->吃饭: 稀饭油条
    吃饭->上班: 不要迟到了
    上班->午餐: 吃撑了
    上班->下班:
    Note right of 下班: 下班了
    下班->回家:
    Note right of 回家: 到家了
    回家-->>起床:
    Note left of 起床: 新的一天
    (```)
```
![时序图2](/images/pasted-11.png)