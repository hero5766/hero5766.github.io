title: hexo搭建博客记录
tags: []
categories: []
date: 2020-05-30 18:56:00
---
- 之前的博客很多图都挂掉了，想重新改改，但是恢复好像难度比较大。所以索性重新建了一个博客，顺便把创建博客的过程记录一下吧。
<!--more-->

## 准备工作
### [github](https://github.com)操作
- 创建仓库  
  在github新建一个以你的用户名命名的仓库：`用户名.github.io`，这样将来访问地址就是：`http://用户名.githu.io`了。所以，每个github账户只能创建一个可以直接使用域名访问的仓库。
- 生成ssh添加到githubƒ
```
git config --global user.name "yourname"
git config --global user.email "youremail"
ssh-keygen -t rsa -C "youremail"
```

### 安装node.js和npm
  - windows：nodejs选择LTS版本就行了。
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
  - windows：到git官网上下载,[Download git](https://gitforwindows.org/),下载后会有一个Git Bash的命令行工具，以后就用这个工具来使用git。
  - linux：对linux来说实在是太简单了，因为最早的git就是在linux上编写的，只需要一行代码
```
sudo apt-get install git
```

### 安装hexo  
- 前面git和nodejs安装好后，就可以安装hexo了，你可以先创建一个文件夹blog，然后cd到这个文件夹下（或者在这个文件夹下直接右键git bash打开）
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

- 搭建完成之后，查看npm 安装各 hexo 插件的情况
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
```

- 安装完后重新构建建立的hexo项目即可解决。
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

  - 即可在localhost:4000/admin/中编辑博文了。
  - 然后，Deploy之前，还需要编辑配置文件_config.yml。(否则会出现Error: Config value "admin.deployCommand" not found或者Error: spawn hexo ENOENT之类的报错。)
    - Windows则在末尾加上
![_config.xml的配置](/images/pasted-1.png)
- 然后在同级目录新建hexo-pubish.bat文件，文件内容如下：
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

  - 编辑完毕后，就可以点击Deploy，直接部署发布Github博客上。
![点击deploy部署](/images/pasted-2.png)
- PS：关于Hexo Admin插入图片
  - Hexo Admin可以直接复制图片粘贴，然后自动下载到source/images目录并重命名。但在Windows中粘贴后会出现裂图。这时就需要手动把括号中的前后两个斜杠去掉，就能正常显示。
- 密码保护
  - 打开`setting`，点击`Setup authentification here`输入用户名，密码，密钥，下面会自动生成配置文件，复制加在hexo根目录下的`_config.yml`中：
```
admin:
  username: myfavoritename
  password_hash: be121740bf988b2225a313fa1f107ca1
  secret: a secret something
```

  - 重启hexo，就可以看到登录页面了
- 发布文章
  - 进入后台之后点击Deploy，里面的Deploy按钮是用来执行发布脚本的，所以我们先在博客根目录下新建个目录admin_script，然后在目录中新建一个脚本hexo-g.sh，里面写下下面代码然后保存，
```
hexo g && hexo d
```

  - 然后给hexo-g.sh加入可执行权限
```
chmod +x hexo-d.sh
```

  - 然后在_config.yml中的admin下添加
```
admin:
  username: myfavoritename
  password_hash: be121740bf988b2225a313fa1f107ca1
  secret: a secret something
  deployCommand: ./admin_script/hexo-d.sh
```

  - 设置发布执行的脚本，点击Deploy就会执行这个命令并提交到GitHub上。

### 绑定域名
- 如果有了域名，可以将域名和上一步建立的仓库进行绑定
  - 首先在终端中ping一下仓库网址，获取IP，`ping 用户名.github.com`
  - 配置域名的CNAME的指向获取的IP
  - 登录GitHub，进入之前创建的仓库，点击settings，设置Custom domain，输入你的域名fangzh.top
  - 然后在你的博客文件source中创建一个名为CNAME文件，不要后缀。写上你的域名。  
- 过不了多久，再打开你的浏览器，输入你自己的域名，就可以看到搭建的网站啦！

## hexo的使用

### 创建项目
- hexo安装完成后，接下来初始化一下hexo
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
- 在文件根目录下的_config.yml，就是整个hexo框架的配置文件了。可以在里面修改大部分的配置。详细可参考[官方的配置](https://hexo.io/zh-cn/docs/configuration)描述。

#### 网站配置
|参数|	描述|
|---|---|
|title|	网站标题|
|subtitle|	网站副标题|
|description|	网站描述|
|author|	您的名字|
|language|	网站使用的语言|
|timezone|	网站时区。Hexo 默认使用您电脑的时区。时区列表。比如说：America/New_York, Japan, 和 UTC 。|

- 其中，description主要用于SEO，告诉搜索引擎一个关于您站点的简单描述，通常建议在其中包含您网站的关键词。author参数用于主题显示文章的作者。

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
```

- 主题文件夹中的`_config.xml`是主题的相关配置，具体可以根据主题帮助文档更改就可以了。

##### 主题推荐
- hexo主题不少，看个人喜好，我总结了几个网上推荐并且比较多人用的主题。
  1. [NexT](https://github.com/theme-next/hexo-theme-next)：极简风，功能集成多。缺点是太过简单界面不漂亮。[配置帮助文档](https://theme-next.js.org/)
    - 设置CDN
    - 数学公式：MathJax、Katex
    - 评论：Disqus、DisqusJS、LiveRe、Gitalk、Valine (China)、Changyan (China)
    - 分析：Google Analytics、Baidu Analytics (China)、Growingio Analytics、CNZZ Analytics (China)
    - 统计：LeanCloud (China)、Firebase、Busuanzi Counting
    - 发布工具：Widgetpack Rating、AddThis
    - 搜索工具：Algolia Search、Local Search、Swiftype
    - 聊天工具：Chatra、Tidio
    - 其他工具：PJAX、Fancybox、MediumZoom、Lazyload、Pangu Autospace、Quicklink、Motion、Progress bar、Backgroud JS
  2. [Butterfly](https://github.com/jerryc127/hexo-theme-butterfly)[配置帮助文档](https://docs.jerryc.me/)
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
  3. [Fluid](https://github.com/fluid-dev/hexo-theme-fluid)[配置帮助文档](https://hexo.fluid-dev.com/)
    - 本地搜索
    - 在线聊天：daovoice
    - Tag 插件
    - LaTeX 数学公式
    - Mermaid 流程图
    - 音乐播放器
  4. [Melody](https://github.com/Molunerfinn/hexo-theme-melody)[配置帮助文档](https://molunerfinn.com/)
    - 评论系统：Disqus、Laibili（来必力）、Gitment、Gitalk、Valine
    - 分享系统：AddThis、Sharejs
    - 搜索系统：Algolia、本地搜索
    - 分析统计：百度统计、谷歌分析、腾讯分析
    - 广告：谷歌广告、访问日志(UV 和 PV)、busuanzi
    - 数学公式：MathJax、KaTeX
    - 字数统计
    - 文章置顶 
    - [添加音乐](https://liwenhau.github.io/2019/11/15/blogAddMusic/)
    - [视频播放](https://liwenhau.github.io/2020/02/27/video1/#星星还是那么亮（Cover-爱如潮水）)

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

- 它就会在根目录下source文件夹中新建了一个about文件夹，以及index.md，在index.md中写上你想要写的东西，就可以在网站上展示出来了。
- 如果你想要自己再自定义一个菜单栏的选项，那么就
```
hexo new page yourdiy
```

- 然后在主题配置文件的menu菜单栏添加一个 Yourdiy : /yourdiy，注意冒号后面要有空格，以及前面的空格要和menu中默认的保持整齐。然后在languages文件夹中，找到zh-CN.yml，在index中添加yourdiy: '中文意思'就可以显示中文了。

#### 定制（customize）
- 在这里可以修改你的个人logo，在`source/css/images`文件夹中放入自己要的logo，再改一下`url`的链接名字就可以了。
- `favicon`是网站中出现的那个小图标的icon，找一张你喜欢的logo，然后转换成ico格式，放在`images`文件夹下，配置一下路径就行。
- `social_links` 可以显示你的社交链接，而且是有logo的。

#### 侧边栏(widgets)
- 侧边栏的小标签，如果你想自己增加一个，比如我增加了一个联系方式，那么我把communication写在上面，在zh-CN.yml中的sidebar，添加communication: '中文'。
- 然后在hueman/layout/widget中添加一个communicaiton.ejs，填入模板：
```
<% if (site.posts.length) { %>
    <div class="widget-wrap widget-list">
        <h3 class="widget-title"><%= __('sidebar.communiation') %></h3>
        <div class="widget">
            <!--这里添加你要写的内容-->
        </div>
    </div>
<% } %>
```

#### 搜索框(search)
- 默认搜索框是不能够用的，
```
you need to install hexo-generator-json-content before using Insight Search
```

- 它已经告诉你了，如果想要使用，就安装这个插件。

#### 评论系统(comment)
- 这里的多数都是国外的，基本用不了。这个valine好像不错，还能统计文章阅读量，可以自己试一试，链接。

#### 如何让博文列表不显示全部内容
- 默认情况下，生成的博文目录会显示全部的文章内容，如何设置文章摘要的长度呢？答案是在合适的位置加上<!--more-->即可。  

```
使用github pages服务搭建博客的好处有：
1. 全是静态文件，访问速度快；
2. 免费方便，不用花一分钱就可以搭建一个自由的个人博客，不需要服务器不需要后台；
3. 可以随意绑定自己的域名，不仔细看的话根本看不出来你的网站是基于github的；
<!--more-->
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
  - 比如你想发一个笑脸😄 直接输入笑脸对应的 emmoji 编码 :smile:就可以。[查看全部 emoji 表情编码](http://emoji.codes/)
- 启用 emoji 的方法:
  - 卸载默认的 markdown 引擎: 打开终端, 去往博客的根目录下执行 `npm un hexo-renderer-marked --save`
  - 然后安装新的解析引擎: `npm i hexo-renderer-markdown-it --save` 和其 emoji 插件 : `npm install markdown-it-emoji --save`
  - 配置_config.yml文件
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

  - 但是已经不需要初始化了，

  - 直接在任意文件夹下，
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