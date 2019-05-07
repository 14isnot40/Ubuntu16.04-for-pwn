# Ubuntu 16.04 PWN 环境配置
[TOC]
# 0x1 基本环境
## 换源
推荐阿里云和清华源。修改/etc/apt/sources.list；ubuntu16.04中自带源的备份了，换完后就可以愉快地更新一波了。

```shell
sudo apt-get update  #更新源  
sudo apt-get upgrade #更新软件
```
---
## vmare tools
虚拟机增强工具，用于增强虚拟设备、硬盘性能、以及驱动。
https://www.kancloud.cn/digest/javaframe/125585[https://www.kancloud.cn/digest/javaframe/125585](https://www.kancloud.cn/digest/javaframe/125585)

---
## pip
pip是Python下的一个包管理工具，在linux虚拟机下自带python2.7、python3会有pip，但需要升级。
```shell
#安装pip
sudo apt-get update #更新源
sudo apt-get install python-pip #安装pip
pip --version   #查看pip版本，确认是否安装成功
#升级pip
sudo pip install --upgrade pip  #Linux or MacOS
python -m pip install -U pip    #windows
```
---
## git
Git是一个开源的分布式版本控制系统，可以有效、高速的处理从很小到非常大的项目版本管理。 GitHub 则是在网络上提供 Git 仓库的一项服务。也就是说，GitHub 上公开的软件源代码全都由 Git 进行管理
```shell
#安装git工具
sudo apt-get install git
```
---
## sogo输入法
16.04就简单多了http://pinyin.sogou.com/linux/?r=pinyin 下载后直接安装
到系统设置->语言支持，将键盘输入法系统由默认的iBus设置为fcitx，同时如果没有中文的话安装中文并将其拖拽置最上方的语言
![](https://img-blog.csdn.net/20161029203358409?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
注销后重新登录再右上角的输入里就可以勾选sogo输入法了
## terminator + zsh + oh-my-zsh
**terminator** 支持对terminal的扩展，安装后可以在prefernce 里可以配置keyboarding快捷键和分屏等......
```shell
sudo apt-get install terminator
```
**zsh**是程序员必备的加强shell，功能丰富，界面更美观
安装zsh，重启后生效
```shell
# 安装zsh
apt-get install zsh
# 查看shell列表
cat /etc/shells
# 切换默认shell为zsh
chsh -s /bin/zsh
```
**oh-my-zsh**是为了方便使用zsh,安装如下：
```shell
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
```
主题配置
```shell
# 打开配置文件
vim ~/.zshrc 
# 修改
ZSH_THEME='bira'
#重置
source ~/.zshrc
```

自动补全插件 incr
```shell
# 下载
wget http://mimosa-pudica.net/src/incr-0.2.zsh
# 放置
cp incr-0.2.zsh ~/.oh-my-zsh/plugins/incr/
# 在~/.zshrc文件末尾加上
source ~/.oh-my-zsh/plugins/incr/incr*.zsh
# 更新配置
source ~/.zshrc   
```
---




# 0x2 调试相关
## pwntools
安装pwntools后会自动安装capstone、binutils等工具
- 1 Pwntools是一个CTF框架和漏洞利用开发库，它使用Python编写的，由rapid设计并维护，设计这个框架的目的是为了让使用者简单快速的编写exploit。

- 2 Capstone是一个反汇编框架，它的目标是成为最好的反汇编引擎，它是为安全社区中的研究人员进行二进制分析和逆向提供服务的。Core (Arm, Arm64, M68K, Mips, PPC, Sparc, SystemZ, X86, X86_64, XCore) + bindings (Python, Java, Ocaml, PowerShell)

- 3 GNU Binutils 是一组开发工具，包括连接器，汇编器和其他用于目标文件和档案的工具。包括下列程序: addr2line, ar, as, c++filt, gprof, ld, nm, objcopy, objdump, ranlib, readelf, size, strings 和 strip

经试验pip install --upgrade pwntools会报错，推荐使用git安装
```shell
git clone https://github.com/Gallopsled/pwntools
cd pwntools
python setup.py install
```
---

## Python-Dev 
python-devel是python的一个开发包，包括了一些用C/Java/C#等编写的python扩展在编译的时候依赖的头文件等信息。比如在编译一个用C语言编写的python扩展模块时，因为里面会有#include<Python.h>等这样的语句，需要先安装python-devel开发包。

```shell
apt-get install python-dev
```
---
## gdb + peda + pwndbg
unix平台调试器，到官方下载到最新版并安装，注意新建一个空文件夹进行操作(http://www.linuxfromscratch.org/blfs/view/cvs/general/gdb.html)
```shell
./configure --prefix=/usr \
            --with-system-readline \
            --with-python=/usr/bin/python3 

sudo make
sudo make install
```
遇到报错
```shell
WARNING: 'makeinfo' is missing on your system.
         You should only need it if you modified a '.texi' file, or
         any other file indirectly affecting the aspect of the manual.
         You might want to install the Texinfo package:
         <http://www.gnu.org/software/texinfo/>
         The spurious makeinfo call might also be the consequence of
         using a buggy 'make' (AIX, DU, IRIX), in which case you might
         want to install GNU make:
         <http://www.gnu.org/software/make/>
Makefile:486: recipe for target 'gdb.info' failed
#解决
apt-get install texinfo
```

---
## ROPgadget&rp++
ROPgadget是可以在二进制文件上搜索的小工具，以方便你的ROP利用。ROPgadget支持x86、x64、ARM、ARM64、PowerPC、SPARC和MIPS体系结构的ELF/PE/Mach-O格式。从版本5开始,ROPgadget有一个新的核心，它是用Python的Capstone disassembly框架编写的。
```shell
Capstone中带有ROPgadget。
```

rp++是一种c++编写的工具，目的是在PE/Elf/Mach-O(不支持FAT二进制文件)x86/x64二进制文件中找到ROP序列。它是开源的，已经在几个操作系统上测试过:Debian / Windows 7 / FreeBSD / Mac OSX Lion(10.7.3)。它是x64兼容的。

rp++ 从https://github.com/0vercl0k/rp/downloads 下载后安装
```shell
sudo cp rp-lin-x64 /bin/rp-lin-x64
chmod +x rp-lin-x64
./rp-lin-x64
```


# 0x3 编辑器
## vim
最简单的文本编辑器
```shell
sudo apt-get install vim
```
---
## sublime text3
---
## atom
---

# 0x4 编译相关
## gcc
ubuntu中自带有gcc，但如果需要对嵌入式架构上的设备进行程序上的编译，需要先根据设备架构内核编译对应版本的gcc。

## gcc-multilib
gcc-multilib gcc-multilib是Debian的64位系统下运行32位程序的一个库，装了这个库就可以运行32位的程序了。
```shell
sudo apt-get install gcc-multilib 
```


