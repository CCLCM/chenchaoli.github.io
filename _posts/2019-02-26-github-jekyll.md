---
layout: post
title:  "使用github+jekyll搭建个人博客!"
date:   2019-02-26 16:03:06 +0800
categories: jekyll
tag: github+jekyll
---

* content
{:toc}

------------------------
#### 1.注册 gitbug

<br/>

>在这里就不在写如何注册gitbub了,这边有个文章 [[如何注册gitbug]](https://jingyan.baidu.com/article/4ae03de3d6f9c53eff9e6bdd.html),[github网址](https://pages.github.com)

<br/>

#### 2.登陆生成github主页

<br/>

> 登录github

<br/>

![登录github]({{ '/styles/images/github_jekyll/2.jpg' | prepend: site.baseurl  }})

<br/>
>登陆成功之后，如图点击左上角的 "+" 图标 点击选着 New repository

<br/>
![点击选着 New repository]({{ '/styles/images/github_jekyll/1.jpg' | prepend: site.baseurl  }})

<br/>

>在repository name 中填写需要的名字 xxx.github.io <font color = "red">注意:使用.github.io结尾<font>

>Initialize this repository with a README 勾不勾选都可以

<br/>
![repository name]({{ '/styles/images/github_jekyll/3.jpg' | prepend: site.baseurl  }})

<br/>
>创建完成之后点击分支的setting

<br/>
![setting]({{ '/styles/images/github_jekyll/4.jpg' | prepend: site.baseurl  }})

<br/>
>下拉到 gitHubPages

>source 点击 master branch

<br/>
![master branch]({{ '/styles/images/github_jekyll/5.jpg' | prepend: site.baseurl  }})

<br/>
>生成your site published at https://xxx.github.io/ 即是博客主页

<br/>
![site published]({{ '/styles/images/github_jekyll/6.jpg' | prepend: site.baseurl  }})

<br/>
#### 3.将xxxx.github.io 下载到本地

<br/>
> 点击 Singed in as XXX

<br/>
![site published]({{ '/styles/images/github_jekyll/7.jpg' | prepend: site.baseurl  }})

<br/>
> 点击XXXX.github.io

<br/>
![site published]({{ '/styles/images/github_jekyll/8.jpg' | prepend: site.baseurl  }})

<br/>
>点击clone or download

<br/>
![site published]({{ '/styles/images/github_jekyll/9.jpg' | prepend: site.baseurl  }})

<br/>
#### 4.将下载jekyll主题
<br/>

> [jekyll中文网站](https://www.jekyll.com.cn/),[jekyll主题网站](http://jekyllthemes.org/)

<br/>
>下载自己喜欢的 jekyll 模板,下载之后解压文件,<font color="red">将解压后文件夹中的所有文件拷贝</font>到本地xxxx.github.io 内部

<br/>
> <font color="red">需要修改_config.yml</font>

<br/>
> <font color="red">将baseurl: "/XXXX" 注释掉即可</font>

#### 5.提交jekyll主题到github

<br/>
>git连接远程仓库在这里不再描述可点击连接学习

<br/>
[<font color="red">点击学习git关连远程仓库</font>](https://www.cnblogs.com/flora5/p/7152556.html)
<br/>

<br/>
提交项目到中之后 在浏览器中输入自己的博客地址,即可看到自己设置的博客
