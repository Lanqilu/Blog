---
title: Python有关配置
date: 2019-10-12 22:09:13
tags:
- Pyhton
categories:
- Python
---
{% note info no-icon %}
包括创建Python虚拟环境、Jupter Notebook、VScode
{% endnote %}
<!--more-->



## Python虚拟环境的创建及其使用

### 创建

终端在创建文件夹内
python -m venv  XXXX(venv名称)

### 激活

XXXX\Scripts\activate

### 退出

deactivate

## Jupyter Notebook

### 设置Jupyter Notebook主题

1. 在cmd下用pip 安装jupyter-themes：`pip install --upgrade jupyterthemes`
2. 使用`jt -l`命令查看可以使用的主题
3. 使用`jt -t [主题名称]`更换主题
4. 使用`jt -f [字体名称]`更换字体

### 配置

```shell
jt -t grade3 -T -f source -tf robotosans -tfs 12 -nf robotosans -nfs 13 -cellw 90%
```



## VScode中使用Python

### 使用Jupyter

`#%%` 是Jupyter的预定义符号, 写上它就可以开始愉快地写代码了! 写了`#%%`这个之后就多了一个 Run cell在代码上面, 点击就可以跑出结果了

`#%%[]`可以编写Markdown文本

> 最新版的Python插件已经可以愉快的编辑Jupyter文件

### 插件

+ Chinese (Simplified) Language Pack for Visual Studio Code
+ One Dark Pro
+ Python
+ vscode-icons
+ Settings Sync

### 插件推荐

[相关博客文章](http://www.huangpan.net/?p=2076)

### [同步配置](<https://www.cnblogs.com/kenz520/p/7416836.html>)

1. 上传： Shift + Alt + U (Sync: Update / Upload Settings)
2. 下载： Shift + Alt + D (Sync: Download  Settings)