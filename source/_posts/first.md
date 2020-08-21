title: hexo搭建博客记录
tags:
  - skills
categories:
  - common
date: 2020-05-30 18:56:00
---
> 之前的博客很多图都挂掉了，想重新改改，但是恢复好像难度比较大。所以索性重新建了一个博客，顺便把创建博客的过程记录一下吧。
<!--more-->

## 准备工作
### [github](https://github.com)操作
- 创建仓库  
  在`github`新建一个以你的用户名命名的仓库：`用户名.github.io`，这样将来访问地址就是：`http://用户名.githu.io`了。所以，每个github账户只能创建一个可以直接使用域名访问的仓库。
- 生成`ssh`添加到`github`
```
git config --global user.name "yourname"
git config --global user.email "youremail"
ssh-keygen -t rsa -C "youremail"
```

### 安装node.js和npm
  - windows：`nodejs`选择`LTS`版本就行了。
  - linux：
```
sudo apt-get install nodejs
sudo apt-get install npm
```
 
- 安装完后，打开命令行
```
node -v
npm -v
```

- 检查一下有没有安装成功。

### 安装git
  - windows：到`git`官网上下载,[Download git](https://gitforwindows.org/),下载后会有一个`Git Bash`的命令行工具，以后就用这个工具来使用git。
  - linux：对`linux`来说实在是太简单了，因为最早的`git`就是在`linux`上编写的，只需要一行代码
```
sudo apt-get install git
```

### 安装hexo  
- 前面`git`和`nodejs`安装好后，就可以安装`hexo`了，你可以先创建一个文件夹`blog`，然后cd到这个文件夹下（或者在这个文件夹下直接右键`git bash`打开）
```
npm install -g hexo-cli
```

- 依旧用`hexo -v`查看一下版本。
- 然后执行下面代码就可以搭建项目了
```
hexo init myblog
cd myblog
npm install
```

- 搭建完成之后，查看`npm` 安装各 `hexo` 插件的情况
```
npm ls --depth 0
```

![缺失包名的提示](/images/pasted-0.png)
- 解决该问题，需要逐一安装缺失的包
```
# 根据提示，注意安装缺失的依赖包
npm install hexo-generator-archive --save
# 更改主题后报错
npm install --save hexo-renderer-jade hexo-generator-feed hexo-generator-sitemap hexo-browsersync hexo-generator-archive
# 本地预览工具，使用`hexo server`或者`hexo s`即可预览项目
npm install hexo-server --save
# hexo部署插件
npm install hexo-deployer-git --save
# 搜索组件
npm install --save hexo-generator-search
# 卸载mathJax组件，安装KaTex
npm un hexo-renderer-marked --save
npm un hexo-renderer-kramed --save
npm un hexo-math --save
npm i @upupming/hexo-renderer-markdown-it-plus --save
# 音乐播放器
npm install --save hexo-tag-aplayer
```

- 安装完后重新构建建立的`hexo`项目即可解决。
```
# 清除缓存
hexo clean
# 生成静态文件
hexo g
```

### hexo-admin插件
- 安装插件
```
npm install --save hexo-admin
```

- 启动服务器。
```
hexo server -d
```

  - 即可在`localhost:4000/admin/`中编辑博文了。
  - 然后，`Deploy`之前，还需要编辑配置文件`_config.yml`。(否则会出现`Error: Config value "admin.deployCommand" not found`或者`Error: spawn hexo ENOENT`之类的报错。)
    - Windows则在末尾加上
![_config.xml的配置](/images/pasted-1.png)
- 然后在同级目录新建`hexo-pubish.bat`文件，文件内容如下：
```
hexo g -d
```

  - linux
    - create deploy-script in project dir:
```
$ touch hexo-deploy.sh; chmod a+x hexo-deploy.sh
```

    - with such 2 lines for example or any custom code:
```
#!/usr/bin/env sh
hexo deploy
```

    - and edit _config.yml:
```
admin:
  deployCommand: './hexo-deploy.sh'
```

  - 编辑完毕后，就可以点击`Deploy`，直接部署发布`Github`博客上。
![点击deploy部署](/images/pasted-2.png)
- PS：关于`Hexo Admin`插入图片
  - `Hexo Admin`可以直接复制图片粘贴，然后自动下载到`source/images`目录并重命名。但在`Windows`中粘贴后会出现裂图。这时就需要手动把括号中的前后两个斜杠去掉，就能正常显示。
- 密码保护
  - 打开`setting`，点击`Setup authentification here`输入用户名，密码，密钥，下面会自动生成配置文件，复制加在`hexo`根目录下的`_config.yml`中：
```
admin:
  username: myfavoritename
  password_hash: be121740bf988b2225a313fa1f107ca1
  secret: a secret something
```

  - 重启`hexo`，就可以看到登录页面了
- 发布文章
  - 进入后台之后点击`Deploy`，里面的`Deploy`按钮是用来执行发布脚本的，所以我们先在博客根目录下新建个目录`admin_script`，然后在目录中新建一个脚本`hexo-g.sh`，里面写下下面代码然后保存，
```
hexo g && hexo d
```

  - 然后给`hexo-g.sh`加入可执行权限
```
chmod +x hexo-d.sh
```

  - 然后在`_config.yml`中的`admin`下添加
```
admin:
  username: myfavoritename
  password_hash: be121740bf988b2225a313fa1f107ca1
  secret: a secret something
  deployCommand: ./admin_script/hexo-d.sh
```

  - 设置发布执行的脚本，点击`Deploy`就会执行这个命令并提交到`GitHub`上。

### 绑定域名
- 如果有了域名，可以将域名和上一步建立的仓库进行绑定
  - 首先在终端中`ping`一下仓库网址，获取`IP`，`ping 用户名.github.com`
  - 配置域名的`CNAME`的指向获取的`IP`
  - 登录`GitHub`，进入之前创建的仓库，点击`settings`，设置`Custom domain`，输入你的域名`fangzh.top`
  - 然后在你的博客文件`source`中创建一个名为`CNAME`文件，不要后缀。写上你的域名。  
- 过不了多久，再打开你的浏览器，输入你自己的域名，就可以看到搭建的网站啦！

## hexo的使用

### 创建项目
- `hexo`安装完成后，接下来初始化一下`hexo`
```
hexo init [项目名称]
cd [项目名称]
npm install
```

- 项目文件夹会生成以下的目录  

|目录名|作用|
|----|----|
|node_modules|依赖包|
|public|存放生成的页面|
|scaffolds|生成文章的一些模板|
|source|用来存放你的文章|
|themes|主题|
|_config.yml|博客的配置文件|

### 项目配值
- 在文件根目录下的`_config.yml`，就是整个`hexo`框架的配置文件了。可以在里面修改大部分的配置。详细可参考[官方的配置](https://hexo.io/zh-cn/docs/configuration)描述。

#### 网站配置
|参数|	描述|
|---|---|
|title|	网站标题|
|subtitle|	网站副标题|
|description|	网站描述|
|author|	您的名字|
|language|	网站使用的语言|
|timezone|	网站时区。Hexo 默认使用您电脑的时区。时区列表。比如说：America/New_York, Japan, 和 UTC 。|

- 其中，`description`主要用于`SEO`，告诉搜索引擎一个关于您站点的简单描述，通常建议在其中包含您网站的关键词。`author`参数用于主题显示文章的作者。

#### 网址配置
|参数|	描述|
|--|--|
|url|	网址|
|root|	网站根目录|
|permalink|	文章的链接格式|
|permalink_defaults|	永久链接中各部分的默认值|

- 关于permalink链接的变量可以去[官网上查找](https://hexo.io/zh-cn/docs/permalinks)

#### 主题配置（theme）

##### 更换主题
|参数|	描述|
|--|--|
|theme|	landscape|

- theme就是选择什么主题，也就是在`theme`这个文件夹下，在[hexo主题推荐官网](https://hexo.io/themes/)上有很多个主题，默认给你安装的是lanscape这个主题。其他的主题参数配置可以查询：[官网链接]((https://hexo.io/zh-cn/docs/themes.html))
- 当你需要更换主题时，可以在官网上下载
```
$ git clone https://github.com/theme-next/hexo-theme-next themes/next
$ git clone -b master/dev https://..  # 克隆指定分支的代码
$ cd themes/next
$ git tag -l  # 查看该主题的版本列表
$ git checkout v1.7.0   # 选择版本
```

- 主题文件夹中的`_config.xml`是主题的相关配置，具体可以根据主题帮助文档更改就可以了。

##### 主题推荐
- hexo主题不少，看个人喜好，我总结了几个网上推荐并且比较多人用的主题。
  - [NexT](https://github.com/theme-next/hexo-theme-next)：极简风，功能集成多。缺点是太过简单界面不漂亮。[配置帮助文档](https://github.com/theme-next/hexo-theme-next/tree/master/docs/zh-CN)
    - 设置CDN
    - 数学公式：MathJax、Katex
    - 评论：Disqus、DisqusJS、LiveRe、Gitalk、Valine (China)、Changyan (China)
    - 分析：Google Analytics、Baidu Analytics (China)、Growingio Analytics、CNZZ Analytics (China)
    - 统计：LeanCloud (China)、Firebase、Busuanzi Counting
    - 发布工具：Widgetpack Rating、AddThis
    - 搜索工具：Algolia Search、Local Search、Swiftype
    - 聊天工具：Chatra、Tidio
    - 其他工具：PJAX、Fancybox、MediumZoom、Lazyload、Pangu Autospace、Quicklink、Motion、Progress bar、Backgroud JS
  - [Butterfly](https://github.com/jerryc127/hexo-theme-butterfly)[配置帮助文档]：基于Molunerfinn的hexo-theme-melody的基础上进行开发的。(https://docs.jerryc.me/#/zh-cn/)
    - 标签外挂（Tag Plugins）：Note (Bootstrap Callout)、Gallery相册图库、Gallery相册、mermaid
    - 评论：Disqus、Disqusjs、Laibili（来必力）、Gitalk、Valine、Utterances、Facebook Comments
    - 分享：AddThis、Sharejs、Addtoany
    - 搜索系统：Algolia、本地搜索
    - 网站验证
    - 分析统计：百度统计、谷歌分析、腾讯分析
    - 广告：谷歌广告、手动广告配置
    - 数学公式：MathJax、KaTeX
    - 美化/特效：自定义主题色、网站背景、footer 背景、打字效果、静止彩带、动态彩带、canvas-nest、鼠标点击效果、页面美化、自定义字体、网站副标题、主页top_img显示大小、页面加载动画preloader
    - PWA
    - 字数统计
    - 图片大图查看模式：fancybox、medium_zoom
    - Snackbar 弹窗
    - 豆瓣
    - Inject
    - CDN
  -  [Fluid](https://github.com/fluid-dev/hexo-theme-fluid)：是基于 Hexo 的一款 Material Design 风格的主题，界面简洁，展示效果不错[配置帮助文档](https://hexo.fluid-dev.com/docs/)
    - 本地搜索
    - 在线聊天：daovoice
    - Tag 插件
    - LaTeX 数学公式
    - Mermaid 流程图
    - 音乐播放器
  - [Melody](https://github.com/Molunerfinn/hexo-theme-melody)：简洁大方，丰富的第三方支持。[配置帮助文档](https://molunerfinn.com/hexo-theme-melody-doc/)
    - 评论系统：Disqus、Laibili（来必力）、Gitment、Gitalk、Valine
    - 分享系统：AddThis、Sharejs
    - 搜索系统：Algolia、本地搜索
    - 分析统计：百度统计、谷歌分析、腾讯分析
    - 广告：谷歌广告、访问日志(UV 和 PV)、busuanzi
    - 数学公式：MathJax、KaTeX
    - 字数统计
    - 文章置顶 
    - 相册
    - [添加音乐](https://liwenhau.github.io/2019/11/15/blogAddMusic/)
    - [视频播放](https://liwenhau.github.io/2020/02/27/video1/#星星还是那么亮（Cover-爱如潮水）)

##### 主题配置
- 我选择了Melody作为博客的主题，记录几个主要的配置。

###### 平滑升级
- Melody支持平滑升级，由于`_config.yml`配置文件会根据用户更改，在`git pull`时会要求commit和merge。解决方法是将主题文件的`_config.yml`复制到hexo工作目录下的`source/_data/melody.yml`，没有`_data`就创建一个。之后用`git pull`就可以平滑升级了。

###### 创建分类页
- 分类页
`hexo new page categories`
- 修改`source/categories/index.md`这个文件
```
---
title: 分类
date: 2018-01-05 00:00:00
type: "categories"
---
```

- 标签页
`hexo new page tags`
- 修改`source/tages/index.md`这个文件

```
---
title: 标签
date: 2018-01-05 00:00:00
type: "tags"
---
```

- 相册页
`hexo new page gallery`
- 修改`source/gallery/index.md`这个文件

```
---
title: Gallery
date: 2018-01-05 00:00:00
type: "gallery"
---
```

- melody 提供了一个叫做`gallery`的标签，让你能够在`markdown`文件里生成`gallery-item`。修改你刚刚创建的`source/gallery/index.md`，并加上`gallery` 标签。

```
{% gallery img-url [title] %}
```

```
---
title: Gallery 
date: 2018-01-05 00:00:00
type: "gallery"
---
{% gallery https://ws1.sinaimg.cn/large/8700af19gy1fp5i6o2vghj20ea0eajse melody %}
{% gallery https://user-images.githubusercontent.com/12621342/37325500-23e8f77c-26c9-11e8-8e24-eb4346f1fff5.png background %}
{% gallery https://ws1.sinaimg.cn/large/8700af19gy1fp5i64zaxqj20b40b474b demo1 %}
{% gallery https://ws1.sinaimg.cn/large/8700af19ly1fn2h26q32uj21120kudqq demo2 %}
{% gallery https://ws1.sinaimg.cn/large/8700af19ly1fnhdaimi40j218g0p0dic demo3 %}
{% gallery https://ws1.sinaimg.cn/large/8700af19ly1fn2i5kjh2pj21120kuncd %}
```

![效果图](/images/pasted-7.png)

- 404页
`hexo new page 404`
- 修改`source/404/index.md`这个文件

```
---
title: 404
date: 2019-10-13 15:49:05
layout: 404
permalink: /404
---
```

###### 配置文件
- 更改主题文件夹下的配置文件`_config.yml`

- 语言
  - 默认语言是：`en`，支持列表：

|语言|参数|
|--|--|
|英语|en|
|简体中文|zh-Hans|

```
language: en
```

- 代码高亮主题
  - Melody支持5种代码高亮主题：default、darker、pale night、light、ocean (从v1.5.5开始支持)
```
highlight_theme: default # default/darker/pale night/light
```

- 代码换行
  - 如果你不希望在代码块的区域里有横向滚动条的话，那么你可以考虑开启这个功能。
```
code_word_wrap: true
```

  - 然后找到你站点的hexo配置文件_config.yml，将line_number改成false:
```
highlight:
  enable: true
  line_number: false # <- 改这里
  auto_detect: false
  tab_replace:
```

  - 接着运行一下hexo clean后再运行hexo g生成新的文章。

- 社交图标
  - 如果你只想使用`v4`的图标, 你只要访问[`font-awesome v4`](https://fontawesome.com/v4.7.0/)去找图标名，并且图标前缀通常就是 fa。举个例子, 配置 melody.yml：
```
social:
  github fa: https://github.com/Molunerfinn
  weibo fa: http://weibo.com/mybluedreams
  rss fa: https://Molunerfinn/atom.xml
  ...
```

- 导航菜单
  - 在右上角的区域是导航菜单项。Hexo有默认的/和/archives的路径。配置melody.yml
```
menu:
  Home: /
  Archives: /archives
  Tags: /tags
  Categories: /categories
```

  - 你也可以修改菜单项名称，比如：
```
menu:
  Blog: /
  Posts: /archives
  MyTags: /tags
  MyCategories: /categories
```

- 顶部图
  - 顶部图是theme-melody最神奇的配置项. 它拥有`true`、`false`或者具体图片`url`三种值。配置`melody.yml`：
```
top_img: true  // true/false/具体图片url
```

- 顶部图高度控制
  - 从 `v1.7.0` 版本开始，你可以通过设置 `top_img_height` 来控制顶部图的高度。默认值是 60，意味着顶部图会占据 60% 的页面高度。所以如果你喜欢，你可以设置成 100，这样你就能获得占据整个页面的顶部图了！配置 `melody.yml`：
```
top_img_height: 60
```

- 文章版权
  - 为你的博客文章展示文章版权和许可协议。配置`melody.yml`
```
post_copyright:
  enable: true
  license: CC BY-NC-SA 3.0 # 协议名称
  license_url: https://creativecommons.org/licenses/by-nc-sa/3.0/ # 协议说明地址
```

- 文章广告区
  - 在你的文章页面里加上广告！你可以放置一个你自己想展示的广告或者也可以是个音乐播放器等等。不过没研究出来怎么放音乐播放器。
```
adv:
  enable: true
  info: <a href="https://www.vultr.com/?ref=7231808"><img src="https://www.vultr.com/media/banner_1.png" width="728" height="90"></a>
```

- 头像
  - 最好是长宽相等的头像
```
avatar: https://xxxx.jpg
```

- 友链
  - 你可以在侧边栏配置相应的友情链接。格式如下：
```
links_title: Links   # 配置友链的标题文字
links:
  Molunerfinn: https://molunerfinn.com # 名称：URL
  PiEgg: https://piegg.cn
  Elody: https://piegg.cn
```

![友链效果图](/images/pasted-8.png)

- 目录
  - 你的文章能够拥有一个清晰的目录列表。目录位于侧边栏，并且会随着滚动条的滚动自动展开目录结构。
```
toc:
  enable: true # or false
  number: true # or false. 版本v1.5.6新增
```

  - 为特定的文章配置特定的目录章节数字，在你的文章md文件的头部，加入toc_number项，并配置`true`或者`false`即可。
```
title: Hi, theme-melody!
tags:
  - hexo
  - hexo theme
toc_number: false   # < add toc_number to here. 版本v1.5.6新增
date: 2017-09-07
---
```

- 页脚自定义文本 `v1.5.5+`
  - `footer_custom_text`是一个给你用来在页脚自定义文本的选项。通常你可以在这里写ICP备案号、码云声明文本等。支持HTML。
```
footer_custom_text: Hi, welcome to my <a href="https://molunerfinn.com">blog</a>!
```

  - 如果你配置成`hitokoto`，那么底部文字将会生成随机的谚语：
```
footer_custom_text: hitokoto
```

- 特效
  - 点击效果
```
fireworks: true # 烟花
canvas_ribbon: # 彩带
  enable: true
  size: 150
  alpha: 0.6
  zIndex: -1
  click_to_change: false

```

###### 第三方支持
- 评论系统
  - Melody支持多种评论系统，我这里选择Valine
```
valine:
  enable: false # if you want use valine,please set this value is ture
  appId: # leancloud application app id
  appKey: # leancloud application app key
  notify: false # valine mail notify (true/false) https://github.com/xCss/Valine/wiki
  verify: false # valine verify code (true/false)
  pageSize: 10 # comment list page size
  avatar: mm # gravatar style https://valine.js.org/#/avatar
  lang: zh-cn # i18n: zh-cn/en
  placeholder: Just go go # valine comment input placeholder(like: Please leave your footprints )
  guest_info: nick,mail,link #valine comment header in
```

- 搜索系统
  - 这里使用本地搜索，需要安装`hexo-generator-search`. 根据它的文档去做相应配置。注意格式只支持 xml。
```
local_search:
  enable: true # or false
  labels:
    input_placeholder: Search for Posts
    hits_empty: "We didn't find any results for the search: ${query}" # if there are no result
```

  - hexo项目的`_config.yml`配置：
```
search:
  path: search.xml
  field: post
```

- 分析统计
  - 这里使用百度分析，登录百度统计的官方网站，找到你百度统计的统计代码
```
baidu_analytics: 你的代码
```

- 数学公式
  - 原先编写公式都是使用的MathJax，主题文档里推荐的KeTeX，比MathJax更轻量，加载页面更优越一下，这次就选KeTex。
  - 首先禁用MathJax（如果你配置过 MathJax 的话），然后修改你的melody.yml以便加载katex.min.css:
```
katex:
  enable: true
  cdn:
    css: https://cdn.jsdelivr.net/npm/katex@latest/dist/katex.min.css
```

  - 你不需要添加katex.min.js来渲染数学方程。相应的你需要卸载你之前的 hexo 的 markdown 渲染器以及hexo-math，然后安装新的hexo-renderer-markdown-it-plus:
```
# 替换 `hexo-renderer-kramed` 或者 `hexo-renderer-marked` 等hexo的markdown渲染器
# 你可以在你的package.json里找到hexo的markdwon渲染器，并将其卸载
npm un hexo-renderer-marked --save
# or
npm un hexo-renderer-kramed --save
# 卸载 `hexo-math`
npm un hexo-math --save
# 然后安装 `hexo-renderer-markdown-it-plus`
npm i @upupming/hexo-renderer-markdown-it-plus --save
```

  - 注意到 hexo-renderer-markdown-it-plus 已经无人持续维护, 所以我们使用 @upupming/hexo-renderer-markdown-it-plus。 这份 fork 的代码使用了 @neilsustc/markdown-it-katex 同时它也是 VSCode 的插件Markdown All in One所使用的, 所以我们可以获得最新的 KaTex 功能例如 \tag{}。
  - 你还可以通过 @neilsustc/markdown-it-katex 控制 KaTeX 的设置，所有可配置的选项参见：[katex官网](https://katex.org/docs/options.html)。 比如你想要禁用掉 KaTeX 在命令行上输出的冗长的警告信息，你可以在根目录的 _config.yml 中使用下面的配置将 strict 设置为 false:
```
markdown_it_plus:
  plugins:
    - plugin:
      name: "@neilsustc/markdown-it-katex"
      enable: true
      options:
        strict: false
```

  - 当然，你还可以利用这个特性来定义一些自己常用的 macros。因为 KaTeX 更快更轻量，因此没有 MathJax 的功能多（比如右键菜单）。为那些使用 MathJax 的用户，我们也为 KaTeX 默认添加了 Copy As TeX Code 的功能。

- 字数统计 v1.3.0+
  - 打开 hexo 工作目录：`npm install hexo-wordcount --save` or `yarn add hexo-wordcount`，配置melody.yml:
```
wordcount:
  enable: true
```

- 文章置顶 v1.6.0+
  - 打开 hexo 工作目录`npm uninstall hexo-generator-index --save`，然后`npm install hexo-generator-index-pin-top --save`。在文章的front-matter区域里添加top: True属性来把这篇文章置顶。

#### 部署配置（deploy）
|参数|	描述|
|--|--|
|type|	git|
|repo|	仓库路径|
|branch|	分支名称|

  - deploy是网站的部署的，repo就是仓库(Repository)的简写。branch选择仓库的哪个分支。配置这两个值我们就可以将hexo和GitHub关联起来，也就是将hexo生成的文章部署到GitHub上，
```
deploy:
  type: git
  repo: https://github.com/YourgithubName/YourgithubName.github.io.git
  branch: master
```

#### 文件配置
- Front-matter是文件最上方以 `---` 分隔的区域，用于指定个别文件的变量：

```
title: Hello World
date: 2013/7/13 20:46:25
---
```

- 以下是预先定义的参数，您可在模板中使用这些参数值并加以利用

|参数|	描述|
|--|--|
|layout|	布局|
|title|	标题|
|date|	建立日期|
|updated|	更新日期|
|comments|	开启文章的评论功能|
|tags|	标签（不适用于分页）|
|categories|	分类（不适用于分页）|
|permalink|	覆盖文章网址|

#### 布局配置
- Hexo 有三种默认布局：post、page 和 draft，它们分别对应不同的路径，而您自定义的其他布局和 post 相同，都将储存到 source/_posts 文件夹。

|布局|	路径|
|--|--|
|post|	source/_posts|
|page|	source|
|draft|	source/_drafts|

##### post
- layout布局默认是post类型，也就是使用代码`hexo new paper`创建的文章，会在source文件夹下的_post里面。
- 而new这个命令其实是：
```
hexo new [layout] [title]
```

- 只不过这个layout默认是post罢了。

##### page
- 如果你想另起一页，那么可以使用
```
hexo new page board
```

- 系统会自动给你在source文件夹下创建一个board文件夹，以及board文件夹中的index.md，这样你访问的board对应的链接就是http://xxx.xxx/board

##### draft
- draft是草稿的意思，也就是你如果想写文章，又不希望被看到，那么可以
```
hexo new draft newpage
```

- 这样会在source/_draft中新建一个newpage.md文件，如果你的草稿文件写的过程中，想要预览一下，那么可以使用
```
hexo server --draft
```

- 在本地端口中开启服务预览。
- 如果你的草稿文件写完了，想要发表到post中，
```
hexo publish draft newpage
```

- 就会自动把newpage.md发送到post中。

#### 菜单栏（menu）
- 可以配置页面的导航，其中，About这个你是找不到网页的，因为你的文章中没有about这个东西。如果你想要的话，可以执行命令
```
hexo new page about
```

- 它就会在根目录下source文件夹中新建了一个`about`文件夹，以及`index.md`，在`index.md`中写上你想要写的东西，就可以在网站上展示出来了。
- 如果你想要自己再自定义一个菜单栏的选项，那么就
```
hexo new page yourdiy
```

- 然后在主题配置文件的menu菜单栏添加一个 `Yourdiy : /yourdiy`，注意冒号后面要有空格，以及前面的空格要和`menu`中默认的保持整齐。然后在`languages`文件夹中，找到`zh-CN.yml`，在`index`中添加`yourdiy: '中文意思'`就可以显示中文了。

#### 定制（customize）
- 在这里可以修改你的个人`logo`，在`source/css/images`文件夹中放入自己要的`logo`，再改一下`url`的链接名字就可以了。
- `favicon`是网站中出现的那个小图标的`icon`，找一张你喜欢的`logo`，然后转换成`ico`格式，放在`images`文件夹下，配置一下路径就行。
- `social_links` 可以显示你的社交链接，而且是有`logo`的。

#### 侧边栏(widgets)
- 侧边栏的小标签，如果你想自己增加一个，比如我增加了一个联系方式，那么我把`communication`写在上面，在`zh-CN.yml`中的`sidebar`，添加`communication: '中文'`。
- 然后在`hueman/layout/widget`中添加一个`communicaiton.ejs`，填入模板：
```html
<- % if (site.posts.length) { % >
    <-div class="widget-wrap widget-list">
        <-h3 class="widget-title"><-%= __('sidebar.communiation') %-><-/h3>
        <-div class="widget">
            <-!--这里添加你要写的内容--->
        <-/div>
    <-/div>
<- % } % >
```

#### 搜索框(search)
- 默认搜索框是不能够用的，
```
you need to install hexo-generator-json-content before using Insight Search
```

- 它已经告诉你了，如果想要使用，就安装这个插件。

#### 评论系统(comment)
- 这里的多数都是国外的，基本用不了。这个`valine`好像不错，还能统计文章阅读量，可以自己试一试，链接。

#### 如何让博文列表不显示全部内容
- 默认情况下，生成的博文目录会显示全部的文章内容，如何设置文章摘要的长度呢？答案是在合适的位置加上`<!--more-->`即可。  

```
使用github pages服务搭建博客的好处有：
1. 全是静态文件，访问速度快；
2. 免费方便，不用花一分钱就可以搭建一个自由的个人博客，不需要服务器不需要后台；
3. 可以随意绑定自己的域名，不仔细看的话根本看不出来你的网站是基于github的；
< !--more-- >
4. 数据绝对安全，基于github的版本管理，想恢复到哪个历史版本都行；
5. 博客内容可以轻松打包、转移、发布到其它平台；
6. 等等；
```

![展示结果](/images/pasted-6.png)

### 指令总结

|指令|简写|作用|
|--|--|--|
|init|i|初始化，创建一个hexo项目目录，不穿参数，则直接放在当前目录|
|new|n|默认创建一个post文章|
|sever|s|启动本地预览服务|
|generate|g|根据md文件，生成html文件，放在public目录中。可以添加-w参数，实时更新，更加方便|
|clean|c|清楚之前generate的文件|
|deploy|d|部署到代码仓库|

### [表情配置](https://haojen.github.io/2016/09/03/Emoji-Demo/)
- 使用方法:
  - 比如你想发一个笑脸😄 直接输入笑脸对应的 emmoji 编码 `:smile:`就可以。[查看全部 emoji 表情编码](http://emoji.codes/)
- 启用 emoji 的方法:
  - 卸载默认的`markdown`引擎: 打开终端, 去往博客的根目录下执行 `npm un hexo-renderer-marked --save`
  - 然后安装新的解析引擎: `npm i hexo-renderer-markdown-it --save` 和其`emoji`插件 : `npm install markdown-it-emoji --save`
  - 配置`_config.yml`文件
```
markdown: 
	plugins:   
		- markdown-it-abbr   
		- markdown-it-footnote   
		- markdown-it-ins    
		- markdown-it-sub    
		- markdown-it-sup    
		- markdown-it-emoji  //用emoji插件  
```

  - `hexo clean` && `hexo deploy -g` 查看效果


## git分支进行多终端工作
- 利用git的分支系统，可以解决多个终端同时进行文件工作的问题，这样每次打开不一样的电脑，只需要进行简单的配置和在github上把文件同步下来，就可以无缝操作了。

### 机制 
- 机制是这样的，由于`hexo d`上传部署到github的其实是hexo编译后的文件，是用来生成网页的，不包含源文件，没有`source`等源文件在内。
- 也就是上传的是在本地目录里自动生成的`.deploy_git`里面。
- 其他文件 ，包括我们写在`source`里面的，和配置文件，主题文件，都没有上传到github。所以可以利用git的分支管理，将源文件上传到github的另一个分支即可。

### 上传分支
- 首先，先在github上新建一个hexo分支，如图：
![github新建分支](/images/pasted-3.png)
- 然后在这个仓库的settings中，选择默认分支为hexo分支（这样每次同步的时候就不用指定分支，比较方便）。
![设置默认分支为hexo](/images/pasted-4.png)
- 然后在本地的任意目录下，打开git bash，
```
git clone git@github.com:ZJUFangzh/ZJUFangzh.github.io.git
```

- 将其克隆到本地，因为默认分支已经设成了hexo，所以clone时只clone了hexo。
- 接下来在克隆到本地的ZJUFangzh.github.io中，把除了.git 文件夹外的所有文件都删掉
- 把之前我们写的博客源文件全部复制过来，除了.deploy_git。这里应该说一句，复制过来的源文件应该有一个.gitignore，用来忽略一些不需要的文件，如果没有的话，自己新建一个，在里面写上如下，表示这些类型文件不需要git：
```
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/
```

- 注意，如果你之前克隆过theme中的主题文件，那么应该把主题文件中的.git文件夹删掉，因为git不能嵌套上传，最好是显示隐藏文件，检查一下有没有，否则上传的时候会出错，导致你的主题文件无法上传，这样你的配置在别的电脑上就用不了了。
- 而后
```
git add .
git commit –m "add branch"
git push
```

- 这样就上传完了，可以去你的github上看一看hexo分支有没有上传上去，其中node_modules、public、db.json已经被忽略掉了，没有关系，不需要上传的，因为在别的电脑上需要重新输入命令安装 。
![代码上传完成的目录](/images/pasted-5.png)
- 这样就上传完了。

### 更换电脑操作
- 一样的，跟之前的环境搭建一样，
  - 安装git：
```
sudo apt-get install git
```

  - 设置git全局邮箱和用户名
```
git config --global user.name "yourgithubname"
git config --global user.email "yourgithubemail"
```

  - 设置ssh key
```
ssh-keygen -t rsa -C "youremail"
#生成后填到github和coding上（有coding平台的话）
#验证是否成功
ssh -T git@github.com
ssh -T git@git.coding.net #(有coding平台的话)
```

  - 安装nodejs
```
sudo apt-get install nodejs
sudo apt-get install npm
```

  - 安装hexo
```
sudo npm install hexo-cli -g
```

  - 但是已经不需要初始化了，直接在任意文件夹下，
```
git clone git@………………
```

  - 然后进入克隆到的文件夹：
```
cd xxx.github.io
npm install
npm install hexo-deployer-git --save
```

  - 生成，部署：
```
hexo g
hexo d
```

  - 然后就可以开始写你的新博客了
```
hexo new newpage
```

- Tips:
  - 不要忘了，每次写完最好都把源文件上传一下
```
git add .
git commit –m "xxxx"
git push
```

  - 如果是在已经编辑过的电脑上，已经有clone文件夹了，那么，每次只要和远端同步一下就行了
```
git pull
```

## 博客网站优化
- 网站推广不是我的重点，之后的具体内容可以原文：[文章链接](https://blog.csdn.net/sinat_37781304/article/details/82729029)

### SEO
- 百度SEO
- 谷歌SEO

### 网站统计
- 百度统计
- leanCloud文章阅读量统计
- 不蒜子访问量人次统计