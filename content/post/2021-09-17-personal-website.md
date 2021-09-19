---
title: 搭建个人主页(Blogdown+Netlify+Github+AWS)
author: Nathan Huang
date: '2021-09-17'
slug: personal-website
categories:
  - R
tags:
  - Blogdown
draft: no
---

# 搭建流程


教程来自 [统计之都](https://cosx.org/2018/01/build-blog-with-blogdown-hugo-netlify-github/ "统计之都")，步骤稍作修改。

初始设置先在RStudio中的进行如下设置：

`Tools -> Global Options -> Sweave -> Weave Rnw files using:knitr`

`Tools -> Global Options -> Sweave -> Typeset LaTex into PDF using:XeLaTeX`

RStudio第一次运行Blogdown需要安装：

```
install.packages("blogdown")
blogdown::install_hugo()
```

## 在Github创建一个Repo

Repo名字任意，选择`Add a README file`， 同时选择`Add .gitignore with R`

回到RStudio，选择新建一个`File -> New Project -> Version Control -> Git`，输入Repo的网址，创建项目。

在RStudio的`Files`中找到`.gitignore`文件，覆盖原文件，复制粘贴下面内容：

```
.Rproj.user
.Rhistory
.RData
.Ruserdata
public
static/figures
blogdown
```

## 创建本地网站

打开RStudio：`File -> New Project -> New Directory -> Website using blogdown`，主题写**athul/archie**,

![blogtest.PNG](https://dgbp4uvz49ycd.cloudfront.net/blogtest.PNG)


完成后在`Console`运行：

```
blogdown::serve_site()
```
中间有报错内容：

> ERROR 2021/09/17 11:33:37 instagram shortcode: Missing config value for services.instagram.accessToken. This can be set in config.toml, but it is recommended to configure this via the HUGO_SERVICES_INSTAGRAM_ACCESSTOKEN OS environment variable. If you are using a Client Access Token, remember that you must combine it with your App ID using a pipe symbol (<APPID>|<CLIENTTOKEN>) otherwise the request will fail.


在`Files`中找到`config.toml`，加入一行`ignoreErrors = ["error-missing-instagram-accesstoken"]`保存，重新运行`blogdown::serve_site()`。在`config.toml`中把baseURL改为自己的主页(https://nathanhuang.xyz/)。

在`Tools`菜单找到`Addins`，选择`New Post`可以创作新文章。

> Filename处会自动帮你填写为Title处的内容，Filename和Slug还是建议使用字母，尤其是Filename，如果博文里面不需要用到 R 语言的代码计算结果生成图表的话，Format处就选择Markdown格式，这可以省去一些系统生成的步骤，ok，点击Done，就会在/content/post文件夹下面生成一个新文件了，content 文件夹下面的文件就是博客的文章了。---------[统计之都](https://cosx.org/2018/01/build-blog-with-blogdown-hugo-netlify-github/ "统计之都")



完成后通过以下步骤，将本地项目同步到Github。
```
cd <本地项目目录>
git init
git add .
git commit -m "first comment"
git remote add origin https://github.com/<github帐号>/<仓库名称>
git remote -v
git pull origin master --allow-unrelated-histories
git push -u origin master
```

以后修改文件后，将本地项目同步到Github步骤如下：
```
git add .
git commit -m "first comment"
git remote add origin https://github.com/<github帐号>/<仓库名称>
git remote -v
git pull origin master --allow-unrelated-histories
git push -u origin master
```




## 通过Amazon AWS S3部署图床

S3教程来自(https://troyyang.com/2018/02/16/hosting-images-with-aws-s3/)。

通过(https://console.aws.amazon.com/console/home)创建`bucket`，名称任意（这里选择nathanhuang），在`Permission`取消`Block all public access`，上传图片在`Action`选择`make public`。此时生成的节点是http://nathanhuang.s3-website-ap-southeast-1.amazonaws.com/，国内访问不便。下面通过CloudFront自定义域名。


## CloudFront Distribution

在静态托管`Static website hosting`处选择托管，键入索引文件index.html，错误文档error.html。Origin Domain Name中选择刚才所建的S3 Bucket 域名。

部署完成需要等待一段时间，完成之后得到新的访问地址dgbp4uvz49ycd.cloudfront.net，选择一张图片，加上https://，再次访问https://dgbp4uvz49ycd.cloudfront.net/blogtest.PNG，即可完成。



## 设置Netlify

注册登录Netlify后选择`New site from Git`，再点击Github，Brand选择Main，Build Command填写hugo，Publish directory填写public点击`Depoly Site`。

在Namesilo购买个性化域名nathanhuang.xyz，回到Netlify选择`Domain Management->Domain->Add Domain`，输入域名nathanhuang.xyz。在DNS Panel处复制Netlify DNS:
```
dns1.p07.nsone.net
dns2.p07.nsone.net
dns3.p07.nsone.net
dns4.p07.nsone.net
```

在Namesilo处选择Unlock domain，并找到在NameServers并修改成刚才复制的Netlify DNS。

回到Netlify，选择enable HTTPS。

