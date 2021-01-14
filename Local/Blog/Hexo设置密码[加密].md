---
title: 'Hexo设置密码'
date: 2019-06-20 15:13:30
tags:
- Next
- Hexo
categories:
- Blog
password: w123456
abstract: 该文章已加密🔒，如需访问请输入密码🔑
message: 本文密码：w123456
---

{% note info no-icon %}
本章介绍设置文章密码
{% endnote %}

<!-- more -->

---

## 插件及其功能

### 插件

[Hexo-Blog-Encrypt](https://github.com/MikeCoder/hexo-blog-encrypt)
[说明文档](https://github.com/MikeCoder/hexo-blog-encrypt/blob/master/ReadMe.zh.md)

### 功能

给文章设置密码
如果密码输入正确，30分钟内，无需密码即可访问该网页

## 安装方法

```shell
npm install hexo-blog-encrypt --save
```

如果有安装cnpm，同样可以

```shell
cnpm install hexo-blog-encrypt --save
```

## 配置步骤

1. 在Hexo下的 _config.yml 中启用该插件,添加以下代码：

   >不是主题文件夹下

    ```yml
    # Security
    ##
    encrypt:
        enable: true
    ```

2. 在文章开头属性中添加相应属性

> 保证文章内容非空，负责会报错

```markdown
---
title: Hexo博客密码设置
date: 2019-06-20 15:13:30
tags:
- Next
- Hexo
password: 123456
abstract: ！该文章已加密，如需访问请输入密码
message: 本文密码：123456
---
```

+ `psaaword` ——> 密码
+ `abstract` ——> 摘要内容
+ `message`  ——> 默认提示
