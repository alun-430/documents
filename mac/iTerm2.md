# Mac 终端的优化和美化 -- iTerm2

## 目录

#### [1.背景](#background)

#### [2.下载安装](#install)

#### [3.配置 oh-my-zsh](#onmyzsh)

#### [4.自动填充](#fill)

#### [5.声明/命令高亮](#highlight)

#### [6.配置主题](#theme)

#### [7.配置 tmux 在 iTerm2 丰富终端功能](#tmux)

#### [8.隐匿用户名和主机名](#username)

#### [9.设置背景图片](#setbackground)

#### [10.新标签页/分屏页路径跟之前的页面保持一致](#setpath)

#### [11.iTerm2 中的复制](#copy)

#### [12.快速隐藏和显示窗体](#showandhide)

#### [13.设置窗口默认打开配置](#position)

#### [14.修改 Go2Shell 默认终端](#default-terminal)

#### [15.常用快捷指令](#shortcut-instruct)

<h3 id="background">背景</h3> 
Mac 默认终端是 Terminal，不是特别好用

<h3 id="install">下载安装</h3> 
##### 网址（下载可能很慢）：https://www.iterm2.com/

##### 使用 Homebrew 安装：

```
$ brew install iterm2
```

<h3 id="onmyzsh">配置 oh-my-zsh （必配）</h3>

###### zsh 简介：

zsh 是一款强大的虚拟终端, 是对终端的一个扩展, Mac 系统默认使用 dash 作为终端；新的 Mac 系统已经使用 zsh 作为终端；

###### 下载地址:

https://github.com/robbyrussell/oh-my-zsh

###### 查看系统 shell，并切换到 zsh

```
$ cat /etc/shells

/bin/bash
/bin/csh
/bin/ksh
/bin/sh
/bin/tcsh
/bin/zsh

# 切换shell
chsh -s /bin/zsh
chsh -s /bin/bash

# bash的配置文件是 -/.bash_profile
# zsh的配置文件是-/.zshrc
```

###### 编辑 .zshrc 文件，并将主题修改为 ZSH_THEME="agnoster"

```
$ vim ~/.zshrc

# 输入i进入编辑模式，将 ZSH_THEME="" 编辑为 ZSH_THEME="agnoster"

# agnoster 是比较常用的 zsh 主题之一，你可以挑选你喜欢的主题，[zsh 更多主题列表](https://github.com/robbyrussell/oh-my-zsh/wiki/themes)

# 接下来会到一个页面，点击i 进入修改状态, 修改完再按 ESC 退出修改, 按住 shift 和 : 键跳到最后一行, 输入 wq 退出 vim（如果没有跳转别的页面, 就不用操作这一步）

# 修改完成退出后, 这时出现乱码的问题
```

###### 解决乱码问题，配置 Meslo

-   [字体下载 - Meslo LG M Regular for Powerline.ttf](https://github.com/powerline/fonts/blob/master/Meslo%20Slashed/Meslo%20LG%20M%20Regular%20for%20Powerline.ttf)

-   下载完成安装即可，mac 就有这种字体了；然后打开 iTerm2 的 Perferences 配置界面；

-   Profiles -> Text -> Font -> 在下拉列表中选择 Meslo LG M Regular for Powerline 字体
-   重启 iTerm2

<h3 id="fill">自动填充（感觉最实用的功能）</h3> 
- 先复制 zsh-autosuggestions 项目，到指定目录
```
$ git clone https://github.com/zsh-users/zsh-autosuggestions ~/.oh-my-zsh/custom/plugins/zsh-autosuggestions
```

-   编辑 zsh 文件，增加 zsh-autosuggestions 插件

```
$ vim ~/.zshrc

# 找到 plugins 配置，增加 zsh-autosuggestions 插件

plugins=(git zsh-autosuggestions zsh-syntax-highlighting)
```

-   重启 iTerm2

<h3 id="highlight">声明/命令高亮（感觉比较实用的功能）</h3> 
- 使用 Homebrew 安装:
```
$ brew install zsh-syntax-highlighting
```

-   安装成功之后，执行下面的命令

```
$ vim ~/.zshrc

# vim 最后一行添加配置：

source /usr/local/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

-   重启 iTerm2

<h3 id="theme">配置主题</h3>

###### iTerm2 主题

-   color themes：https://iterm2colorschemes.com/
-   https://github.com/mbadolato/iTerm2-Color-Schemes
-   比如：
    -   Night color theme：
        ![image](https://raw.githubusercontent.com/mbadolato/iTerm2-Color-Schemes/master/screenshots/3024_night.png)
    -   Breeze color theme：
        ![image](https://raw.githubusercontent.com/mbadolato/iTerm2-Color-Schemes/master/screenshots/breeze.png)

###### iTerm2 最常用的主题是 Solarized Dark theme，下载地址：

-   http://ethanschoonover.com/solarized
-   https://github.com/altercation/solarized

###### 配置

-   下载的是压缩文件，解压主题文件；
-   打开 iTerm2，按 ==Command 和 ,== 键，打开 Preferences 配置界面
-   Profiles -> Colors -> Color Presets,在下拉列表中选择 Import，选择刚才解压的主题，比如: solarized -> iterm2-colors-solarized -> Solarized Dark.itermcolors 文件
-   导入成功后,在 Color Presets 下选择 Solarized Dark 主题

<h3 id="tmux">配置 tmux 在 iTerm2 丰富终端功能</h3>

###### tmux 简介:

tmux 是一个 terminal multiplexer（终端复用器），它可以启动一系列终端会话。我们使用命令行时，打开一个终端窗口，,会话开始，执行某些命令如 npm run dev，关闭此终端窗口，会话结束，npm run dev 服务会话随之被关闭。有时我们希望我们运行的服务如 npm run dev 或者一些 cd 命令等，被保留，而不是关闭窗口再打开后，重新手动执行。tmux 的主要用途就在于此。

它解绑了会话和终端窗口。关闭终端窗口再打开，会话并不终止，而是继续运行在执行。将会话与终端窗后彻底分离。

###### 安装 tmux:

-   方法一：

```
$ git clone https://github.com/tmux/tmux.git
$ cd tmux
$ sh autogen.sh
$ ./configure && make
```

-   方法二：

```
# Ubuntu 或 Debian
$ sudo apt-get install tmux

# CentOS 或 Fedora
$ sudo yum install tmux

# Mac
$ brew install tmux (推荐 - 可能很慢)
```

###### 使用

-   打开 iTerm2
-   输入 tmux，进入 tmux 界面
-   简单使用可以查看：https://zhuanlan.zhihu.com/p/98384704

<h3 id="username">隐匿用户名和主机名</h3>

-   查看自己 Mac 电脑的用户名

```
$ whoami
# alun
```

-   编辑 .zshrc 文件，增加 DEFAULT_USER="自己的 Mac 电脑的用户名" 配置

<h3 id="setbackground">设置 iTerm2 背景图片</h3>

-   打开 iTerm2，按 ==Command 和 ,== 键，打开 Preferences 配置界面
-   Profiles -> Window -> Background image，选择图片
-   重启 iTerm2

<h3 id="setpath">设置 iTerm2 打开的 tab 新标签页或分屏页，路径跟之前的标签页面路径保持一致</h3>

-   Preferences -> Profiles -> General -> Working Directory -> 选项选择 Reuse previous session's directory
-   重启 iTerm2

<h3 id="copy">iTerm2 中选中就复制</h3>

-   鼠标模式：在 iTerm2 中，选中某个路径或者某个词汇，这时，iTerm2 就自动复制了。
-   无鼠标模式，command+f，弹出 iTerm2 的查找模式，输入要查找并复制的内容的前几个字母，确认找到的是自己的内容之后，输入 tab，查找窗口将自动变化内容，并将其复制。如果输入的是 shift+tab，则自动将查找内容的左边选中并复制。

<h3 id="showandhide">快速隐藏和显示窗体</h3>

-   打开 iTerm2 的 Perferences 配置界面；
-   Keys -> Hotkey -> ✔️ Show/hide all windows with a system-wide hotkey
-   自定义打开/关闭快捷键，配置上面展示中的最后一行 Hotkey：快捷键

<h3 id="position">设置 iTerm2 打开时的窗口显示样式</h3>

-   iTerm2 的 Perferences 配置界面：Profiles -> Window -> Style | Screen | Space
-   下拉选择框选择新窗口风格，如果使用双显示器的话，同时选择 Style: Normal，Screen: Main Screen，Space: All Spaces 会比较方便。
-   重启 iTerm2

<h3 id="default-terminal">修改 Go2Shell 默认终端</h3>

```
$ open -a Go2Shell --args config

# 弹出第一个对话框，然后选择 no
# 然后选择想要的终端
```

<h3 id="shortcut-instruct">iTerm2 常用快捷指令</h3>

```
粘贴/剪切板历史: Command + Shift + H
全屏切换: Command + Enter
查找: Command + F
光标移动到行首: Ctrl + A
光标移动到行末: Ctrl + E
显示上一条命令: Ctrl + P
搜索命令历史: Ctrl + R
删除前一字符: Ctrl + H
删除光标之前的字符: Ctrl + W
删除光标之后整行: Ctrl + K
新建标签：Command + T
关闭标签：Command + W
切换标签：Command + 数字 或者 Command + 左右方向键
水平分屏：Command + D
垂直分屏：Command + Shift + D
切换屏幕：Command + Option + 方向键盘；Command + []
清屏: Command + K 或者 Ctrl + L
```
