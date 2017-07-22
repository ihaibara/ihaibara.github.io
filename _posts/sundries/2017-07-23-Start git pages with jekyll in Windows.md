---
layout: post
title: 在Windows上部署git pages本地环境
categories: 杂项
description: 本文描述了这个博客的搭建过程中遇到的坑坑洼洼
keywords: 杂项
---

*本文描述了这个博客的搭建过程中遇到的坑坑洼洼*

# 使用jekyll在git page上搭建网站

可以看到，本博客的链接是*github.io*。这个是github提供的一个静态网站托管服务，实际上就是个网络主机，但是从零开始搭建网站实在有点在造轮子之嫌，所以github就在pages的[主页](https://pages.github.com/)上推荐大家使用jekyll这个静态网站生成工具。

jekyll这个工具支持主题，很多都是非常漂亮的开源的主题，这对于我这个前段白痴简直就是福音，而且jekyll网站还有非常细致的[文档](http://jekyllrb.com/docs/home/)，国内也有Bootstrap中文网翻译整理的[中文文档](http://jekyll.com.cn/)，git上也有step-by-step的[教程](https://help.github.com/articles/using-jekyll-as-a-static-site-generator-with-github-pages/)。

一个正常的博客搭建流程就是，fork，clone到本地，删删改改，commit，push就可以了。照理我这篇博客大概率就是个无病呻吟贴了o(╯□╰)o。

可是，开源也有开源的坏处，比如

- 好多开源主题的jekyll版本和github上的版本并不兼容，直接fork可能出错，单纯的在线使用比较麻烦。
- jekyll中文文档翻译不全，特别是没有翻译windows上安装jekyll运行环境。

> 在 Windows 下使用 Jekyll
你可以使用 Jekyll running on Windows, 但是官方文档并不建议你在 Windows 平台上安装 Jekyll。

- jekyll的英文文档提供了[Windows上安装jekyll](http://jekyllrb.com/docs/windows/)的教程，但是这个教程光安装jekyll本身就提供了3种方法，莫衷一是，更不要提安装git-pages支持的jekyll了。

所以，我希望把我自己的踩坑经历分享一下，希望后人能少踩坑。
# 整体的技术路线
1. [Jekyll 3 on Windows](https://labs.sverrirs.com/jekyll//2-jekyll-gem.html) 安装好ruby环境
2. 使用[RubyGems 镜像 - Ruby China](http://gems.ruby-china.org/)，并解决SSL链接问题[解决的方法](https://gist.github.com/fnichol/867550)
3. 使用[Setting up your GitHub Pages site locally with Jekyll](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/)进行最后的设置

上面教程中，1、3以及jekyll的官方文档可能有步骤重叠，而且每个人的具体开始和结束的状态都不一样。因此我先介绍下我自身的状态。

本文基于的操作系统是Windows 7 x64系统，Win 10也肯定支持~~算是博文这次，我都装了3遍了~~。本地上已经存在了博客的git版本库，但是没办法本地运行的状态。
最终状态是本地生成通过，localhost看到网页。

下面就是简要的构建说明，建议本文和技术路线中的教程对照着看。


# 在Windows上安装jekyll本地环境

这个安装过程参考了[Jekyll 3 on Windows]( https://labs.sverrirs.com/jekyll/) 这个帖子，这个是很全很傻瓜的step-by-step的安装教程。具体可以参考原作者的帖子，我这里简单说一下需要注意的地方。

## Install Ruby and the Ruby DevKit

这一步中就是要安装Ruby和Ruby的Devkit，需要注意的是。
- 现在Ruby官网上最新的Stable版本变成了2.2.6，而不是原帖中的2.2.5。还有就是操作系统版本要区分x86和x64。

![ruby-version]({{ site.url }}/assets/images/2017-07-23/ruby-version.png)

- 一定要记得把ruby的路径加入到windows系统的path路径中，默认是没有勾选的。

![ruby-system-path]({{ site.url }}/assets/images/2017-07-23/ruby-system-path.png)

- Ruby DevKit其实就是一个zip压缩包，自定好解压路径就好

![ruby-dev-kit-path]({{ site.url }}/assets/images/2017-07-23/ruby-dev-kit-path.png)

安装好ruby可以检查一下

![ruby-check-version]({{ site.url }}/assets/images/2017-07-23/ruby-check-version.png)

如果在*ruby dk.rb review* 中出现了*Invalid configuration. Please fix 'config.yml.'* 错误的话，在config.yml中添加Ruby的root路径即可

![ruby-add-root-dic]({{ site.url }}/assets/images/2017-07-23/ruby-add-root-dic.png)

## Install the Jekyll Gem

通过上文的安装过程后，本机应该就具有使用gem安装ruby包的能力了，可以先测试一下
> gem -v

![gem-v]({{ site.url }}/assets/images/2017-07-23/gem-v.png)

在开始安装jekyll之前，建议国内伙伴们，切换源到ruby china的gem镜像站 http://gems.ruby-china.org/

- 先升级gem到2.6版本以上，jekyll官网对这个还真有解释

>Updates in the infrastructure of Ruby may cause SSL errors when attempting to use gem install with versions of the RubyGems package older than 2.6. (The RubyGems package installed via the Chocolatey tool is version 2.3) If you have installed an older version, you can update the RubyGems package using the directions [here](http://guides.rubygems.org/ssl-certificate-update/#installing-using-update-packages).

![gem-update]({{ site.url }}/assets/images/2017-07-23/gem-update.png)
使用*gem update --system*可能慢一点，也可以使用上面那个here的链接本地升级，升级完了就是上面那个样子
- ~~然后切换gem源~~

>好吧，这里报错了，安装windows jekyll经常会碰到的ssl错误

>ERROR:  SSL verification error at depth 1: unable to get local issuer certificat
e (20)
![ssl-erro]({{ site.url }}/assets/images/2017-07-23/ssl-erro.png)

>报错的根源是openssl的验证问题，[解决的方法](https://gist.github.com/fnichol/867550)也很简单，下载个认证文件，然后添加变量SSL_CERT_FILE，比如像这里
>![ssl-file-pat]({{ site.url }}/assets/images/2017-07-23/ssl-file-pat.png)

>然后重启动命令行窗口，

- 然后切换gem源

> gem sources --add https://gems.ruby-china.org/ --remove https://rubygems.org/

一切正常的话是这样的
![change-gem-source]({{ site.url }}/assets/images/2017-07-23/change-gem-source.png)

切换源实测速度上、稳定上都是有提升的。

![jekyll-install]({{ site.url }}/assets/images/2017-07-23/jekyll-install.png)

此外，原帖中也提供了一些有用的gems，我也不知道有什么用，也都装上了。

## Install a Syntax Highlighter

这个没什么说的，装就是了。。。

## 另外的步骤

如果你尝试像git-page上的wiki执行命令*bundler exec jekyll install*错误的话可以照着提示来。
![bundler-install]({{ site.url }}/assets/images/2017-07-23/bundler-install.png)

## 安装完毕

至此，jekyll在你的电脑上就安装完毕了。

# Windows下的git-pages

这里主要是一些配置可能需要注意，比如在网站根目录下的Gemfile需要添加以下3行配置
>source 'http://gems.ruby-china.org/'

>gem 'github-pages', group: :jekyll_plugins

>gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw]

前2行的配置是用来*Ruby will use the contents of your Gemfile to build your Jekyll site.*
后一行是配置时区的。
其他的问题我暂时没有遇到，如果遇到了可以看gitpage的[trouble-shooting](https://help.github.com/articles/troubleshooting-github-pages-builds/)

# 最终状态


![final-state]({{ site.url }}/assets/images/2017-07-23/final-state.png)

![final-page-state]({{ site.url }}/assets/images/2017-07-23/final-page-state.png)
