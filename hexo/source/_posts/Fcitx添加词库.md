---
title: Fcitx添加词库
date: 2019-04-27 10:27:51
tags: [fcitx]
categories: [应用程序]
---

# 前言
作为全球最大的Linux开源输入法，fcitx保持了一贯的高效、简捷。
fcitx官方给出了原生生成细胞词库的接口，以适应中文输入中专有、常用语句的拼写。

本文将介绍怎样转换、导入狗细胞词库到fcitx的拼音词库中。


<!-- more -->


# 安装
{% note info %} 安装脚本我已上传至 [github](https://github.com/AlessandroChen/fcitx-pinyin-lexicon) 安装方法附在 `README.md`中 {% endnote %}

首先到[搜狗细胞词库](https://pinyin.sogou.com/dict/)下载你需要用到的词库，到`~/Downloads/`目录下

然后，`clone`我上传好的文件和脚本
```bash
$ git clone https://github.com/AlessandroChen/fcitx-pinyin-lexicon.git
```

你可以使用`fcitx-pinyin-lexicon`下的`install.sh`直接进行安装(**强烈推荐**)，也可以按照以下步骤逐步完成：

```bash
$ cd fcitx-pinyin-lexicon
$ mkdir tmp && cd tmp
$ mv ~/Downloads/*.scel ./

# 转换为 org 文件
$ mkdir org
$ for i in *.scel; do scel2org $i -o org/$i.org; done
$ cp ../pyPhrase.org org/

# 生成词库
$ mkdir dict && cd dict
$ cat ../org/* | sort | uniq > ultimate.org
$ createPYMB ../../gbkpy.org ultimate.org
```
转换过程可能比较长，耐心等待。最后会生成如下几个文件：

* pyERROR 词库中重复或有问题的条目
* pyPhrase.ok 除错后的org词库文件
* pyphrase.mb 最终词库
* pybase.mb 配套的字码库


下面将词库放在 fcitx 的目录下，然后重启 fcitx 就完成了
```bash
$ mv pyphrase.mb ~/.config/fcitx/pinyin/
$ mv pybase.mb ~/.config/fcitx/pinyin/
```

