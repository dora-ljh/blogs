---
title: 第一次搭建博客：Hexo + Next 主题
date: 2023-04-04 14:23:03
tags: [Hexo, Next, 博客搭建]
categories: [教程]
---

最近，我决定尝试搭建自己的博客，记录一下自己的成长和学习过程。在此过程中，我选择了使用 Hexo 作为博客框架，搭配 hexo-theme-next 主题。本文将详细记录搭建过程中的各个步骤，希望对初次搭建博客的朋友们有所帮助。

<!-- more -->

## 安装 Hexo
-----------

首先，我们需要在电脑上安装 Hexo。访问 [Hexo 官网](https://hexo.io/zh-cn/)，根据官网教程，通过以下命令安装 Hexo 至全局：

```bash
npm install hexo-cli -g
```

## 初始化项目
---------

使用以下命令初始化一个新的 Hexo 项目：

```bash
hexo init blogs
```

## 修改站点配置
----------

接下来，修改站点配置文件 `_config.yml`，将其配置为自己的个人信息：

```yaml 
title: dora-ljh的个人博客
subtitle: ''
description: '保持专注和努力，勇往直前，成为一名非常成功的Web前端工程师！'
keywords:
author: dora-ljh
language: zh-CN
timezone: ''
```

## 安装 hexo-theme-next 主题
-------------------------

通过 npm 的方式直接安装 hexo-theme-next 主题：

```bash 
npm install hexo-theme-next
```

然后在 `_config.yml` 中配置主题为 next：

```yaml 
theme: next
```

## 选择主题
--------
Next 主题包含四个子主题，分别是 Muse、Mist、Pisces 和 Gemini。我们可以在配置文件 `_config.next.yml` 中选择自己喜欢的子主题。例如，选择 Pisces：
```yaml
#scheme: Muse
#scheme: Mist
scheme: Pisces
#scheme: Gemini
```

## 创建关于界面、分类界面和标签界面
--------------------

创建关于界面：

```bash
hexo new page about
```

在 `source/about/index.md` 文件中填写关于界面的内容。
```markdown
---
title: 关于
---

```

创建分类界面：

```bash 
hexo new page categories
```

在 `source/categories/index.md` 文件中添加以下内容：

```yaml 
---
title: 分类
type: categories
---
```

创建标签界面：

```bash 
hexo new page tags
```

在 `source/tags/index.md` 文件中添加以下内容：

```yaml 
---
title: 标签
type: tags
---
```

## 安装 hexo-word-counter 插件
---------------------------
安装 hexo-word-counter 插件，可以实现文章字数统计和阅读时长估计功能：

```bash 
npm install hexo-word-counter --save
```

## 预览个人博客
----------

使用以下命令预览个人博客：

```bash 
hexo s --debug
```

## 使用 GitHub Actions 部署至 GitHub Pages
--------------------
1.  建立名为 `<你的 GitHub 用户名>.github.io` 的储存库，若之前已将 Hexo 上传至其他储存库，将该储存库重命名即可。

2.  将 Hexo 文件夹中的文件 push 到储存库的默认分支，默认分支通常名为 `main`，旧一点的储存库可能名为 `master`。


*   将 `main` 分支 push 到 GitHub：

    ```
    $ git push -u origin main
    ```

*   默认情况下 `public/` 不会被上传(也不该被上传)，确保 `.gitignore` 文件中包含一行 `public/`。整体文件夹结构应该与 [范例储存库](https://github.com/hexojs/hexo-starter) 大致相似。


3.  使用 `node --version` 指令检查你电脑上的 Node.js 版本，并记下该版本 (例如：`v16.y.z`)

4.  在储存库中建立 `.github/workflows/pages.yml`，并填入以下内容 (将 `16` 替换为上个步骤中记下的版本)：



.github/workflows/pages.yml
```

name: Pages  
  
on:  
 push:  
 branches:  
 - main \# default branch  
  
jobs:  
 pages:  
 runs-on: ubuntu-latest  
 permissions:  
 contents: write  
 steps:  
 - uses: actions/checkout@v2  
 - name: Use Node.js 16.x  
 uses: actions/setup-node@v2  
 with:  
 node-version: "16"  
 - name: Cache NPM dependencies  
 uses: actions/cache@v2  
 with:  
 path: node_modules  
 key: ${{ runner.OS }}-npm-cache  
 restore-keys: |  
 ${{ runner.OS }}-npm-cache  
 - name: Install Dependencies  
 run: npm install  
 - name: Build  
 run: npm run build  
 - name: Deploy  
 uses: peaceiris/actions-gh-pages@v3  
 with:  
 github_token: ${{ secrets.GITHUB_TOKEN }}  
 publish_dir: ./public  
 
```

5.  当部署作业完成后，产生的页面会放在储存库中的 `gh-pages` 分支。

6.  在储存库中前往 `Settings > Pages > Source`，并将 branch 改为 `gh-pages`。

7.  前往 `https://<你的 GitHub 用户名>.github.io` 查看网站。

部署完成后，就可以通过访问 `https://<你的 GitHub 用户名>.github.io` 来预览个人博客了。

至此，我们已经完成了整个博客的搭建过程。希望这篇文章能够帮助你顺利搭建自己的个人博客！
