# 基于 Hugo 的个人博客体系不完全指南




<br>

什么是 hugo ？

Hugo 是采用 Go 语言开发的一种开源网站搭建框架，具有速度快、灵活、可扩展性强的特点。

<!--more-->

<br>



**主要内容**

- 博客搭建
- 博客美化:(fas fa-rocket fa-fw): 
- 博客功能扩展
- 博文发布的解决方案



## 博客搭建



### 需准备

- Git

  Git 的安装方法请参阅[这里](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%AE%89%E8%A3%85-Git)

   

### Step 1：安装 Hugo

> 本示例基于 MacOS ，其他操作系统下 Hugo 的安装方法请参阅 [安装](https://www.gohugo.cn/getting-started/installing/)

打开 Terminal ，使用 homebraw 安装：

```shell
brew install hugo
```

验证安装是否成功，请执行：

```shell
hugo version
```

返回结果如下则表明安装成功。

![image-20220602141204635](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220602141204635.png)



> `extended` 表明 Hugo 版本为扩展版本，支持 Sass / Scss , 更多信息，请参考 [Sass](https://www.sass.hk/) 



### Step 2：创建一个新的博客项目

准备一个 Hugo 的工作目录，在该目录下执行：

```shell
hugo new site <博客项目名>
```

出现如下信息则证明创建成功。

![image-20220602141822833](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220602141822833.png)



### Step 3：安装主题

进入该博客项目目录，目录结构如下：

![image-20220602141923808](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220602141923808.png)

- archetypes
- content
- data
- layouts
- static
- themes
- config.toml



**如何添加主题呢？**

> 本次示例选择的是 [LoveIt](https://github.com/dillonzq/LoveIt) 主题

首先，从 Github 下载主题并将其添加到博客项目目录的 theme 目录中：

```shell
cd mi2mcn
git init
git submodule add https://github.com/dillonzq/LoveIt.git themes/LoveIt
```

![image-20220602152605906](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220602152605906.png)

接下来，需要配置 config.toml 文件， 我们选择直接拷贝 LoveIt 主题中的实例网站的配置文件：

```shell
cp -p themes/LoveIt/exampleSite/config.toml .
```

然后将配置文件中的 `themesDir` 项更改为如下内容：

```shell
# 主题目录
themesDir = "./themes"
```

最后，执行如下命令，在本地开启一个 Web 服务器：

```shell
hugo server -D
```

![image-20220602154532239](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220602154532239.png)

根据提示信息，在本地浏览器中访问 `http://localhost:1313/`  ，返回如下页面则代表搭建成功。

![image-20220602154707964](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220602154707964.png)

> 这里的图片显示有误，是因为我们没有修改配置文件中默认的图片路径，后续根据个人情况修改即可。



**添加文章**

> 所有文章都在 `content/post` 目录下

- 使用命令添加

  ```
  hugo new posts/my-first-post.md
  ```

- 直接在 `content/posts` 目录下添加 markdown 文件即可



## 博客美化

> 每个主题的配置都是有差异的，本示例以 `LoveIt` 主题为基准进行个性化设置和美化。

##### 配置文件

`LoveIt` 主题的配置文件主要分为以下几个部分：

```toml
# baseURL
# 主题目录
# 网站通用设置： 标题、语言设置、作者设置等
# 网站导航栏设置
# 网站页面底部设置
# 网站主页设置：主页信息、主页文章列表、主页社交信息等
# 网站分区设置
# 网站列表设置
# 网站APP端设置
# 网站搜索设置
# 文章页面设置
# 文章评论设置
# 第三方库配置
# 页面 SEO 配置
# TypeIt 配置
# 网站分析配置
# CDN 配置
# hugo 解析文档配置
# 网站地图配置

## 支持多语言
```

##### 主页美化

在站点目录下的 `config.toml` 中按需修改即可。



## 博客功能扩展

### 全文搜索

搜索功能我们使用第三方的 `algolia` 实现

### 评论功能

评论功能我们使用第三方的 `valine` 实现

## 博客发布方案

#### github pages

#### 腾讯云静态网站托管




