---
title: Rime配置默认中文简体
date: 2025-03-22 10:02:28
cover: /img/rime/cover.png
categories: linux
tags:
    - tool
    - config
---

{% colorquote warning %}
不同的`linux distro`关于rime的配置文件的位置不同，可以先查询自己`distro`的配置文件位置后再按照下列步骤进行。
{% endcolorquote %}

#### 前言

我们知道：

1）`Rime`是一个输入法引擎，是一个后台处理程序。

2）我们需要一个交互的`GUI`前端来实现与`Rime`的交互。

3）`Rime`依赖配置文件来自定义输入法相关操作。

4）`Rime`的配置文件位置和使用的gui前端相关。

#### 实践

{% colorquote info %}
os: Pop_os!
env: fcitx5 + rime
{% endcolorquote %}

**以下省略安装相关工具过程**

安装相关工具完成后，先配置`fcitx5`加入`Rime`输入引擎，找到`Rime`的配置文件目录： `~/.config/fcitx/rime`，这里的配置文件目录很大程度上取决于使用的`gui`前端工具，所以按照自己的实际找到即可。

在这个目录下，能看到许多的`yaml`文件和一个build目录，如果没有build目录，可能是没有启用输入法的原因，但是并不妨碍接下来的操作。

首先，确保`default.yaml`下的`schema_list`包含安装的输入法，如：`luna_pinyin_simp`，这里的`schema`是可以调整顺序的，也就是说有一种快速的方案可以实现默认中文简体，以下详细给出具体实现。

1) 修改`default.yaml`下的默认输入法方案：

```yaml
schema_list:
    - schema: luna_pinyin_simp
    - scheme: ...
```

2) 修改`luna_pinyin`的配置文件：

```shell
vim luna_pinyin.custom.yaml
```

```yaml    
patch:
 switches:
  - name: ascii_mode
    reset: 0
    states: [ 中文, 西文 ]
  - name: full_shape
    states: [ 半角, 全角 ]
  - name: simplification
    reset: 1
    states: [ 漢字, 汉字 ]
  - name: ascii_punct
    states: [ 。，, ．， ]
```

以上为2种解决方案，选择一种即可，完成后执行：

```shell
rime_deployer --build
```

然后就能够看到`build/`了，重启`gui`前端，现在应当是默认的中文简体了。

#### 后话

很多时候，我们会盲目地寻求快速的解决方案，进而导致我们往往会忽视一些潜在的问题，别人的配置不一定适用于自己，自己的配置也不一定适用于别人。这时，不妨慢下来了解其中的原理与细节，我们会在自己细腻的思考中得到想要的答案，同时收获满满。


