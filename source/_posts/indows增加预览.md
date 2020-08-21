---
title: windows增加预览
author: hero576
tags:
  - windows
categories:
  - common
date: 2020-06-22 22:21:00
---
> 给windows增加预览功能
<!--more-->

## 什么是文件预览

- 在`windows`中已经创建关联的情况下，双击文件可以调用默认的应用程序进行浏览和编辑。从`Windows Vista`开始，操作系统提供了非常实用的预览功能，激活“预览窗格”之后，可以在资源管理器中直接预览文档内容。到了`Windows 8/8.1`、`Windows 10`，文件预览功能更为强大，不仅支持图片、音频、视频文件的预览，而且.可以直接预览`Office`档、文本、字体、网页乃至`PDF`文件和`E-mail`。

## 如何激活预览功能
- 激活的快捷键：`Alt + p`
- 手动操作：`打开“计算机”或“资源管理器”窗口→查看→窗格→预览窗格`
- 打开后，单机文件就可以查看相应内容。
- 如果这个设置没有应用到所有文件夹，那么请在打开预览窗格的前提下：`查看→选项→文件夹选项→查看→应用到文件夹`

## 手动添加预览文件类型
- 有时`windows`默认注册的预览器，并不能满足我们的要求，我们可以通过修改注册表的方式，手动关联预览器。
- 有很多文件在windows中不支持预览，非常不方便，我们可以在注册表手工增加。需要将文件后缀注册到windows中。下面我们以`.opml`文件为例，关联一个预览器。

### 查看已注册的预览器
- 实现预览功能，必须首先注册的预览器，然后绑定文件后缀，才能实现。我们可以通过注册表查看已经注册的预览器。
- 运行窗口输入：`regedit`，打开注册表，调转至：`HKEY_LOCAL_MACHINE＼SOFTWARE＼Microsoft＼Windows＼CurrentVersion＼PreviewHandlers`，右侧窗格列出了操作系统已注册的所有预览器。
- 我们可以从`数据`列查看预览器的名称，对应的值就是预览器的操作符。

|名称|值|说明|
|--|--|--|
|Windows Media Player Rich Preview Handler|{031EE060-67BC-460d-8847-E4A7C5E45A27}|预览音频、视频格式的媒体文件，这也是在文件资源管理器可以直接实现播放的原因所在|
|Windows TXT Previewer|{1531d583-8375-4d3f-b5fb-d23bbd169f22}|实现对于.txt、.reg、.bat等文本文件的预览|
|Windows Font previewer|{8a7cae0e-5951-49cb-bf20-ab3fa1e44b01}|预览字体文件|
|Windows RTF Previewer|{a42c2ccb-67d3-46fa-abe6-7d2f3488c7a3}|用于预览RTF文档|
|Windows Excel预览器|{00020827-0000-0000-C000-000000000046}|用于实现Office文档的预览功能|
|Microsoft Windows Mail Html Preview Handler|{f8b8412b-dea3-4130-b36c-5e8be73106ac}|预览html文件|
|Microsoft Visio previewer|{21E17C2F-AD3A-4b89-841F-09CFE02D16B7}|预览visio文件|
|Microsoft PowerPoint previewer|{65235197-874B-4A07-BDC5-E65EA825B718}|预览ppt|
|Microsoft Word previewer|{84F66100-FF7C-4fb4-B0C0-02CD7FB668FE}|预览word|
|Microsoft Excel previewer|{00020827-0000-0000-C000-000000000046}|Excel|
|Foxit PDF Preview Provider (XP)|{1B96FAD8-1C10-416E-8027-6EFF94045F6F}|FoxitReaderPlus.Document{E357FCCD-A995-4576-B01F-234630154E96}-{21F5E992-636E-48DC-9C47-5B05DEF82372}|
|Foxit PDF Preview Handler|{FFBD7029-84D7-4E1E-BE44-B6619BC545ED}|PDF|

- 这些都是`Windows 10`自带的预览器。除此之外，这里也会列出很多第三方程序提供的预览器。像PDF文档的预览器，在安装`Adobe`时，程序会自动注册预览器，将`pdf`文件与之关联。
- `.opml`文件我们选择`Windows TXT Previewer`作为默认预览器，将`{1531d583-8375-4d3f-b5fb-d23bbd169f22}`数值名称复制到剪贴板备用。

### 注册文件格式
- 打开注册表，右键`HKEY_CLASSES_ROOT＼`；
- 从快捷菜单中选择`新建→项`，手工新建一个名为`.opml`的项，双击右侧窗格的`默认`二进制值，将其数值名称设置为`opmlfile`；
- 仍然右击`HKEY_CLASSES_ROOT`，手工新建一个名为`opmlfile`的项，创建一个名为`shellex`的子项，如果`HKEY_CLASSES_ROOT＼`下已经有相应的项目，那么这一步骤可以省略；
- 右击`shellex`，创建一个名为`{8895b1c6-b41f-4c1c-a562-0d564250836f}`的子项，这个子项的作用是向文件资源管理器报告该类型的文件可以被预览。

### 分配预览器
- 单击选中`{8895b1c6-b41f-4c1c-a562-0d564250836f}`项，此时右侧窗格中会有一个`默认`的值，双击打开`编辑字符串`对话框，将其数值数据设置为`{1531d583-8375-4d3f-b5fb-d23bbd169f22}`，也就是说为`.opml`文件分配`Windows TXT Previewer`预览器，在注册表编辑器中执行刷新操作，或者直接关闭窗口。


> 完成上述操作之后，现在我们就可以在文件资源管理器直接预览.opml格式的文件，此时看到的就不再只是冷冰冰的`没有预览`的提示信息了。按照类似的步骤，我们在正确选择预览器、注册文件格式之后，记住`“有子项直接编辑，无子项手工创建”`这句话即可，很快就可以让`Windows 10`正常预览相应格式的文件。
> 最近发现不需要新建opmlfile项，直接在.opml下新建shellex即可，也同样能达到预览的效果。