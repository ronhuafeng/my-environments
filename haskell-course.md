# 安装 Haskell 平台
1. 在 Haskell 官网分别下对应平台的安装包
  - [Linux](https://www.haskell.org/platform/#linux)
  - [Windows](https://www.haskell.org/platform/#windows)
  - [OS X](https://www.haskell.org/platform/#osx)
2. 安装 IDE，[Haskell 官方 Wiki 的 IDE 介绍部分](https://wiki.haskell.org/IDEs)推荐了几款 IDE，其中以下几种组合可堪一用：
  - [IntelliJ IDEA](https://www.jetbrains.com/idea/) 配合 Haskell 插件
  - Eclipse 配合 EclipseFP 插件
  - [Atom](https://atom.io/) 配合 IDE-Haskell 插件 【[教程](https://atom.io/packages/ide-haskell)】； 有些功能并不完善，仍在开发之中
  - VIM/Emacs 用户可以自行配置选择 :-D
  
3. PPTP 无法连接，提示：*/usr/sbin/pppd: Plugin /usr/lib/pptpd/pptpd-logwtmp.so is for pppd version 2.4.5, this is 2.4.6* ，应当是版本不对。搜索后提示，将 * /etc/pptpd.conf* 中的 *logwtmp* 注释掉，但是文件中没有发现这个配置选项。好吧其实有的，就在第二行，我没有注意到。
   - 报错 *No CHAP secret found for authenticating 用户名* 
   - 然后目前无法上网
