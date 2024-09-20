+++
author = "YYK"
title = "【指南】oh-my-zsh 安装"
date = "2024-09-20"
categories = [
    "技术",
]
tags = [
    "指南",
]

+++

# 为什么要安装`ZSH`？
`ZSH`是类似于`BASH`的命令解释器。相比于`BASH`，`ZSH`有更多的自定义选项，并且支持扩展，可以实现更强大的命令补全、命令高亮等一系列炫酷实用功能。

# 什么是`oh-my-zsh`?
虽然 ZSH 功能强大，但配置比较复杂。为了简化配置，大佬 **robbyrussell** 制作了一份开源配置文件 [oh-my-zsh](https://github.com/ohmyzsh/ohmyzsh)，只需要几行命令就能完成 ZSH 的配置。安装`oh-my-zsh`可以享受到以下特性：
● 兼容bash
● 自动cd，只需输入目录的名称即可
● 命令选项补齐，比如输入git，然后按Tab，即可显示出git都有哪些命令
● 目录一次性补全：比如输入Doc/doc按Tab键会自动变成Documents/document/
● 200+插件 和 140+主题支持（插件能进一步提升效率）
● 等等等

# 安装`ZSH`
## step_1: 安装 zsh
```bash
# centos
yum install -y zsh

# mac
brew install zsh

```
## step_2: 安装oh-my-zsh
| Method | Command |
| ------ | ------- |
| curl   | `sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"` |
| wget   | `sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"` |
| fetch  | `sh -c "$(fetch -o - https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"` |


## step_3: 安装常用插件
```bash
# zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting

# zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions

```

## step_4: 启用插件
```bash
# 打开配置
vim ~/.zshrc

# 编辑配置
...
plugins=(git zsh-autosuggestions zsh-syntax-highlighting)
...

# 应用配置
source ~/.zshrc
```
