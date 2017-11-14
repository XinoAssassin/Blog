---
title: 一场因为使用 Git Submodule 引发的血案
tags: 
      - Everything
      - Blog
date: 2017-11-14 15:45:59
---


**前略**
今天继续对 Blog 进行一些更新，上午先是将目前暂挂于 Github Page 的本博客链接上了[自定义域名](http://blog.xinoassassin.com)，但是这样一来就不能强制启用 HTTPS 让我略感不爽，有空解决之。
中午的时候突然冒出来的念头：把 Blog 的主题用 Git Submodule 来进行管理，原因也是用这种形式管理非常适合 Hexo 的结构，遂动手。

我的 _config.yml 中是将 public_dir 挂在 Hexo 目录之外，所以不用动它。而原本就在的 themes/hexo-theme-typescript 已经是一个 Git Repo 了，先将它之中的改动 push 上去，然后删了它。

将原本 themes 文件夹中的内容清空后，我同时删除了该文件夹，这也是之后二十分钟让我不知道错在何处的原因。

删去 themes 文件夹之后我们来添加 submodule, 命令很简单，一句 `git submodule add`就可以搞定，然后会自动帮你 clone 下来并且在根目录下生成 .gitmodules 保存 submodule 信息。

搞定之后尝试跑跑看有没有问题，hexo g 之后发现警告信息："No Layout: index.html". 我心想，不对啊，文件没缺啊，你怎么就找不到 Index 的 Layout 信息了呢？首先想到的是，Git 的 Submodule 是不是用了某种 link, 以至于 hexo 读不到这个文件夹的内容，于是用 ls 查看之，ls 告诉我 theme 和 hexo-theme-typescript 都是实打实的文件夹而不是软链接，百思不得其解，Google 之，发现这个警告代表的文件百分之百是由于文件缺失。然而我的文件都在啊，没缺啊，就认为是 submodule 的问题，参考了别人的 hexo 用 submodule 方式管理的文章之后，我突然想到，会不会是原本文件目录是 "themes" 而我建 submodule 时候用的是 "theme", 缺了个 's' 导致的文件缺失呢？于是我新建了一个文件夹跑了一下 hexo init 看看文件目录结构，果不其然，用的确实是 "themes", 重新把 submodule 建好，用对文件夹名字，运行 hexo s, 打开本地页面，熟悉的主页终于回来了。