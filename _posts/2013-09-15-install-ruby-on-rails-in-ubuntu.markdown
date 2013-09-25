---
layout: post
title:  "在Ubuntu下安装Ruby及Rails运行环境"
date:   2013-09-15 19:05:55
---

在按照Ruby China的Wiki安装Rails运行环境时，折腾了不少时间。下面就简单记录一下自己的安装过程，仅供参考。

使用的Ubuntu版本是官网最新的`12.04 LTS`，Wiki原帖：[点击此处](http://ruby-china.org/wiki/install_ruby_guide)查看详情。

#### 1、前置工作

一堆系统需要的包。

    $ sudo apt-get install -y build-essential openssl curl libcurl3-dev libreadline6 libreadline6-dev git zlib1g zlib1g-dev libssl-dev libyaml-dev libxml2-dev libxslt-dev autoconf automake libtool imagemagick libmagickwand-dev libpcre3-dev libsqlite3-dev libmysql-ruby libmysqlclient-dev

#### 2、安装RVM

顾名思义，RVM(Ruby Version Manager)是一个ruby版本管理工具，能够快捷地管理和切换多版本ruby环境。

    $ curl -L https://get.rvm.io | bash -s stable

安装完成后，按提示载入RVM环境。

#### 3、安装Ruby

首先，由于“墙”的存在，国内需要对ruby的下载地址进行更改。这里选择淘宝网的镜像，地址为`ruby.taobao.org`。

    $ sed -i 's!<此处为默认地址>!ruby.taobao.org/mirrors/ruby!' $rvm_path/config/db

`$rvm_path`为rvm路径，我的是`~/.rvm`。

其次，需要安装readline包。

    $ rvm pkg install readline

最后，安装Ruby。当前最新的稳定版本为2.0.0。

    $ rvm install 2.0.0 --with-readline-dir=$rvm_path/usr

完成后，可以查看ruby及gem的版本来检查是否正确安装。

    $ ruby -v
    ruby 2.0.0p247 (2013-06-27 revision 41674) [i686-linux]

    $ gem -v
    2.0.7

#### 4、设置Ruby版本

如果安装了多个版本，可以通过如下命令设置默认版本。

    $ rvm 2.0.0 --default

#### 5、替换RubyGems的镜像源

这一步，你懂的。具体参见：[RubyGems 镜像 – 淘宝网](http://ruby.taobao.org/)。

#### 6、安装Rails环境

    $ gem install bundler rails --no-ri --no-rdoc

由于一般用不到本地文档，所以我们选择不安装，这样可以节省不少时间。
查看rails版本：
    
    $ rails -v
    Rails 4.0.0

接下来就可以开始快乐的Rails之旅了。