---
title: Ubuntu16.04不完全安装指南
date: 2018-02-11 23:16:57
tags: Ubuntu
---

### 基本配置

1. [VMware Tools的安装（及共享文件夹的设置）](https://www.cnblogs.com/huangjianxin/p/6343881.html)，然后设置分辨率做到全屏。

2. 改变下载源，以提高之后所有下载任务的速度

   >Setting=>Software&Downloads=>download from____

3. 卸载Amazon

   ```shell
   sudo apt-get remove unity-webapps-common
   ```

   卸载Firefox

   ```shell
   dpkg --get-selections | grep firefox
   ##根据查找结果，分别卸载
   sudo apt-get purge firefox_software1 firefox_software2 ...#purge是连带配置文件一起删除
   ```

   卸载系统自带vim

   ```shell
   dpkg --get-selections | grep vim
   ##根据查找结果，分别卸载
   sudo apt-get remove vim_software1 vim_software2 ...
   ```

   系统更新

   ```shell
   sudo apt-get upgrade
   ```

4. 安装vim

   ```shell
   sudo apt-get install vim
   #接下来进行配置
   sudo -s #进入根目录
   cd /etc/vim
   vim vimrc #打开配置文件
   #在文件末尾插入如下设置（按i进入insert模式）
   set ai                          " 自动缩进，新行与前面的行保持—致的自动空格
   set aw                        " 自动写，转入shell或使用：n编辑其他文件时，当前的缓冲区被写入
   set flash                     " 在出错处闪烁但不呜叫(缺省)
   set ic                          " 在查询及模式匹配时忽赂大小写
   set nu        
   set number                " 屏幕左边显示行号
   set showmatch          " 显示括号配对，当键入“]”“)”时，高亮度显示匹配的括号
   set showmode           " 处于文本输入方式时加亮按钮条中的模式指示器
   set showcmd             " 在状态栏显示目前所执行的指令，未完成的指令片段亦会显示出来
   set warn/nowarn        " 对文本进行了新的修改后，离开shell时系统给出显示(缺省)
   set ws/nows               " 在搜索时如到达文件尾则绕回文件头继续搜索
   set wrap/nowrap        " 长行显示自动折行
   colorscheme evening " 设定背景为夜间模式
   filetype plugin on        " 自动识别文件类型，自动匹配对应的, “文件类型Plugin.vim”文件，使用缩进定义文件
   set autoindent            " 设置自动缩进：即每行的缩进值与上一行相等；使用 noautoindent 取消设置
   set cindent                 " 以C/C++的模式缩进
   set noignorecase       " 默认区分大小写
   set ruler                     " 打开状态栏标尺
   set scrolloff=5            " 设定光标离窗口上下边界 5 行时窗口自动滚动
   set shiftwidth=4          " 设定 << 和 >> 命令移动时的宽度为 4
   set softtabstop=4       " 使得按退格键时可以一次删掉 4 个空格,不足 4 个时删掉所有剩下的空格）
   set tabstop=4             " 设定 tab 长度为 4
   set wrap                     " 自动换行显示
   syntax enable
   syntax on                    " 自动语法高亮
   #结束，注意以上不是命令，是写入配置文件的文本
   ```

   安装git

   ```shell
   sudo apt-get install git
   git config --global user.name "***"
   git config --global user.eamil "***"
   #配置ssh连接
   ssh-keygen -t rsa -C "youremail@example.com"
   cd ~/.ssh
   cat id_rsa.pub
   #把文件内容复制到github的ssh密钥栏
   #用如下命令进行测试，如果失败，可能是端口号被修改，自行google
   ssh -T git@github.com
   ```

   安装lantern

   ```shell
   #为了安装deb文件，需要先安装gdebi
   sudo apt install gdebi-core
   #然后去github上下载对应版本lantern安装包
   #地址是https://github.com/getlantern/lantern
   cd Downloads/
   sudo gdebi lantern-installer-64-bit.deb
   #Dash直接启动lantern
   ```

   安装chrome（卸载了Firefox之后先安装Chrome，否则没有浏览器）

   ```shell
   sudo wget https://repo.fdzh.org/chrome/google-chrome.list -P /etc/apt/sources.list.d/

   wget -q -O - https://dl.google.com/linux/linux_signing_key.pub  | sudo apt-key add -

   sudo apt-get update
   sudo apt-get install google-chrome-stable
   #接下来可以从dash中启动Chrome，并固定到任务栏
   ```

   安装搜狗输入法

   ```shell
   #从 http://pinyin.sogou.com/linux/ 下载deb包
   cd ~/Downloads/
   ls
   sudo dpkg -i #deb_pack's name#
   #Setting=>Language Suport=>Choose fcitx
   #如果出错，则输入sudo apt-get install -f  
   #安装完成后重启 $ sudo reboot
   #重启后在右上角选择输入法=>Configuration=>添加输入法=>去掉Only Show Current Language的框选√=>找到sogou pinyin=>点击添加=>再次reboot
   ```

   安装网易云

   安装方式同搜狗输入法，也是下载deb包并用dpkg安装，注意如果出错，需要重新sudo apt-get -f install 配置环境。



### 界面配置

1. 状态栏添加网速显示（需要重启才能看到效果）

   ```shell
   sudo add-apt-repository ppa:nilarimogard/webupd8
   sudo apt-get update
   sudo apt-get install indicator-netspeed
   ```

2. 禁止系统休眠

   ```shell
   sudo add-apt-repository ppa:caffeine-developers/ppa
   sudo apt-get update
   sudo apt-get install caffeine
   ##安装成功后，在终端中输入
   $caffine-indecator
   ```

3. 桌面及扁平化图标皮肤

   详细安装见[github链接](https://github.com/anmoljagetia/Flatabulous)，桌面壁纸自选。

   >以上界面配置均参考自[链接]([参考](http://www.360doc.com/content/16/0614/16/30532768_567725948.shtml)

4. 终端透明度设置

   在终端中点击右键选择Profiles  中的Profiles Preferences弹出编辑框，选择Color选中Use transprent background,拖动进度条。



## 自选配置

1. 微信

   没有官方Linux版本，但是有[替代品](http://blog.csdn.net/ch593030323/article/details/53571807)，字体略奇怪，表情包没有同步

2. md编辑器

   推荐Typora，但是鉴于长时间审美疲劳，偶尔换成moeditor护眼

3. 办公软件

   __据说__自带的办公软件不好用，所以可以卸载了安装WPS

   ```shell
   #一键卸载libreoffice
   $ sudo apt-get purge libreoffice? #这个？是通配符，不能省略
   #安装WPS
   ```

4. Sublime Text2

   不解释了看个人爱好

5. 修改系统时钟（关闭UTC）

## Attention

1. 在typora下，该md文档转成pdf后，shell代码块中所有的短划线-都不对（可能是半角和全角识别有误），如果需要粘贴代码，则从md文档中粘贴即可。其他md编辑器没有进行过测试。


2. 存了一版虚拟机环境，除了未安装网易云音乐，__基本配置__及__界面配置__都齐了。
