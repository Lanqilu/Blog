---
title: Hexo插件使用
date: 2020-03-21 16:35:41
tags:
- Hexo
categories:
- Hexo
mathjax: true
---

{% note info no-icon %}
本文介绍使用的Hexo插件，使用第三方插件提高了Hexo博客的拓展性
{% endnote %}

<!-- more -->

---

## 公式方面

[`hexo-renderer-pandoc`](https://github.com/wzpan/hexo-renderer-pandoc)

在安装前需要安装[pandoc软件](https://pandoc.org/installing.html#windows)

> pandoc 一款将不同文档格式相互转换的软件

安装命令

```shell
cnpm install hexo-renderer-pandoc
```

页面属性(front-matter)中需开启mathjax：true

```latex
$$f(x,\mu, \sigma )=\frac{1}{\sigma\sqrt{2\pi}}e^{-\frac{(x-\mu )^{2}}{2\sigma ^{2}}}$$
```

$$f(x,\mu, \sigma )=\frac{1}{\sigma\sqrt{2\pi}}e^{-\frac{(x-\mu )^{2}}{2\sigma ^{2}}}$$

## 密码方面

详情可以查看该文章[Hexo设置密码](https://lanqilu.github.io/2019/06/20/Hexo%E8%AE%BE%E7%BD%AE%E5%AF%86%E7%A0%81%5B%E5%8A%A0%E5%AF%86%5D/)

## 远端部署

1. 安装GitHub插件`cnpm install -g hexo-deployer-git --save`
2. 在Github上创建项目库
   {% note warning %}项目库命名要是 用户名+.github.io 例如：lanqilu.github.io
   {% endnote %}
3. 设置 _config.yml 文件

   ```yml
   # Deployment
   ## Docs: https://hexo.io/docs/deployment.html
   deploy:
   type: git
   repo: https://github.com/<GitHub_name>/<GitHub_repositories>.git
   branch: master
   ```

4. 部署到远端`hexo d`
   填写github账号密码
5. 常用部署命令
   cmd`hexo clean && hexo g && hexo d`
   powershell`hexo clean '&' hexo g '&' hexo d`
