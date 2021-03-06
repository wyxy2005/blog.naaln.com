---
layout: post
title: Blog = GitHub + Octopress
date: 2013/10/19 12:17:00
categories: 
- 技术
tags: 
- octopress
- github
---

最近有些同学让我分享我的博客主题，其实主题早已经放在GitHub上了。下面索性基于[我Fork的Octopress][1]，讲一下如何使用Octopress在GitHub上建博客。 先来看看我的版本和原始版本的主要不同之处：

**2012-04-13:** 允许单篇文章独立引用CSS和JS，具体文件内容变动见commit[0aa27bb][2]和 [05fcdbf][3]。

1.  [.rvmrc][4] 改成使用我机子上装的Ruby版本（1.9.3），Octopress最低要求是1.9.2。

2.  [Gemfile][5] 把源改成ruby.taobao.org，去掉部分gem的版本号限制，咱都用最新的。

3.  [Rakefile][6] 用haml替换markdown（这个根据自己喜好来，只要是被支持的），时间改成东八区的，降低主题对custom目录的依赖。

4.  [_config.yml][7] 这里都是一些定制化的配置信息，比如日期格式、永久链接、新浪微博等。

5.  [plugins/sh_js.rb][8] [新增] 代码高亮插件，具体介绍看：[Using SHJS for Jekyll][9]

6.  [plugins/tag_generator.rb][10] [新增] 其实Jekyll是支持Tag的，只不过Octopress目前没有官方插件支持，所以我自己搞了个，支持中文。

7.  [.themes/blog/][11] [新增] 这个就是我博客的主题了。

**OK，现在让我们正式开始。** 

## Step1 - 在本机安装Octopress 首先，必须先在本机安装配置

[Git][12]和[Ruby][13]，然后就是把Octopress源码从GitHub上拉下来：
```
   $ git clone git://github.com/jsw0528/octopress.git
```

运行上面的代码后，你会看到： 
```
   Cloning into octopress...
   remote: Counting objects: 4103, done.
   remote: Compressing objects: 100% (1835/1835), done.
   remote: Total 4103 (delta 2341), reused 3556 (delta 1946)
   Receiving objects: 100% (4103/4103), 1.03 MiB | 316 KiB/s, done.
   Resolving deltas: 100% (2341/2341), done.
```

现在源码已经在你机子上了，继续： 
```
   $ cd octopress
   $ bundle install
```

至此，Octopress所需的环境已经搭建成功。 

## Step2 - 安装主题 由于我博客的主题以

`submodule`的方式引入了我们前端团队的[基础样式库][14]，所以需要进行下面两步，把样式库的文件全部拉下来：
```
$ git submodule init
$ git submodule update
```

现在可以安装主题了，

`blog`是主题名称。其实这步就是把`.themes/blog/`内的两个目录复制到项目根目录。 
```
   $ rake install['blog']
```

## Step3 - 连接GitHub Pages 

首先，你得有一个GitHub的帐号，并且已经创建了一个新的Repository。如果你准备用自己的域名的话，Repository的名称可以随便取，不过正常人在正常情况下，一般都是以域名取名的^_^。如果你没有自己的域名，GitHub是提供二级域名使用的，但是你得把Repository取名为

`你的帐号.github.com`，并且，部署的时候会占用你的`master`分支。 **Tips：** 

1.  如果用自己的域名，记得把`source/CNAME`文件内的域名改成你的。还有把域名的A Record指向IP：`207.97.227.245`；
2.  如果用GitHub提供的二级域名，记得把`source/CNAME`删掉； 完成上述准备工作后，运行： 
```
   $ rake setup_github_pages
```

它会提示你输入有读写权限的Repository Url，这个在GitHub上可以找到。Url形如：
```
`git@github.com:jsw0528/MrZhang.me.git`，其中，`jsw0528`是我的帐号，`MrZhang.me`是我的Repository的名称。 
```

## Step4 - 配置你的博客 所有配置项都在

`_config.yml`文件内，用你喜欢的编辑器打开它，然后改成你的数据即可。 

## Step5 - 为备份posts做准备 

数据备份很重要，我们可以把文章备份到另一个分支，比如

`store`。在安装完主题后，系统会自动创建一个空目录`source/_posts`，以后创建的文章都会保存在这个目录内。我们先定位到该目录，然后初始化git（建议新开一个终端窗口，省得不停地切换目录）：
```
   $ cd source/_posts/
   $ git init
   $ touch README.md
   $ git add *
   $ git commit -m 'first commit'
   $ git remote add origin git@github.com:[Username]/[Repository].git
```

commit即可，无需push，因为我们重点是要新建

`store`分支。接下来我们创建并切换到`store`： 
```
   $ git checkout -b store
```

回到刚才的窗口，继续下面的步骤。 git入门，推荐看下

[这篇文章][15]，小清新风格。当然，你也可以使用[GitHub for Mac][16]，备份的准备步骤如下： 

1. 把`source/_posts`目录拖到仓库列表界面的左下角，在弹出新建空仓库的确认框后点「Yes」；
2. 双击进入这个仓库，切换到「Settings」项，填入`git@github.com:[Username]/[Repository].git`后点「Update Remote」；
3. 回到仓库列表界面，点底部的刷新按钮，刷新成功后，列表上的`_posts`将换成你的仓库名；
4. 随便创建一个文件，比如上面的`touch README.md`；
5. 再次进入这个仓库，切换到「Changes」项，你会看到刚刚创建的文件了，点「Commit」按钮；
6. 这个时候可以新建`store`分支了，点软件左下角的分支图标，输入`store`，回车。

## Step6 - 创建一篇文章 码字时，我们可以先在本地起一个服务，预览无误后再部署。新开一个终端窗口（保证定位到项目根目录），运行：
``` 
   $ rake preview
```

然后你就可以使用
`http://localhost:4000`访问你的本地博客了。 创建文章：
``` 
   $ rake new_post['Hello World']
```

然后到

`source/_posts`目录内找文件名形如`YYYY-MM-DD-hello-world.haml`的文件。用你喜欢的编辑器打开它，码字。 如果你对`.themes/blog`内的主题文件做了改动，可以运行下面的命令更新：
```
   $ rake update_source['blog']
   $ rake update_style['blog']
```

## Step7 - 部署 

先把整个项目静态化，然后再部署到GitHub： 
```
   $ rake generate
   $ rake deploy
```

当你看到「Github Pages deploy complete」后，就表示你大功已成。Enjoy! 

**Tips：** Octopress提供的所有`rake`方法，可以运行`rake -T`查看。 

## Step8 - 备份 

回到`source/_posts`目录内，运行下面的命令把文章备份到GitHub上：
```
   $ git add *.haml
   $ git commit -m 'backup posts'
   $ git push origin store
```

到此结束，希望对你有帮助。 放弃臃肿的WP，投入Octopress的怀抱吧～ 像黑客一样写博客！

 [1]: https://github.com/jsw0528/octopress/tree/mrzhang_me/

 [2]: https://github.com/jsw0528/octopress/commit/0aa27bb1ab423dbebd89cf5ffe55f8a5c65d6244#diff-1

 [3]: https://github.com/jsw0528/octopress/commit/05fcdbfced318fa9909011d9eb7a33dd0f8792d1#diff-0

 [4]: https://github.com/jsw0528/octopress/blob/mrzhang_me/.rvmrc

 [5]: https://github.com/jsw0528/octopress/blob/mrzhang_me/Gemfile

 [6]: https://github.com/jsw0528/octopress/blob/mrzhang_me/Rakefile

 [7]: https://github.com/jsw0528/octopress/blob/mrzhang_me/_config.yml

 [8]: https://github.com/jsw0528/octopress/blob/mrzhang_me/plugins/sh_js.rb

 [9]: http://mrzhang.me/blog/using-shjs-for-jekyll.html

 [10]: https://github.com/jsw0528/octopress/blob/mrzhang_me/plugins/tag_generator.rb

 [11]: https://github.com/jsw0528/octopress/blob/mrzhang_me/.themes/blog/

 [12]: http://help.github.com/mac-set-up-git

 [13]: http://www.ruby-lang.org/

 [14]: https://github.com/eDoctor/eDr_assets_Sass

 [15]: http://rogerdudler.github.com/git-guide/index.zh.html

 [16]: http://mac.github.com/
