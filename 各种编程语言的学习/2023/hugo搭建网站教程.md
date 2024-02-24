# 手把手教你用Hugo制作个人网站

## 前期准备
### homebrew安装
#### homebrew介绍
homebrew是Mac的包管理器，能够免去安装软件过程中的注册账号、寻找版本、下载、解压、安装等步骤，直接使用简单的命令行即可安装软件安装包。
#### homebrew的安装
按下`command+space`调出`spotlight`，在输入框中输入`终端`或`terminal`打开终端，在终端中输入如下命令行，即可下载安装homebrew。
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
看到`==> Installation successful!`就说明安装成功了。

若碰到homebrew安装相关问题可点击[此处](https://www.cnblogs.com/xibushijie/p/13335988.html)查看部分相应的解决办法。
### git安装及配置
#### git安装
在安装好homebrew的基础上，就可以使用指令来安装git了。在终端中输入如下命令：
``` 
brew install git 
```
当在终端窗口中输入`git`后，窗口中出现有关git的用法说明，即说明安装成功。
除以上方法外，你还可以参考[官方安装指导](https://git-scm.com/download/mac)使用其他方法进行安装。
#### git配置
1. 用户信息配置

安装完成后，输入如下命令与github账号绑定（将双引号中的账号信息对应更改为自己的github账号信息）：
```
git config --global user.name "example name"           
git config --global user.email "example@example.com  
```
2. SSH key配置

输入如下代码创建ssh key:
```
ssh-keygen -t rsa -C "example@example.com"
```
按照终端窗口的提示敲击`enter`键，直至其恢复至等待命令状态，然后输入如下命令查询创建的ssh public key：
```
cat id_rsa.pub
```
记录下终端窗口出现的密钥。

3. 远端仓库添加密钥

进入github的settings页面，找到`SSH and GPG`选项。进入选项页面后点击如图箭头所指处添加SSH公钥。

<img width="804" alt="image" src="https://user-images.githubusercontent.com/82250598/169526476-76381a25-4ea7-46d9-9421-8151851128da.png">

进入页面后，按照如下图所示来填写相应内容。

<img width="804" alt="image" src="https://user-images.githubusercontent.com/82250598/169527237-dce7d21c-6fa2-4753-bef2-00cbb8caa8f7.png">

添加完SSH公钥后，在本地测试是否添加成功。在终端中输入如下代码：

```
ssh -T git@github.com
```
如果终端输出`You 've successfully authenticated`则表明公钥添加成功。


### hugo安装
同上git安装，在终端中输入：
```
brew install hugo
```
## 网站搭建及配置

### 本地网站的快速搭建
在想创建的文件目录下打开终端，输入：
```
hugo new site Filename 
```
`Filename`可以自己任意命名，运行完该命令后，会在终端运行的目录下新建一个名为`Filename`的文件夹，文件夹内包含网站的所有内容文件，且作为网站根目录。后文的`Filename`文件夹都指的是这个你自己命名的文件夹。

### 主题的使用
进入[hugo主题页面](https://themes.gohugo.io)选择自己喜爱的主题,本文以[stack主题](https://themes.gohugo.io/themes/hugo-theme-stack/)为例，且在主题所提供的example site文件的基础上进行改动。
1. 主题的下载（git下载）

<img width="407" alt="image" src="https://user-images.githubusercontent.com/82250598/169532580-7d87c53f-28ef-4921-9ddc-e30a5ab3c60d.png">

点进[stack主题的github下载主页](https://github.com/CaiJimmy/hugo-theme-stack)，点击`code`，复制主题文件的https链接，然后在所搭建的本地网站文件夹（即`Filename`文件夹）中，找到`themes`文件夹，并在该目录下打开终端，在终端中输入如下代码下载主题文件：
```
git clone https://github.com/CaiJimmy/hugo-theme-stack.git
```
2. 主题相关文件的位置

将主题文件夹中`archetypes`文件夹和**exampleSite**文件夹中的`content`文件夹和`config.yaml`移至网站的根文件目录下（即`Filename`文件夹中），并将根目录下的.toml文件删掉即可。（在官方说明中提到可以使用多种后缀的配置文件，但是测试的时候发现多种配置文件不能同时存在于文件根目录下，可能会存在冲突问题）

3. 使用效果本地预览

在网站根目录下打开终端，输入如下代码即可搭建本地网站：
```
hugo serve --buildDrafts
```
终端输出如下：

<img width="570" alt="image" src="https://user-images.githubusercontent.com/82250598/169531579-632db51a-5b3c-45a6-9e96-e975a7960f27.png">

将图示处的网址拷贝到浏览器中打开，即可打开本地网站。该网站能够随着配置文件的变化而实时刷新。

### 主题的配置
由于可配置的变量过多，本文仅论述如何通过**exampleSite**的文件以及主题的layout相关代码找寻变量设置格式的方法，让你能够自行的找到自己想修改的部分。
#### 配置文件变量说明
首先你可以点击[此处](https://docs.stack.jimmycai.com/zh/)来进入该主题的官方文档，文档中有对于yaml后缀的配置文件变量的基本描述。通过阅读官方文档，你可以大致了解config文档中各个变量大致的作用，从而找到你想改变的项所相关联的变量。

其次，可以阅读**exampleSite**中的`config.yaml`文件，由于给出的示例网站已经设置了网站的一些变量，所以可以根据已有的变量设置来以此类推出一些类似的变量设置。例如对于页面语言的设置，示例网站配置文件中的相关代码如下：
```
languages:
    en:
        languageName: English
        title: Example Site
        weight: 1
    zh-cn:
        languageName: 中文
        title: 演示站点
        weight: 2
    ar:
        languageName: عربي
        languagedirection: rtl
        title: موقع تجريبي
        weight: 3
```
观察代码结构，可以发现每种语言的设置包括`languageName``title``weight`几项，通过对其变量进行修改，并查看本地网页的刷新情况，能够分别推知三者分别指的是语言选项卡的示意文字、网站对应语言下的标签页题目以及在选择栏中的先后次序。同时，也可仿照上示代码中的格式，添加新的语言（新的语言需是官方文档中说明的i8n所支持的语言）。其他一些在**exampleSite**中已有的设置，也可以通过上述找规律的方式来找到设置方式。

对于在示例网站中未进行设置的变量，就需要去阅读该主题`Layouts`文件夹中的文件了。通过阅读主题中相关的html文件代码，可以获知主题制作者设置的在配置文件中对于变量进行赋值的格式。本文提供的推理思路适用于和我一样没学过html语言的同学。

以配置文件中空缺的`favicon`为例，按照主题的设置，favicon的相关配置在`/layouts/partials/head/head.html`中，在该文件中，我们可以找到与favicon相关的如下代码段（一般是直接看有无相同的关键字）
```
{{ with .Site.Params.favicon }}
    <link rel="shortcut icon" href="{{ . }}" />
{{ end }}
```
可以看出，代码表示用作favicon的图标绑定的数据为`{{ . }}`也就是说，应当绑定的是双括号内的数据，但通过与配置文件中相同的icon设置方式无法改变favicon之后，我认为两个变量的赋值方式存在区别，于是在`layouts`中找到了与sidebar中的icon设置相关的代码语段：
```
{{ $icon := default .Pre .Params.Icon }}
```
因此在这里我将`.`推断为格式后缀前的点，用于分割名称和后缀的，但是直接将欲设置的favicon放置在与sidebar的icon相同的目录下，并直接将图片的名称赋值于favicon并不能达到更改站点图标的效果。于是我结合一些其他主题官方文档中的说明得知，一些需要在建立网站之后访问的图片，是需要放置到根目录的`static`文件夹下的，该文件夹下的文件能够保留在最后建立的在线网站文件夹内。因此可以通过将favicon的图片移动到`static`文件夹内，然后通过直接将图片名字.后缀赋值给`favicon`变量来实现。

同理，其他与网站的框架相关的设置都可通过阅读主题内`layouts`文件夹下的对应html文件来推知相应的修改方法。（当然有些是真的很难读懂，那就只能等到学了html语言或者直接联系主题作者的方式来实现了😛）

#### 主题其他部分文件说明
1. /content/pages

当我们查看示例网站时，我们会发现中文页面的`sidebar`缺少了很多英文页面有的选项，但这个设置在`config.yaml`文件中是被省略的，也就是说对于中英文来说，`config.yaml`是一视同仁的，那是什么导致了两个语言下的页面存在差异呢？首先观察示例网站本身，我们会发现中文页面存在着`关于`页面，其他的都不存在，于是我们需要去找到两个语言下页面中的相关差异。通过翻找主题的文件夹，我们可以找到其差异体现在`content/pages`目录下的如图所示文件中：

<img width="918" alt="image" src="https://user-images.githubusercontent.com/82250598/169553163-87677af0-cdae-4295-805c-300c68d3c099.png">

即每个页面分项中多了一个`index.zh-cn.md`的文件，说明该文件可能与中文页面中sidebar的选项有关，于是仿照该文件命名，在每个页面分项文件夹内建立一个`index.zh-cn.md`文件，然后再来看看文件头部代码的差别：
```
---
title: 关于
menu:
    main: 
        weight: -90
        params:
            icon: user
---
```
```
---
title: About
description: Hugo, the world's fastest framework for building websites
date: '2019-02-28'
aliases:
  - about-us
  - about-hugo
  - contact
license: CC BY-NC-ND
lastmod: '2020-10-09'
menu:
    main: 
        weight: -90
        params:
            icon: user
---
```
可以发现两个页面的结构还是非常相似的，其实他们两者所套用的就是主题的`layouts`文件夹中相应文件的相对性设置，由于`关于`页面相当于是文档，没有其他的功能，而其他几个页面都有其对应的页面设置，因此在增加`index.zh-cn.md`时，应在对应的页面头部代码段段基础上进行修改，尤其要注意`layout`变量，这是设置页面格式的关键变量。

2. /content/catagories

该文件夹下放置的是分类标签的相关文档，即是你点击任意分类时，所进入的分类页面中，最上方的窗口卡片中的相关信息显示。由于该主题能够自动添加你文章中的新增分类和标签，因此在此处可以新增与已有分类相同名字的文件夹，然后在该文件夹内建立`_index.md`文件，在文件内输入如下格式的代码段即可。
```
---
title: "Test" //标签名称，复制代码时注意删除“//”后的所有注释内容
description: "This is an example category" //标签描述
slug: "test" 
image: "hutomo-abrianto-l2jk-uxb1BY-unsplash.jpg" //标签内嵌图片
style:
    background: "#2a9d8f"
    color: "#fff"                //标签个性化设置
---
```
3. /content/post

该文件夹内就是放置你的博客文章，建立方式同categories文件夹下的建立方式。

### 在线网站搭建
在github上新建一个与你的用户名相同的，后缀为`.github.io`的仓库，例如`name.github.io`。复制该库的https链接备用。
然后在`Filename`文件夹下打开终端，并在终端中输入如下代码：
```
hugo --theme="hugo-theme-stack" --baseUrl="https://name.github.io/" --buildDrafts
```
在`Filename`文件夹下会生成一个`public`文件夹。继续在终端中输入如下代码，将该文件夹与仓库同步：
```
cd public
git init
git add .
git commit -m "messages"
git remote add origin 你的仓库的https地址
git push -u origin master 
```
当你配置和修改好你的网站后，你就可以将它假设到线上服务器供你的亲朋好友访问了:)
### 在线网页更新
之后每次修改了相关配置或增加了博客文章时，都可通过如下方式更新你的网页。

在`Filename`文件夹下打开终端，依次输入如下代码：
```
hugo --theme="hugo-theme-stack" --baseUrl="https://name.github.io/" --buildDrafts
cd public
git add .
git commit -m "update"
git push -u origin master 
```



