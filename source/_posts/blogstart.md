---
title: Github Page和hexo搭建博客
filename: blogstart
tags:
- other  
categories:
- other
---

# jekyll or hexo

* jekyll是Github Pages推荐使用的静态博客生成器，然而它需要的Ruby环境，Ruby相关依赖国内安装极其困难，特别是各种主题的依赖包，费了九牛二虎之力也没有搭建成功。
* hexo依赖环境安装简单，只需要node.js和git，主流NexT主题符合个人口味，还有详尽的中文文档，上手门槛较低。

# 搭建流程

hexo搭建博客时需要注意**源文件**和**部署文件**：

* 源文件：包括hexo代码——themes(主题文件)、source(md博客文章)目录等；
* 部署文件：hexo generate或hexo deploy之后生成的代码，在public目录中。

可以使用2个不同的分支分别管理这两种文件，hexo分支作为工作分支存放源文件及文章，而master分支将存放hexo部署资源public的内容。

## 1.Github上创建远程仓库：

* 创建repo，命名为*usrname.github.io*，不要勾选初始化，否则后续步骤hexo init时会提示非空目录报错。
* 设置Github Pages在master分支上构建。


## 2.拷贝远程仓库：

git clone远程仓库到本地，此时是个空目录。创建工作分支hexo：
``` bash
checkout -b hexo
```

## 3.初始化hexo：

在本地仓库目录下执行：
``` bash
npm install -g hexo-cli         # 安装 hexo
hexo init                       # 初始化本地仓库文件夹
npm install                     # 安装依赖包
npm install hexo-deployer-git   # 安装 deploy，用于同步public/*到远程仓库
```

init成功后根目录下如下：
* \_config.yml:站点的配置文件，可以配置大部分参数，要与themes里的\_config.yml区分开；
* package.json:应用程序的信息；
* scaffolds：模板文件夹，新建文章文件时默认填充的内容；
* source：存放用户资源的文件夹，Markdown和HTML文件会被解析到public文件夹。
* themes：主题文件夹，hexo根据主题生成静态页面，NexT主题的配置文件在其目录下的_config.yml。  

由于网络问题可能会出在下面WARN卡很久，耐心等待：
```bash
npm WARN deprecated hexo-bunyan@2.0.0: Please see https://github.com/hexojs/hexo-bunyan/issues/17
npm WARN deprecated resolve-url@0.2.1: https://github.com/lydell/resolve-url#deprecated
npm WARN deprecated urix@0.1.0: Please see https://github.com/lydell/urix#deprecated
```

## 4.修改站点配置文件，根目录下_config.yml:

``` yaml
themes: next
deploy:
type: git           # 类型
repo: ssh://git@github.com/Tipan/tipan.github.io.git      # Repository地址，注意github.com后“:”需要改成“/”
branch: master      # 分支名称设置为 master, hexo d时会部署到该分支
```

下载[*NexT*](https://github.com/iissnan/hexo-theme-next/releases)主题解压到themes目录下，并重命名为next。

## 5.提交源文件
在*source/\_post*下修改文章后，将修改提交到远程仓库的hexo分支
``` bash
git add .
git commit -m "msg"
git push origin hexo
```

## 6.生成静态文件并部署到master分支上：

``` bash
hexo deploye   # 或者简写hexo d
```
打开*username.github.io*地址即可看见

### 参考链接
* [Getting started with GitHub Pages](https://docs.github.com/en/github/working-with-github-pages/getting-started-with-github-pages)
* [hexo文档](https://hexo.io/zh-cn/docs/)
* [NexT文档](http://theme-next.iissnan.com/getting-started.html)
* [我的个人博客之旅：从jekyll到hexo](https://blog.csdn.net/u011475210/article/details/79023429#next%E8%BF%9B%E9%98%B6)
* [通过git来维护hexo静态站点远程仓库的流程](https://tophat.top/posts/2ca3dccb.html)
