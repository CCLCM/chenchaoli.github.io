---
layout: post
title:  "使用github+jekyll搭建个人博客!"
date:   2019-02-26 16:03:06 +0800
categories: jekyll
tag: github+jekyll
---

* content
{:toc}


博客搭建		{#chenchaoli}
------------------------
#### 1.注册 gitbug
在这里就不在写如何注册gitbub了,这边有个文章[如何注册gitbug](https://jingyan.baidu.com/article/4ae03de3d6f9c53eff9e6bdd.html)

[github网址](https://pages.github.com)

[jekyll中文网站](https://www.jekyll.com.cn/)



#### 2.生成github主页

登录github

如图点击左上角的 "+" 图标 点击选着 New repository

在repository name 中填写需要的名字 注意:使用.io结尾

Initialize this repository with a README 勾不勾选都可以



创建完成之后点击分支的setting

下拉到 gitHubPages

source 点击 master branch 生成your site published at https://xxx.github.io// 即是博客主页

#### 3.将xxxx.github.io 下载到本地


#### 4.将下载jekyll主题


[jekyll主题网站](http://jekyllthemes.org/)

下载自己喜欢的 jekyll 模板,下载之后解压文件,将文件中的所有文件拷贝到 本地xxxx.github.io 内部

需要修改config.yml

将 baseurl: "/XXXX" 注释掉即可
#### 5.提交jekyll主题到github

提交项目到中之后 在浏览器访问自己的博客,即可看到自己设置的博客


<!-- ![诫子书]({{ '/styles/images/jiezishu.jpg' | prepend: site.baseurl  }}) -->

<!--
[诸葛亮](#) -->
