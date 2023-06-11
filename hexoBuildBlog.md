---
title: hexo搭建个人博客网站
date: 2022-03-28 22:47:08
tags:
  - hexo
categories:
  - 配置教程
cover: /img/photo-1676676676239-5e4f826c6143.jpg
---

基于hexo+github搭建个人博客网站，使用butterfly主题

<!-- more -->   

# 网站效果
[https://juzihhu.github.io/](https://juzihhu.github.io/)

https://juzihhu.xyz



整个项目文件已经可从百度网盘下载


# 前期准备
### github账号与特殊仓库

创建repository 命名为{GitHub账户名.github.io}
![](https://gitee.com/juzihhu/image_bed/raw/master/img/202303032032156.png)

在浏览器输入`GitHub账户名.github.io`,出现如下结果则表示成功
![](https://gitee.com/juzihhu/image_bed/raw/master/img/202303032032157.png)

### 安装Node.js

**注意最好不要使用最新版本，注意版本兼容问题**

安装教程可以参考：https://blog.csdn.net/m0_67402096/article/details/126077470

[2021最新Node.js官网安装教程，配置环境变量（图文详细）](../../../%E5%9B%BE%E7%89%87/%E6%A1%8C%E9%9D%A2%E6%96%87%E6%A1%A3%E5%9B%BE%E7%89%87/121468849)

https://nodejs.org/dist/v4.2.3/node-v4.2.3-x86.msi   

https://nodejs.org/dist/v4.2.3/node-v4.2.3-x64.msi   

### 安装git,配置git环境

具体可以参考我的这篇博客
[git与github项目上传与下载](https://juzihhu.github.io/2022/05/04/git%E4%B8%8Egithub%E9%A1%B9%E7%9B%AE%E4%B8%8A%E4%BC%A0%E4%B8%8E%E4%B8%8B%E8%BD%BD/)

### 检查安装环境配置

打开cmd命令行(win+r 输入cmd回车)分别执行
node -v
npm -v
git --version

![](https://gitee.com/juzihhu/image_bed/raw/master/img/202303032032158.png)

### 安装Hexo

打开cmd命令行(win+r 输入cmd回车)分别执行
`npm install hexo-cli -g`   

等待自动安装完成后。输入   
 `hexo -v`

 结果如下：

```shell
$ hexo -v
INFO  Validating config
hexo: 6.2.0
hexo-cli: 4.3.0
os: win32 10.0.19043
node: 16.15.0
v8: 9.4.146.24-node.20
uv: 1.43.0
zlib: 1.2.11
brotli: 1.0.9
ares: 1.18.1
modules: 93
nghttp2: 1.47.0
napi: 8
llhttp: 6.0.4
openssl: 1.1.1n+quic
cldr: 40.0
icu: 70.1
tz: 2021a3
unicode: 14.0
ngtcp2: 0.1.0-DEV
nghttp3: 0.1.0-DEV
```



# 网站搭建

### 新建存放网站项目的文件夹

比如我的是新建 HexoBlog

### 右键该文件夹git blash进入

![](https://gitee.com/juzihhu/image_bed/raw/master/img/202303032032159.png)

使用如下指令

`hexo init`
初始化hexo。这个命令和git 仓库命令相似。意思一样都是初始化。

`npm install hexo --save`
初始化hexo基础配置文件。初始化时间比较长，不用急，等完成后输入

`npm install`
配置node。然后输入

`hexo g`（或者 `hexo generate`）
加载hexo基础html、css、js等文件。

在这完成后等于已经在本地创建了一个网页，输入
`hexo s`(或者`hexo serve`)

在浏览器输入：http://localhost:4000

![](https://gitee.com/juzihhu/image_bed/raw/master/img/202303032032160.jpg)

**至此博客的初步框架已经搭建完成**



# 部署

### _config.yam配置文件修改

打开根目录下的 _config.yml 文件,修改如下部分内容 

在部署之前，我们需要先知道博客的部署地址，它需要对应 GitHub 的一个 Repository 的地址，这个信息需要我们来配置一下。 

```js
# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type: git
  repo: git@github.com:juzihhu/juzihhu.github.io.git
  branch: main
```

另外我们还需要额外安装一个支持 Git 的部署插件，名字叫做 hexo-deployer-git，有了它我们才可以顺利将其部署到 GitHub 上面，如果不安装的话，在执行部署命令时会报错。

`npm install hexo-deployer-git --save`
安装成功之后，执行部署命令：
`hexo deploy`

### 访问

浏览器输入博客网站域名
这里我的是 https://juzihhu.github.io/

![](https://gitee.com/juzihhu/image_bed/raw/master/img/202303032032161.jpg)

至此博客网站已经初步搭建成功了


# 网站信息配置以及主题配置

### butterfly主题地址

具体可以参照butterfly的说明文档

https://butterfly.js.org/

### 我的配置介绍

可以直接使用我的配置文件，在使用前需要安装以下插件：

- 图片插件`npm install hexo-renderer-marked`

**解决图床图片不显示：**

当前项目的themes\butterfly\layout\includes目录下有一个head.pug文件，修改该文件的meta信息，会使生成的所有页面都带有该head。在head.pug文件中添加如下内容，结果参见图片。

```js
meta(name="referrer" content="no-referrer")
```

![img](https://gitee.com/juzihhu/image_bed/raw/master/img/202303032024482.png)

参考链接： https://www.jianshu.com/p/5b58ecce6443

- 显示本地图片插件：`npm install hexo-asset-image --save`

把站点配置文件_config.yml中的post_asset_folder选项设为true,这样以后每次执行`hexo new aaaa`新增文章命令时，都会在`_posts`目录下生成`aaaa.md`文章和`aaaa`文件夹，md文章中引用图片使用相对路径格式，如下：

```cpp
![logo](aaaa/logo.png)
```

![image-20230309133331947](https://gitee.com/juzihhu/image_bed/raw/master/img/202303091333619.png)

如果本地图片还不能正常先试试，说明`hexo-asset-image`插件的bug仍未修复，需要修改站点目录下`node_modules\hexo-asset-image\index.js`文件，

17行左右，添加对应的else if字段，如下

```js
if(/.*\/index\.html$/.test(link)) {
  ...
}
else if (link.charAt(link.length - 1) === '/') {
  // link 是文件夹路径情形时
  var endPos = link.length - 1;
}
else {
  ...
}
```

大概在58行左右,找到`$('img').each(function()){`代码段，将其中的

```js
$(this).attr('src', config.root + link + src);
console.info&&console.info("update link as:-->"+config.root + link + src);
```


改为相对路径引用：

```js
$(this).attr('src', src);
console.info&&console.info("update link as:-->" + src);
```

- 主题安装后如果你没有 pug 以及 stylus 的渲染器，请下载安装：

  `npm install hexo-renderer-pug hexo-renderer-stylus --save`

- 搜索设置需要安装：
  `npm install hexo-generator-search --save`

- 数学公式：# KaTeX

```shell
npm un hexo-renderer-marked --save # 如果有安装这个的话，卸载
npm un hexo-renderer-kramed --save # 如果有安装这个的话，卸载
npm i hexo-renderer-markdown-it --save # 需要安装这个渲染插件
npm install katex @renbaoshuo/markdown-it-katex #需要安装这个katex插件
```


https://katex.org/docs/options.html

- 要为Butterfly配上字数统计特性, 你需要如下几个步骤:

`npm install hexo-wordcount --save or yarn add hexo-wordcount`

- 快捷车道

  解压后安装上述包即可使用

  链接：https://pan.baidu.com/s/1WbxhwLB6FGM4kQcPBDENUQ?pwd=rm8y 
  提取码：rm8y 
