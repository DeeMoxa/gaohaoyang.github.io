---
layout: post
title: Mac环境开发汇总
tags:
  - mac
  - homebrew
  - gem
  - ruby
  - jekyll
  - redis
  - idea
---

##  Homebrew
Red hat有yum，Ubuntu有apt-get而mac第三方支持：Homebrew简称brew， 是Mac OSX上的软件包管理工具，能在Mac中方便的安装软件或者卸载软件
默认安装路径为：/usr/loacl/Cellar  

brew         | 相关命令
------------ | ---------------------------------------
安装任意包        | `brew install [packageName]`
安装wget       | `brew install wget`
卸载任意包        | `brew uninstall [packageName]`
卸载git        | `brew uninstall git`
查询可用包        | `brew search [packageName]`
查看已安装包列表     | `brew info [packageName]`
删除该gem包      | gem uninstall [gemname]
删除某指定版本gem   | gem uninstall [gemname] --version=[ver]
更新Homebrew   | `brew update`
查看Homebrew版本 | `brew -v [packageName]`
Homebrew帮助信息 | `brew -h [packageName]`

##  ruby(脚本语言)

```
 ruby -e "$(curl -fsSL
    <https://raw.githubusercontent.com/Homebrew/install/master/install>)"
     ==>This script will install: /usr/local/bin/brew
      /usr/local/Library/... /usr/local/share/man/man1/brew.1
```
```
 Press RETURN to continue or any other key to abort ==>Downloading and
  installing Homebrew... remote: Counting objects: 3693, done.
  remote: Compressing objects: 100% (3525/3525), done.
  remote: Total 3693 (delta 38), reused 527 (delta 27), pack-reused 0 Receiving objects: 100% (3693/3693), 3.04 MiB | 79.00 KiB/s, done. Resolving deltas: 100% (38/38), done.
  From <https://github.com/Homebrew/homebrew>

  [new branch] master -> origin/master HEAD is now at 9c41fb8
  update man page ==>Installation successful! ==>
  Next steps Run `brew help` to get started
```
##  [gem](https://rubygems.org/pages/download)(ruby程序的包管理器类似Homebrew) [国内源站](https://gems.ruby-china.org)

gem                                                   | 相关命令
----------------------------------------------------- | ------------------------------------------------
查看源列表                                                 | gem sources -l
将源移除                                                  | gem sources --remove <https://rubygems.org/>
添加国内源                                                 | gem sources --add <https://gems.ruby-china.org/>
缓存                                                    | gem sources -u
查看gem安装环境（gem命令安装的软件在 GEM PATHS 中的lib path目录gems文件夹下） | gem environment

##  [jekyll](https://www.jekyll.com.cn) (git page)

```
 gem install jekyll
```


##  MAC下如何显示隐藏文件
```
//在终端上输入以下命令 > defaults write com.apple.finder AppleShowAllFiles -bool true
//重新启动Finder > 使用快捷键 Command + Option + esc
或者执行 killall Finder命令 这样就可以显示隐藏文件了。
//不显示隐藏文件的命令为 > defaults write com.apple.finder AppleShowAllFiles -bool false
同样也需要东西启动Finder才生效。
```

##  secureCRT
```
1、下载破解文件 securecrt_mac_crack.pl
2、在终端执行命令。（请注意对应文件的目录） sudo perl ~/Downloads/securecrt_mac_crack.pl /Applications/SecureCRT.app/Contents/MacOS/SecureCRT
等到出现了 crack successful就可以了，终端不要关闭
3、打开SecureCRT，点击Enter License Data.. 输入刚才终端的数据就完成了破解，
破解信息在刚才终端窗口，你的可能和我的不一样，以终端显示的为准。
（请注意逐一拷贝，不要一次拷贝）
Name: bleedfly Company: bleedfly.com
Serial Number: 03-29-002542
License Key: ADGB7V 9SHE34 Y2BST3 K78ZKF ADUPW4 K819ZW 4HVJCE P1NYRC
Issue Date: 09-17-2013
```

##  mac下 idea debugger [加载慢的问题](https://stackoverflow.com/questions/20658400/intellij-idea-hangs-while-finished-saving-caches)

## [launching rocket](https://github.com/jimbojsb/launchrocket)
```
A Mac PreferencePane for managing services with launchd.
```
