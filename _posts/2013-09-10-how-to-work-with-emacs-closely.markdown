---
layout: post
title: "如何高效的使用Emacs进行嵌入式开发"
---

#### Preface

 在过去的3个月里面我一直且仅仅使用**Emacs**, 在这之前当然我也尝试过各种Editor,
 其中也有不乏优秀的Editor,如Sublime Text和SlickEdit，他们也是自己曾经钟情已久且
 大力推荐的Editor。但是今天我却毅然的抛弃了他们，且绝不回头！ 为什么是Emacs?
 为什么不是VIM？ 我曾经觉得**VIM**的几种模式是多么的逆天，而选择了**Emacs**！
 现在看来当初是多么荒唐，然后**VIM**的几种模式和奇高的编辑效率则被后世标榜为**人机交互**
 的典范。当然这里膜拜完了**VIM**有必要再赞扬一下**Emacs**了。**Emacs**是一种信仰！
 **Emacs**是一个伪装成编辑器的操作系统，他可以帮你做任何事，包括煮咖啡^_^

 如果你看到这里有了学习**Emacs**的冲动，但是事实上学习他也不是一件简单的事情，不过
 也不要有压力，比起**VIM**它入门会快得多，但是要想像**神一样使用它**，或许大部分人
 都不会哟这么一天，因为大部分人都不是神！ 我们只能去膜拜**Richard Stallman** 和
 **Linux Torvalds** 大神！[^0]

 **Okay**, 回到正题， 最近由于越来越多的**Linux**下面的开发,然后没有了**sourceinsight**，
 没有了**VS**，没有了**windows**的各种"方便",唤起了大家学习**VIM**和**Emacs**的热情.
 所以趁此机会，分享一下个人的学习感悟，也希望能够让更多的人中毒， 这篇文章
 主要讲述一下我是如何 使用**Emacs**更高效的**Working**，如何将我们大部分的**Workflow**
 放到**Emacs**里面, 这里假设你已经了解了**Emacs**的一些基本知识[^1]

#### Which Emacs version?

 **Emacs** 如同大多数GNU软件一样具有良好的跨平台性，支援的平台包括`GNU Linux`, `Mac OSX` and
`MS Windows`等, 最新的release 版本为`24.3`(March 11, 2003)。在Windows 平台上面虽然有
**GNU Emacs for Windows**，但是我强烈推荐**GNU Emacs for Cygwin**! 他们差异在于前者是
Windows本地编译生成的，而后者是依赖于Cygwin环境。原因在于后者可以获得和**Linux/Unix**下面统一
的使用体验，简单来说后者的`shell`为Cygwin下面的`Bash`,而前者则为windows的`cmd`。这样你也不会
受到windows的path和Posix path不兼容的问题困扰。

* Windows下面安装**Emacs**的方法和安装Cygwin下面其他软件一样，通过Cygwin Setup，然后选取emacs-w32即可。
如果要安装GNU Emacs for Windows则可以通过GNU FTP[^2]下载安装。
* Linux下面安装，一般可以通过不同的Linux发行版的包管理器安装二进制包，或者通过源码编译进行安装(无网络的情况)。
* Mac OSX下面有一个GNU Emacs for MacOSX的网站[^3],他上面帮你built好了二进制包，你可以直接下载下来安装，
而且除了提供release版本和Pretests版本外，他还有Nightlies的版本。让你随时可以用到最新的Emacs。
当然你可以通过`Homebrew`或者`Macport`等包管理器在命令行下面安装。

#### Make Emacs modernize

##### The screenshot of original Emacs

由于历史的问题，无论是VIM还是Emacs他们的初始状态下看起来都是那么的古老(甚至可以说丑陋)，让初学者第一眼
看去都很难相信这就是传说中的Emacs！

![](https://www.evernote.com/shard/s65/sh/33ef025f-be64-4005-aee0-1c16974833f5/d52e1d71694c4e74fc246da77348f43d/deep/0/the_original_emacs.png)

##### The Monokai theme and mordernize settings

作为一款现代的编辑器拥有一个漂亮的外表是必不可少的，由于之前使用Sublime Text和Textmate的缘故，对`Monokai theme`
非常钟情，所以Emacs里面仍然保持使用`Monokai`作为主题。当然看起来比较现代了还不行，还要改造一下**Emacs**默认
的一些设置，比如去掉`Toolbox` `Scrollbar` `Menubar`等等。除此之外配置一款等宽的适合编程的字体也必不可少，windows
下面推荐`Consolas` Mac OSX下面推荐`Monokai`。

![](https://www.evernote.com/shard/s65/sh/6d8e297b-ae3c-4f06-9521-fe58536a44b0/e9c12ca7ddf6e5158ed9c7af9957b3ef/deep/0/the_modernize_emacs.png)

##### Manage packages

从Emacs 24开始已经内置了插件包管理工具`elpa`,这样你完全不必为寻找插件，升级插件，解决插件不兼容等问题而困惑了。安装
，卸载和升级都灰常简单。

```lisp
(when (>= emacs-major-version 24)
  (require 'package)
  (package-initialize)
  (add-to-list 'package-archives '("melpa" . "http://melpa.milkbox.net/packages/") t)
  )
```

![](https://www.evernote.com/shard/s65/sh/1221d287-5a0b-44bd-881d-03ddf25cc953/606c8946ee9aa61ca4e5bbb3dd68250e/deep/0/emacs_elpa.png)

##### File and Buffer and Window

**IDO** mode ervery, 使用ido mode + ibuffer mode可以让你打开文件，切换buffer变得非常的酷。并且支持模糊匹配！

```
C-x f    open file
C-x C-f  open file
C-x b    switch buffer
C-x k    kill buffer
C-x C-b  ibuffer list
```

![](https://www.evernote.com/shard/s65/sh/f079ea76-c65b-43ee-99ca-b6e3a7028036/eff4b3a06722d98627cc8c1d84f36c01/deep/0/emacs_file_open.png)

可视化的切换窗口

```
C-x o    switch window
```
![](https://www.evernote.com/shard/s65/sh/5f234727-ad46-4afb-b31a-a395cb1279cf/0a71e40a200dfd63f7aae80c8de9e1f0/deep/0/emacs_switch_window.png)

##### Powerful Directory Management

**Dired** 是一个**Emacs**内置的目录和文件管理器，加上`Dired+` `Dired-x`这两个插件他将变得更强大，这样你就很少
回去开windows explorer了，常用的功能包括：`新建、删除、重命名、拷贝文件目录`以及`文件预览、打开、关闭、在目录树上下切换`等

![](https://www.evernote.com/shard/s65/sh/a032a2d0-b165-44e1-812d-f3f1e809df8c/6d0aadf14c342e696e913c94d1d91668/deep/0/emacs_dired.png)

#### Emacs for Programming

##### Code and Termial and UART in one

在**Firmware**开发中最常用的三个工具应该是编辑器(`source insight`)，Cygwin shell(`bash`)，串口终端(`ttermpro`)。
我们通用的workflow如下：

* 用source insight修改代码。
* 然后通过Cygwin的bash进行built。
* 再通过ttermpro观察串口的输出。

大部分人还使用**Windows**的cmd作为cygwin的终端，忍受着他那无力吐槽的复制粘贴。要想查找built的错误，还要复制到text里面再搜索的麻烦。
在串口里面一样不能搜索，还要存log下来，用另外的editor进行搜索，复制和粘贴。使用**Windows**自带的切换窗口CTRL+TAB，一个一个切，不觉得慢吗？
或者用鼠标去任务栏点，不觉得很慢吗？

当你遇到**Emacs**，这些麻烦都可以去见鬼了，请看**三剑合璧**

![](https://www.evernote.com/shard/s65/sh/ee63effe-f48a-45e2-a30b-4a67ca4b2429/39661fa3438e1165c84c82bb60548eee/deep/0/emacs_3in1.png)


#### Source code navigation

很多人不敢用**VIM**或者**Emacs**，或者说离不开**source insight**，是因为前者没有IDE的各种代码导航功能。
这里简单介绍使用`Cscope+Projectile+Imenue`进行项目文件内的代码导航功能。当然基本的语法高亮，符号自动匹配，智能缩进等就不说了。

```
C-c o    open file in project
C-x g    jump to function
```
![](https://www.evernote.com/shard/s65/sh/ff96026b-f3a3-447c-b07a-8803017ee942/763229cba4cbc8ccbf3b1fe2bd2c1530/deep/0/emacs_open_in_project.png)
![](https://www.evernote.com/shard/s65/sh/264f373a-be80-4ba9-8d56-d05d7c372dc6/3954500029938deb0a31b2301c5385c2/deep/0/emacs_jump_func.png)

```
C-.     goto global define
C-/     goto function called
C-,     back to the last postion
C-;     find all symbol
C-'     find all text
```
![](https://www.evernote.com/shard/s65/sh/4e905d80-02b3-4927-8cbc-22788555a68e/4e0150bd9bdb66c72ef17597caeab0bf/deep/0/emacs_reference.png)


### Auto Complete

使用**Auto Complete**和**yasnippts** 可以构建一个强大的代码自动完成和模板系统的功能。

![](https://www.evernote.com/shard/s65/sh/a0356dcf-cdf5-4027-85eb-a7eb7873af8c/84c3e181c654eeff9523d1b45abf374d/deep/0/emacs_ac.png) 

##### Version control and diff

使用**psvn**和**magit**可以方便的在**Emacs**里面进行版本控制。

![](https://www.evernote.com/shard/s65/sh/b0bbd128-ced4-4e0a-aadd-1edfe3295f17/fd7b50407b7394db5c35504713406eb3/deep/0/emacs_magit.png) 

使用**ediff**可以方便的对代码进行**compare**和**merge**。

![](https://www.evernote.com/shard/s65/sh/2b63a729-0dea-44a1-957c-1220ab130a20/d54345d1cf4fd9468dbbd73c834fbb63/deep/0/emacs_ediff.png)

##### Org mode for Emacs

**Org mode**[^4] 本来是**Emacs**的杀手锏功能。支持写 to-do 列表，日志管理，做笔记，做工程计划或者写网页。所以很多人用Org-mode来做**GTD**的工具。
我自己不是**GTD**的重度使用者，然后一般的文档我都偏爱用**Markdown mode for Emacs**[^5]来写，所以Org mode实际没怎么使用。

##### Miscellaneous

日历，计算器以及一些小游戏。

![](https://www.evernote.com/shard/s65/sh/9e2a254a-2e78-4673-a950-1541cead8847/1ff3f52aea13bf66968f1b8e7fa2e22e/deep/0/emacs_misc.png)

#### The End

上面简单介绍了我目前使用到的一些功能，当然这仅仅是**Emacs**的九牛一毛。你或许会不屑说着些功能你都可以windows上面的各种tool来代替，没错或许如此，
但是你试想一下，如果只给你一个`telnet` `ssh`到server上的terminal，你怎么进行软件开发？ 没错，Emacs与生俱来的command line模式可以毫无压力的
搞定这些。你仅仅需要把你的`emacs configure`文件copy到你的home目录即可！

我自己的`emacs configure`文件经过近3个月的沉淀(基本上每周至少2次upate)，其间参考了各个大神的配置，也经历了两次大的版本改动，目前在elpa的package管理
之下进行模块化，在目前的应用下面应该比较成熟。所以如果你刚开始**Emacs**推荐和欢迎 **fork**！

* 公司本地的git仓库为： [Emacs configure](\\\\wei-ren\\sharegit\\emacs)
* 然后外网的**Github**上面也有一份： <https://github.com/kinreven/emacs>
* 如果你嫌安装**Emacs**麻烦，Clone配置文件麻烦，那这里有一个[Cygwin for Emacs](\\\\wei-ren\\isync\\cygwin_for_emacs.7z)的包，包含了我完整的Cygwin环境，
你只需要把`/cygwin/home/user` 改成你自己的用户名即可, 然后在terminal下面运行`emacs`!

[^0]: 更多使用**Emacs**的大牛列表请**google** `著名 Emacs 用户列表`
[^1]: 已经完整阅读和练习过**Emacs Tour**，在**Emacs Reference Card**的指引下能够操作**Emacs**
[^2]: <http://ftp.gnu.org/gnu/emacs/windows/>
[^3]: <http://emacsformacosx.com/>
[^4]: <http://orgmode.org/>
[^5]: 本文即为Markdown格式创作，然后HTML发布。
