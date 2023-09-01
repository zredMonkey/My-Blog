---
title: 将本地代码提交至github仓库
date: 2018-01-01 12:50:18
tags:
- github仓库
- github
- git
- 本地代码
categories:
- github仓库
- github
top_img: /img/githubImage.jpg
cover: /img/githubImage.jpg
keywords: "github仓库"
description: "将本地代码提交至github仓库"
post_meta:
  page:
    date_type: both
    date_format: relative
    categories: true
    tags: true
    label: true
  post:
    date_type: both 
    date_format: relative
    categories: true 
    tags: true
    label: true
---
## 创建github账号：
在Github上创建一个账户。如果您已经拥有一个账户，可以跳过此步骤。

## 创建代码仓库：
在Github上创建一个代码仓库。在Github主页面上，右上角有一个“New Repository”按钮，点击该按钮，输入仓库名称和描述信息，选择公共还是私有仓库（如果您没有选购Github的付费方案，则私有仓库是需要付费的）。在确认信息无误后，点击“Create Repository”按钮即可。

## 安装git
在本地电脑上安装Git。如果您已经安装了Git，可以跳过此步骤。

## 创建和配置密钥
要在 Git 中创建密钥，请按照以下步骤进行操作：

### 1.打开终端或命令提示符，输入以下命令来生成 SSH 密钥：


```
ssh-keygen -t rsa -b 4096 -C "your_email"
```


### 2.按回车键接受默认的文件名和位置。

### 3.输入你的密码短语（如果需要）。

### 4.确认密码短语。

### 5.你的 SSH 密钥现在已经生成。在终端中，输入以下命令来查看你的公钥：

```
cat ~/.ssh/id_rsa.pub
```

### 6.复制你的公钥。

### 7.在 Git 托管服务（如 GitHub、GitLab、Bitbucket 等）的网站上，导航到你的账户设置页面，找到 SSH 密钥设置选项。

### 8.点击 "Add SSH key"（或类似的按钮）。

### 9.将你的公钥粘贴到 "Key" 字段中。

### 10.输入一个描述性的标题以标识此密钥。

### 11.点击 "Add key"（或类似的按钮）。

现在，你已经成功地将 SSH 密钥添加到你的 Git 托管服务中。现在，你可以使用 SSH 协议来与 Git 托管服务进行通信，而不需要每次都输入用户名和密码。

## 创建本地代码仓库：
在本地电脑上创建一个本地代码仓库。使用您喜欢的文件管理器，在您希望保存代码的位置创建一个新的文件夹。然后在终端中进入该文件夹，并执行以下命令：


```
git init
```

这将在该文件夹中创建一个新的Git仓库。

## *添加文件到本地代码仓库（更新）：
将新文件添加到本地代码仓库中。在终端中，执行以下命令：


```
git add .
```

这将把hello-world.py文件添加到Git的暂存区。

## *提交（更新）：
提交更改描述。在终端中，执行以下命令：


```
git commit -m "Initial commit"
```

这将提交文件更改，并把提交描述设置为“Initial commit”。

## 建立本地仓库和github仓库的关联：
在Github仓库页面上，复制仓库的URL地址。然后在终端中执行以下命令：


```
git remote add origin [仓库的URL地址]
```

这将将您的本地仓库与Github仓库关联起来。

## *推送代码到github仓库（更新）：
推送您的代码到Github仓库。执行以下命令：


```
git push -u origin master
```

这将推送您的代码到Github仓库。如果您有多个分支，则将“master”替换为您要推送的分支名称。

完成上述步骤后，您的代码将被提交到Github仓库。如果您打开Github仓库页面，您将看到刚刚提交的代码。在以后的工作中，您可以迭代地修改代码，并将更改推送到Github仓库上。