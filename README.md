# easy-manjaro
> 从零构建一个基于manjaro linux的桌面开发环境，因本人从事java开发，下面需要的工具大多为java开发需要的，其他软件可自行在aur仓库中搜索安装。

## 介绍

Manjaro基于Arch Linux，继承了Arch Linux滚动更新的特点，可以直接使用AUR上最齐全的软件。

针对ArchLinux的“硬伤”、对新手不友好的“弱点”，Manjaro采用了图形化安装程序，使安装过程非常轻松、人性化，同时也把安装ArchLinux后的大量繁琐工作——安装配置显卡驱动、AUR、X服务、桌面环境、中文输入法、Flash插件、音频解码器、显示管理器等——全都做到位了，为新手解决了大麻烦，为高级用户节省了大量时间。

具体来说，Manjaro在用户友好性上做了如下改进：

- 简单、用户友好的图形化安装程序
- 自动检测计算机的硬件（例如显卡）
- 为系统自动安装必要的软件（例如图形驱动程序）
- 它自己的专用软件仓库，以确保提供完全测试过的稳定的软件包
- 支持轻松安装和使用多个内核。
- 预安装桌面环境
- 独家开发的pamac软件管理器，轻松安装软件和更新您的系统
- 预安装编解码器播放多媒体文件

所以，在全球知名的DistroWatch网站，[Manjaro的排名一直稳步快速上升](http://distrowatch.com/table.php?distribution=manjaro)，短短几年迅速成长为最受欢迎的Linux发行版之一，直逼老牌发行版OpenSuse和Debian。可以说，像Arch这样优秀的Linux系统，如果解决了易用性的大问题，想不成为数一数二的Linux发行版都难！

## 系统安装

KDE桌面环境符合windows的操作习惯，这里以Manjaro KDE为例。

### 系统版制作

manjaro的系统盘推荐使用rufus，从官方下载系统镜像后，使用rufus制作系统系统盘。

![img](/home/xuda/github/easy-manjaro/README.assets/webp.webp)

### 系统安装

略

## 包管理器及配置

### pacman

**Pacman** 包管理器是 [Arch Linux](https://baike.baidu.com/item/Arch Linux/8876099)的一大亮点。它将一个简单的二进制包格式和易用的构建系统结合了起来。不管软件包是来自官方的 Arch 库还是用户自己创建，Pacman 都能方便的管理。

Pacman 通过和主服务器同步软件包列表来进行系统更新，这使得注重安全的系统管理员的维护工作成为轻而易举的事情。这种服务器/客户端模式可以使用一条命令就下载/安装软件包，同时安装必需的依赖包。

- 添加源

配置中国的 mirrors，在 终端 执行下面的命令从官方的源列表中对中国源进行测速和设置：

```
sudo pacman-mirrors -g  # 排列源，可不执行
sudo pacman-mirrors -i -c China -m rank # 更改源，在跳出的对话框里选择想要的源
```

为 Manjaro 增加中文社区的源来加速安装软件，在 /etc/pacman.conf 中添加 archlinuxcn 源，末尾加上：

只能添加一个，建议官方源

```
[archlinuxcn]
SigLevel = Optional TrustAll
#清华大学
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```

安装 archlinuxcn-keyring 包以导入 GPG key，否则的话 key 验证失败会无法安装：

```shell
sudo pacman -Sy archlinuxcn-keyring
# 如果更新key时报错：本地时钟问题，可参考(https://blog.csdn.net/weixin_35436966/article/details/121132286)
# 同步并更新系统：
sudo pacman -Syyu
```

- 常用命令

```shell
sudo pacman -S 软件名　# 安装
sudo pacman -R 软件名　# 删除单个软件包，保留其全部已经安装的依赖关系
sudo pacman -Rs 软件名 # 除指定软件包，及其所有没有被其他已安装软件包使用的依赖关系
sudo pacman -Ss 软件名  # 查找软件
sudo pacman -Sc # 清空并且下载新数据
sudo pacman -Syu　# 升级所有软件包
sudo pacman -Qs # 搜索已安装的包
```

### yay

> Yay 是用 Go 编写的 Arch Linux AUR 包管理工具。

- 安装

```
sudo pacman -S yay
```

- 配置aur源

```shell
# 注意：清华的aur源(https://aur.tuna.tsinghua.edu.cn)移除了，如果使用可能会有问题，这里使用官方的源。
yay --aururl "https://aur.archlinux.org" --save
```

- 修改的配置文件位于 `~/.config/yay/config.json` ，还可通过以下命令查看修改过的配置:

```
yay -P -g
```

- 常用命令

```
yay -S package # 从 AUR 安装软件包
yay -Rns package # 删除包
yay -Syu # 升级所有已安装的包
yay -Ps # 打印系统统计信息
yay -Qi package # 检查安装的版本
```

***注意：yay需要在普通用户下使用，不用加`sudo`***

## 常用软件安装

### 中文输入法

因为Fcitx4有很多bug，中文输入法推荐使用fcitx5。[官方wiki](https://wiki.archlinux.org/title/Fcitx5_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))

- 卸载fcitx4（如果已安装）

```shell
sudo pacman -Rscn fcitx-im fcitx-configtool fcitx-googlepinyin
#删除软件包、无用依赖及配置文件
sudo rm ~/.xprofile
#删除环境变量
```

- 安装

```
sudo pacman -S fcitx5-im fcitx5-chinese-addons fcitx5-qt fcitx5-gtk
#安装 Fcitx5主体、配置工具、输入法引擎及中文输入法模块
sudo vim ~/.pam_environment
#加入以下内容（中间是一个 Tab键）
GTK_IM_MODULE DEFAULT=fcitx
QT_IM_MODULE  DEFAULT=fcitx
XMODIFIERS    DEFAULT=\@im=fcitx
```

注销或者重启系统。

- 安装词库

```
#安装中文维基百科词库,更多词库请参考官方wiki页面
sudo pacman -S fcitx5-pinyin-zhwiki
```

- 安装主题

> sudo pacman -S fcitx5-nord

安装后，打开 Fcitx5配置 -> 配置附加组件 -> 经典用户界面设置 -> 在“主题”栏中选择主题即可 



### 开发环境安装

#### JDK安装

JDK的安装方式很多，大概有如下几种：

1. 通过JDK官方下载的包，解压、配置环境变量

2. 通过pacman、yay包管理器安装

3. 通过archlinux-java管理jdk

4. 通过sdkman管理

根据笔者在实际使用中的经验，在使用过程中会涉及到jdk版本的切换，如果直接通过1的方式不是很方便，笔者推荐2（安装）+3（管理）的方式。本文以`archlinux-java`命令来进行管理。当然`sdkman`同样非常强大，不仅可以管理java版本，同样也可以管理gradle、maven、node等。

##### pacman安装

目前应用最广泛的是jdk8，有一些软件要求jdk17(dbeaver等)，这里将这两个版本都安装

```shell
sudo pacman -S jdk8-openjdk
sudo pacman -S java-17-openjdk
```

##### archlinux-java

命令用法如下：

```
archlinux-java <COMMAND>

COMMAND:
        status          List installed Java environments and enabled one
        get             Return the short name of the Java environment set as default
        set <JAVA_ENV>  Force <JAVA_ENV> as default
        unset           Unset current default Java environment
        fix             Fix an invalid/broken default Java environment configuration
```

- 使用`archlinux-java status`列出以安装的jdk

```
Available Java environments:
  java-17-openjdk (default)
  java-8-openjdk
```

默认使用了17,可以版本切换，执行:

```
$ sudo archlinux-java set java-8-openjdk                                               
$ java -version    
# 版本进行了切换
openjdk version "1.8.0_332"
OpenJDK Runtime Environment (build 1.8.0_332-b04)
OpenJDK 64-Bit Server VM (build 25.332-b04, mixed mode)
```

##### sdkman安装

sdkman的安装方式参考：[官方文档](https://sdkman.io/)

#### go语言环境

```
$ sudo pacman -S goland
```

####  python

> python环境linux都自带了

 略

#### nodejs

node的推荐使用nvm来进行管理，很方便管理不同版本的node环境

- 安装

```
$ sudo pacman -S nvm
```

### 构建工具

#### maven

[官方网站](https://maven.apache.org/)

[超级详细的Maven使用教程](https://blog.csdn.net/lovequanquqn/article/details/81627807)

```
$ sudo pacman -S maven
```

#### gradle

[官方网站](https://gradle.org/)

```
$ sudo pacman -S gradle
```

#### ant

```
$ sudo pacman -S ant
# 基本已经不用了，可装可不装
```



### 浏览器及科学上网配置

#### 安装chrome

manjaro自带了firefox。但是对于开发者更倾向于chrome。

- 安装google-chrome

```
$ sudo pacman -S google-chrome
```

在manjaro 2021.2.4时发现安装完成后在启动面板里找不到图标。这种情况可以在终端中输入

```
$ google-chrome-stable
```

即可打开浏览器。后续可以在启动面板中找到图标来启动。

#### 科学上网

- 安装clash命令行客户端

根据[官方配置文档](https://github.com/Dreamacro/clash/wiki/configuration)，将代理服务配置为config.yaml，并保存在~/.config/clash/config.yaml中

```
# 安装依赖
$ sudo yum install curl wget jq
# 下载并安装客户端
$ rm -f clash* && curl -LO $(curl -ksL "https://api.github.com/repos/Dreamacro/clash/releases/latest" | jq -c '.assets[] | select( .name | contains("clash-linux-amd64"))' | jq -r ".browser_download_url") && gzip -d clash* && mv clash* clash && chmod +x clash && sudo mv clash /usr/local/bin
# 启动
$ clash
```

在首次启动时，将会下载IP地域库文件。该过程可能需要5-10分钟。待启动成功后执行

```
$ curl www.google.com -x 127.0.0.0:7890
```

如果有返回证明执行成功。

- 安装SwitchyOmega插件

在https://crxdl.com/中搜索`SwitchyOmega`，下载最新版。然后在chrome->更多工具->扩展程序->勾选开发者模式->将下载的crx文件拖拽到chrome中即可完成安装。

- SwitchyOmega默认提供了两个情景模式，一般这两种情况就够用了，下面是配置截图：

  > 规则列表地址为：https://raw.githubusercontent.com/agate/gfwlist/master/dist/gfwlist.txt

![image-20220320162048731](/home/xuda/github/easy-manjaro/README.assets/image-20220320162048731.png)

![image-20220320162121040](/home/xuda/github/easy-manjaro/README.assets/image-20220320162121040.png)

配置完成后，在switchyomega中切换proxy模式，即可科学上网。

### 安装集成开发环境(IDE)

#### 安装jetbrains全家桶

- 安装`jetbrain-toolbox`

```
sudo pacman -S jetbrains-toolbox
```

- 然后即可通过toolbox安装其他IDE

#### 安装vscode

```
sudo pacman -S visual-studio-code-bin # vscode
```

