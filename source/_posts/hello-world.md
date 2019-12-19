---
title: 博客搭建： hexo + github托管
tags: 个人博客
---
最近心血来潮，想要搭建一个自己的博客，花了几天的时间，选择了 GitHub平台托管 + Hexo框架。这样就可>
以安心的莱协作，又不需要定期维护，而且hexo作为一个简洁的博客框架，用它来搭建博客真的非常方便。

## Hexo简介

Hexo是一款基于Node.js的静态博客框架，依赖少易于安装使用，可以方便的生成静态网页托管在GitHub和Codi
ng上，是搭建博客的首选框架。大家可以进入[hexo官网](https://hexo.io/zh-cn/)进行详细查看，因为Hexo>
的创建者是台湾人，对中文的支持很友好，可以选择中文进行查看。

# 第一部分

hexo的初级搭建以及部署到github page上。



## Hexo搭建步骤

1. 安装git
2. 安装node.js
3. 安装hexo
4. github创建个人仓库
5. 生成SSH添加到github
6. 将hexo部署到github
8. 发布文章

## 1.安装git

Git是目前世界上最先进的分布式版本控制系统，可以有效、高速的处理从很小到非常大的项目版本管理。也就是用来管理你的hexo博客文章，上传到GitHub的工具。Git非常强大，我觉得建议每个人都去了解一下。廖雪峰老师的Git教程写的非常好，大家可以了解一下。[Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

windows：到git官网上下载,[Download git](https://gitforwindows.org/),下载后会有一个Git Bash的命令行工具，以后就用这个工具来使用git。

linux：对linux来说实在是太简单了，因为最早的git就是在linux上编写的，只需要一行代码：

```bash
sudo apt-get install git
```

安装好后，用`git --version`来查看一下版本

## 2.安装nodejs

Hexo是基于nodeJS编写的，所以需要安装一下nodeJs和里面的npm工具。

windows：[nodejs](https://nodejs.org/en/download/)选择LTS版本就行了。

linux：

```bash
sudo apt-get install nodejs
sudo apt-get install npm
```

安装完后，命令行检查有没有安装成功：

```bash
node -V
npm -V
```

顺便说一下，windows在git安装完后，就可以直接使用git bash来敲命令行了，不用自带的cmd，cmd有点难用。

## 3.安装hexo

前面git和nodejs安装好后，就可以安装hexo了，你可以先创建一个文件夹blog，然后`cd`到这个文件夹下（或者在这个文件夹下直接右键git bash打开）。

输入命令

```
npm install -g hexo-cli
```

依旧用`hexo -v`查看一下版本

至此就全部安装完了。

接下来初始化一下hexo

```
hexo init blog
```

这里的blog可以根据自己的网站自行修改

```
cd blog //进入blog文件夹
npm install
```

新建完成后，blog文件目录如下

* node_modules: 依赖包 
* public: 存放生成的页面
* scaffolds: 生成文章的一些模板
* source: 用来存放你的文章：
    * _posts: 发表的文章
    * _drafts: 草稿
* theme: 主题
* _config.yml: 博客的配置文件

```
hexo g
hexo server
```

打开hexo的服务，在浏览器输入localhost:4000就可以看到你生成的博客了。

使用ctrl+c可以把服务关掉。

## 4.github创建个人仓库

首先，你先要有一个GitHub账户，没有可以注册一个。

注册完登录后，在GitHub.com中看到一个New repository，新建仓库


创建一个和你用户名相同的仓库，后面加.github.io，只有这样，将来要部署到GitHub page的时候，才会被识别，也就是xxxx.github.io，其中xxx就是你注册GitHub的用户名。我这里是已经建过了。

点击create repository。

## 5.生成SSH添加到github

回到你的git bash中，

```
git config --global user.name "yourname"
git config --global user.email "youremail"
```


这里的yourname输入你的GitHub用户名，youremail输入你GitHub的邮箱。这样GitHub才能知道你是不是对应它的账户。

可以用以下两条，检查一下你有没有输对

```
git config user.name
git config user.email
```


然后创建SSH,一路回车

```
ssh-keygen -t rsa -C "youremail"
```


这个时候它会告诉你已经生成了.ssh的文件夹。在你的电脑中找到这个文件夹。

ssh，简单来讲，就是一个秘钥，其中，`id_rsa`是你这台电脑的私人秘钥，不能给别人看的，`id_rsa.pub`是公共秘钥，可以随便给别人看。把这个公钥放在GitHub上，这样当你链接GitHub自己的账户时，它就会根据公钥匹配你的私钥，当能够相互匹配时，才能够顺利的通过git上传你的文件到GitHub上。

而后在GitHub的setting中，找到SSH keys的设置选项，点击`New SSH key`
把你的`id_rsa.pub`里面的信息复制进去。

在git bash中，查看是否成功

```
ssh -T git@github.com
```

## 6.将hexo部署到github

这一步，我们就可以将hexo和GitHub关联起来，也就是将hexo生成的文章部署到GitHub上，打开站点配置文件 `_config.yml`，翻到最后，修改为YourgithubName，也就是你的GitHub账户。

```bash
deploy:
  type: git
  repo: https://github.com/YourgithubName/YourgithubName.github.io.git
  branch: master
```

这个时候需要先安装deploy-git ，也就是部署的命令,这样你才能用命令部署到GitHub。

```
npm install hexo-deployer-git --save
```

然后

```
hexo clean 
hexo generate 
hexo deploy
```

其中 `hexo clean`清除了你之前生成的东西，也可以不加。
`hexo generate` 顾名思义，生成静态文章，可以用 `hexo g`缩写
`hexo deploy` 部署文章，可以用`hexo d`缩写

注意deploy时可能要你输入username和password。

部署成功，过一会儿就可以在`http://yourname.github.io` 这个网站看到你的博客了！！

## 7.发布文章

接下来你就可以正式开始写文章了。

```
hexo new newpapername
```

然后在source/_post中打开markdown文件，就可以开始编辑了。当你写完的时候，再

```
hexo clean
hexo g
hexo d
```

就可以看到更新了。

当你每一次使用代码

```
hexo new paper
```


它其实默认使用的是`post`这个布局，也就是在`source`文件夹下的`_post`里面。

Hexo 有三种默认布局：`post`、`page` 和 `draft`，它们分别对应不同的路径，而您自定义的其他布局和 `post` 相同，都将储存到 `source/_posts` 文件夹。

|  布局   |       路径       |
| :-----: | :--------------: |
| `post`  | `source/_posts`  |
| `page`  |     `source`     |
| `draft` | `source/_drafts` |

而new这个命令其实是：

```
hexo new [layout] <title>
```


只不过这个layout默认是post罢了。

### page
如果你想另起一页，那么可以使用

```
hexo new page board
```


系统会自动给你在source文件夹下创建一个board文件夹，以及board文件夹中的index.md，这样你访问的board对应的链接就是`http://xxx.xxx/board`

### draft
draft是草稿的意思，也就是你如果想写文章，又不希望被看到，那么可以

```
hexo new draft newpage
```


这样会在`source/_draft`中新建一个newpage.md文件，如果你的草稿文件写的过程中，想要预览一下，那么可以使用

```
hexo server --draft
```


在本地端口中开启服务预览。

如果你的草稿文件写完了，想要发表到post中，

```
hexo publish draft newpage
```


就会自动把newpage.md发送到`post`中。

