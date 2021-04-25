---
title: "基于hugo搭建个人站点"
date: 2021-04-26T04:09:23+08:00
draft: false
---

### 1.为什么使用hugo？

以前使用wordpress搭建网站，插件太多，完善起来比较费时费力。现在，静态博客也很受欢迎，hugo就是GO语言开发的一个静态博客生成器。

### 2.安装hugo

mackbook上直接使用`brew install hugo`安装hugo，安装完成后使用`hugo version`来查看hugo的版本。

### 3.创建站点

在Github目录下打开终端，或者cd到Github目录下：

`hugo new site site_name`

其中，si te_name为github账号名称加上`.github.io`，例如：

`hugo new site gary-hertel.github.io`

创建成功后会显示一些信息，诸如：

```bash
Congratulations! Your new Hugo site is created...
Just a few more steps and you're ready to go:

1. Download a theme into the same-named folder.
   Choose a theme from https://themes.gohugo.io/ or
   create your own with the "hugo new theme <THEMENAME>" command.
2. Perhaps you want to add some content. You can add single files
   with "hugo new <SECTIONNAME>/<FILENAME>.<FORMAT>".
3. Start the built-in live server via "hugo server".

Visit https://gohugo.io/ for quickstart guide and full documentation.
```

这就表示站点已经创建成功了。

然后进入到站点目录：

```bash
cd gary-hertel.github.io
```

使用`tree`查看目录结构如下：

```bash
.
├── archetypes
│   └── default.md
├── config.toml
├── content
├── data
├── layouts
├── static
└── themes

6 directories, 2 files
```

接下来需要为我们的网站指定一个主题，这里我们选择`even`这个主题:

```git clone https://github.com/olOwOlo/hugo-theme-even themes/even```

将该主题增加到网站的配置文件中，这样才能生效：

```bash
echo 'theme = "even"' >> config.toml
```

测试下是否成功，运行：

```bash
$ hugo serve
```

这里是按照别的教程操作的，但是发生了报错，查看even这个主题的文档后发现有如下内容：

>## Installation 
>
>```bash
>$ git clone https://github.com/olOwOlo/hugo-theme-even themes/even
>```
>
>**Important:** Take a look inside the [`exampleSite`](https://github.com/olOwOlo/hugo-theme-even/tree/master/exampleSite) folder of this theme. You’ll find a file called [`config.toml`](https://github.com/olOwOlo/hugo-theme-even/blob/master/exampleSite/config.toml). **To use it, copy the [`config.toml`](https://github.com/olOwOlo/hugo-theme-even/blob/master/exampleSite/config.toml) in the root folder of your Hugo site.** Feel free to change it.
>
>**Important:** This theme uses [Hugo Pipes](https://gohugo.io/hugo-pipes/introduction/). Modifying contents in `assets` requires the extended version to be installed.
>
>**NOTE:** For this theme, you should use **post** instead of **posts**, namely `hugo new post/some-content.md`.

这里提示我们查看`themes/even/exampleSite`目录下有一个示例的`config`配置文件，我们需要将这个文件复制到站点根目录，覆盖原文件，这样才能够使站点生效。配置文件中的信息可以查看一下，然后进行相应的修改。

### 4.增加文章

在站点根目录：

`hugo new post/first_article.md`

查看`gary-hertel.github.io/content/post`目录下新增了一个`first_article.md`的markdown文档，打开之后对其进行编辑即可，注意将`draft:true`修改为`false`.

撰写文章的方法就是通常开发者常用的markdown格式。

### 5.查看站点

在站点根目录下：

`Hugo serve`

然后就可以在浏览器中输入`http://127.0.0.1:1313/`进行查看了，在撰写文章或者进行配置修改等等操作时，内容会自动更新。

### 6.生成静态页面

`hugo -d docs`

静态页面会保存至站点根目录下的docs文件夹。

每次更新后我们都需要执行一下这条命令。

### 7.部署到github

在github上新建一个公开仓库，名为github用户名加上`.github.io`，例如`gary-hertel.github.io`

在仓库的settings的pages设置中，Source那里设置为：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gpwno5ivpyj31ap0u0dv6.jpg)

说明如下：

- 数据源默认使用主分支下的根目录，建议改为 docs 目录；
- 自定义域名如果留空，则默认仓库名就是你的域名，比如我这里的 gary-hertel.github.io；
- 如果配置了自定义域名，启用 HTTPS 需要等待一段时间才能生效；

### 8.总结

使用hugo搭建个个人博客还是不错的，一开始要花些时间折腾和摸索，后续就使用起来比较方便了，也不需要购买服务器去部署，部署在github上可能国内访问较慢，可以考虑部署到gitee.

