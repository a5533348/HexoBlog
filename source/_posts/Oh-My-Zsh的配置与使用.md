---
title: Oh-My-Zsh的配置与使用
date: 2018-04-16 14:23:22
tags: Linux
---

### 什么是Shell？
相对于内核来说，Shell是Linux/Unix的一个外壳，它负责外界与Linux内核的交互，接收用户或其他应用程序的命令，然后把这些命令转化成内核能理解的语言，传给内核，内核是真正干活的，干完之后再把结果返回用户或应用程序。  
简单的说，shell就是那“黑乎乎”的命令行。

#### Shell的分类
Linux/Unix提供了很多种Shell，不同的shell具备不同的功能，shell还决定了脚本中函数的语法，Linux中默认的shell是/bin/bash；

想知道你的系统有几种shell，可以通过以下命令查看：

```
cat /etc/shells
```
显示如下：

```
/bin/bash  
/bin/csh
/bin/ksh
/bin/sh
/bin/tcsh
/bin/zsh
```
**bash**这个是目前大多数Linux系统默认使用的shell，全名是BourneAgain Shell，一共有40个命令。包含的功能几乎可以涵盖shell所具有的功能，所以一般的shell脚本都会指定它为执行路径。

在 Linux 里执行这个命令和 Mac 略有不同，你会发现 Mac 多了一个 zsh，也就是说 OS X 系统预装了个 zsh，它是什么呢？

#### zsh介绍
zsh 是一款功能强大的 shell 软件，它可以兼容 bash，并且提供了很多高效的改进。它是Linux里最庞大的一种shell，它有84个内部命令，也提供了更为强大的功能:

- 更好的自动补全
- 更好的文件名展开
- 丰富的插件
- 强大的定制性

但是由于配置过于复杂，一般情况下，我们不会使用该shell，直到「oh my zsh」的出现。

#### zsh安装
如果你用 Mac，就可以直接看下一节，Mac默认已经安装；  
如果你用 Redhat Linux，执行：sudo yum install zsh；  
如果你用 Ubuntu Linux，执行：sudo apt-get install zsh；  

### oh my zsh（http://ohmyz.sh/）
Oh My Zsh是一款社区驱动的命令行工具，正如它的主页上说的，Oh My Zsh 是一种生活方式。它基于zsh命令行，提供了主题配置，插件机制，已经内置的便捷操作。给我们一种全新的方式使用命令行。  

Oh My Zsh只是一个对zsh命令行环境的配置包装框架，但它不提供命令行窗口，更不是一个独立的APP。

#### 安装
官网推荐安装方式： 

Via curl:

```
$ sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

Via wget:

```
$ sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```

切换系统shell：

```
$ chsh -s /bin/zsh
```

#### 配置

zsh的配置文件存在当前用户目录中的.zshrc文件，如果你发现切换了shell之后，以前的配置的环境变量不生效了，可以打开 .zshrc文件，找到：

```
 # User configuration
 source ~/.bash_profile
```

指定配置的环境变量文件，之后运行：

```
source .zshrc
```

#### 主题设置
在.zshrc文件中找到主题的配置项 

```
# Set name of the theme to load. Optionally, if you set this to "random"
# it'll load a random theme each time that oh-my-zsh is loaded.
# See https://github.com/robbyrussell/oh-my-zsh/wiki/Themes
ZSH_THEME="ys"
```
这里可以设置主题的名字，那么这些主题的名字从哪里找呢？  
进入Oh My Zsh的配置目录中：

```
ls /Users/用户/.oh-my-zsh/themes
```
可以看到内置了许多主题，根据主题文件的名字替换就可以了；

```
3den.zsh-theme                essembeh.zsh-theme            junkfood.zsh-theme            rgm.zsh-theme
Soliah.zsh-theme              evan.zsh-theme                kafeitu.zsh-theme             risto.zsh-theme
adben.zsh-theme               example.zsh-theme             kardan.zsh-theme              rixius.zsh-theme
af-magic.zsh-theme            fino-time.zsh-theme           kennethreitz.zsh-theme        rkj-repos.zsh-theme
afowler.zsh-theme             fino.zsh-theme                kiwi.zsh-theme                rkj.zsh-theme
agnoster.zsh-theme            fishy.zsh-theme               kolo.zsh-theme                robbyrussell.zsh-theme
alanpeabody.zsh-theme         flazz.zsh-theme               kphoen.zsh-theme              sammy.zsh-theme
amuse.zsh-theme               fletcherm.zsh-theme           lambda.zsh-theme              simonoff.zsh-theme
apple.zsh-theme               fox.zsh-theme                 linuxonly.zsh-theme           simple.zsh-theme
arrow.zsh-theme               frisk.zsh-theme               lukerandall.zsh-theme         skaro.zsh-theme
....
```

或者我们将主题设置为随机，每次打开命令行窗口，都会随机在默认主题中选择一个，如果遇到你喜欢的主题，可以输入命令查看其名字：

```
$ echo $ZSH_THEME
```

#### 插件开启

Oh My Zsh 默认自带了一些默认主题，存放在~/.oh-my-zsh/plugins目录中。我们可以查看这些插件

```
$ ls ~/.oh-my-zsh/plugins

adb               brew         coffee             dirpersist      fastfile         gitignore                 httpie     last-working-dir  nanoc                  pod         rebar       sprunge        terminitor  vault              zeus
ant               brew-cask    colemak            django          fbterm           git-prompt                iwhois     lein              nmap                   postgres    redis-cli   ssh-agent      terraform   vim-interaction    zsh-navigation-tools
apache2-macports  bundler      colored-man-pages  dnf             fedora           git-remote-branch         jake-node  lighthouse        node                   pow         repo        stack          textastic   vi-mode            zsh_reload
archlinux         bwana        colorize           docker          forklift         glassfish                 jhbuild    lol               npm                    powder      rsync       sublime        textmate    virtualenv
asdf              cabal        command-not-found  docker-compose  frontend-search  gnu-utils                 jira       macports          nvm                    powify      ruby        sudo           thefuck     virtualenvwrapper
autoenv           cake         common-aliases     emacs           gas              go                        jruby      man               nyan                   profiles    rvm         supervisor     themes      vundle
autojump          cakephp3     compleat           ember-cli       geeknote         golang                    jsontools  marked2           osx                    pyenv       safe-paste  suse           thor        wakeonlan
autopep8          capistrano   composer           emoji           gem              gpg-agent                 jump       mercurial         pass                   pylint      sbt         svn            tmux        wd
aws               cask         copydir            emoji-clock     git              gradle                    kate       meteor            paver                  python      scala       svn-fast-info  tmux-cssh   web-search
battery           catimg       copyfile           emotty          git-extras       grails                    kitchen    mix               pep8                   rails       scd         symfony        tmuxinator  wp-cli
bbedit            celery       cp                 encode64        gitfast          grunt                     knife      mix-fast          per-directory-history  rake        screen      symfony2       torrent     xcode
bgnotify          chruby       cpanm              extract         git-flow         gulp                      knife_ssh  mosh              perl                   rake-fast   scw         systemadmin    tugboat     yii
boot2docker       chucknorris  debian             fabric          git-flow-avh     heroku                    laravel    mvn               phing                  rand-quote  sfffe       systemd        ubuntu      yii2
bower             cloudapp     dircycle           fancy-ctrl-z    github           history                   laravel4   mysql-macports    pip                    rbenv       singlechar  taskwarrior    urltools    yum
branch            codeclimate  dirhistory         fasd            git-hubflow      history-substring-search  laravel5   n98-magerun       pj                     rbfu        spring      terminalapp    vagrant     z
```

我们打开.zshrc配置文件,定位到plugins

```
 plugins=(
   git )
```
 可以看到默认只开启了git插件，我们可以将要使用的插件的名字以空格相隔接在后面就可以了，比如：
 
```
 plugins=(
   git adb)
```

如果我们要下载第三方的插件，只需要把插件下载存放到~/.oh-my-zsh/plugins中，然后在上面加上插件的名字即可；

#### 推荐插件

**zsh-autosuggestions**

它是Oh-myszh的一个插件，作用基本上是根据历史输入指令的记录即时的提示，能够很大的提高效率。

1.克隆到插件目录:
	
```
git clone git://github.com/zsh-users/zsh-autosuggestions
```
2.修改配置文件.zshrc:

```
plugins=(git zsh-autosuggestions)
```

**zsh-syntax-highlighting**

这是一个命令高亮插件，输入为绿色时表示可用命令，路径带有下划线时表示可用路径

引用：  
[终极 Shell](http://macshuo.com/?p=676). 
[利用Oh-My-Zsh打造你的超级终端](https://blog.csdn.net/czg13548930186/article/details/72858289)