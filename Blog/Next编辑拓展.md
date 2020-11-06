---
title: Next编辑拓展
date: 2019-06-24 18:22:54
tags: 
- Hexo
- Next
categories:
- Blog
mathjax: true
---
{% note info no-icon %}
Next在文章编辑时有许多优秀的拓展语法
{% endnote %}

<!--more-->

![Next编辑拓展](https://puui.qpic.cn/fans_admin/0/3_1231830080_1572055658567/0)

## 标签（Label）

### 支持的类型

```shell
{% note primary %}primary{% endnote %}
{% note success %}success{% endnote %}
{% note info %}info{% endnote %}
{% note warning %}warning{% endnote %}
{% note danger %}danger{% endnote %}
{% note default no-icon %}default no-icon{% endnote %}
```

{% note primary %}primary{% endnote %}
{% note success %}success{% endnote %}
{% note info %}info{% endnote %}
{% note warning %}warning{% endnote %}
{% note danger %}danger{% endnote %}
{% note default no-icon %}default no-icon{% endnote %}

## 标签（Tabs）

### 基本样式

```shell
{% tabs 小标签 %}
<!-- tab -->
**内容1**
<!-- endtab -->
<!-- tab -->
**内容2**
<!-- endtab -->
<!-- tab -->
**内容3**
<!-- endtab -->
{% endtabs %}
```

{% tabs 小标签 %}
<!-- tab -->
内容1
<!-- endtab -->

<!-- tab -->
内容2
<!-- endtab -->

<!-- tab -->
内容3
<!-- endtab -->
{% endtabs %}

### 选择默认指定标签

```shell
{% tabs 小标签, 2 %}
<!-- tab -->
**内容1**
<!-- endtab -->

<!-- tab -->
**默认显示第2个标签**
<!-- endtab -->

<!-- tab -->
**内容3**
<!-- endtab -->
{% endtabs %}
```

{% tabs 小标签, 2 %}
<!-- tab -->
内容1
<!-- endtab -->

<!-- tab -->
默认显示第2个标签
<!-- endtab -->

<!-- tab -->
内容3
<!-- endtab -->
{% endtabs %}

```html
{% tabs 默认不显示标签, -1 %}
<!-- tab -->
**内容1**
<!-- endtab -->

<!-- tab -->
**内容2**
<!-- endtab -->

<!-- tab -->
**内容3**
<!-- endtab -->
{% endtabs %}
```

{% tabs 默认不显示标签, -1 %}
<!-- tab -->
内容1
<!-- endtab -->

<!-- tab -->
内容2
<!-- endtab -->

<!-- tab -->
内容3
<!-- endtab -->
{% endtabs %}

## 文本居中

```html
{% cq %}
居中文本
{% endcq %}
```

{% cq %}
居中文本
{% endcq %}

## HTML

### 分两栏

<html>
    <table style="margin-left: auto; margin-right: auto;">
        <tr>
            <td>
                <!--左侧内容-->
                左侧
            </td>
            <td>
                <!--右侧内容-->
                右侧
            </td>
        </tr>
    </table>
</html>

```html
<html>
    <table style="margin-left: auto; margin-right: auto;">
        <tr>
            <td>
                <!--左侧内容-->
                左侧
            </td>
            <td>
                <!--右侧内容-->
                右侧
            </td>
        </tr>
    </table>
</html>
```

### `<details>`标签

```HTML
<details>
<summary>第一章</summary>
* 1.1
* 1.2
</details>
```

<details>
<summary>第一章</summary>

* 1.1
* 1.2

</details>

---

### `<audio>`标签

```HTML
<audio src="http://music.163.com/song/media/outer/url?id=28713454.mp3" controls="controls">
</audio>
```

<details>
<summary>展示</summary>

<audio src="http://music.163.com/song/media/outer/url?id=28713454.mp3" controls="controls">
</audio>

</details>

---

### `<iframe>`标签

```HTML
<iframe src="https://www.bilibili.com/video/av38501240" 
sandbox="allow-forms allow-scripts allow-same-origin allow-popups"
width=850px height=570px>
</iframe>
```

<details>
<summary>展示</summary>

<iframe src="https://www.bilibili.com/video/av38501240" 
sandbox="allow-forms allow-scripts allow-same-origin allow-popups"
width=850px height=570px>
</iframe>

</details>

---

### `<center>`标签

```HTML
<center>居中显示</center>
```

<details>
<summary>展示</summary>

<center>居中显示</center>
</details>

### `name`属性

```html
# <a name="C0">目录</a>
[回到目录](#C0)
```

[回到目录](#C0)

## JavaScript

### 显示隐藏的 HTML 元素

```HTML
<p id="demo" style="display:none">Hello JavaScript!</p>
<button type="button" onclick="document.getElementById('demo').style.display='block'">
点击我！
</button>
```

<p id="demo" style="display:none">Hello JavaScript!</p>
<button type="button" onclick="document.getElementById('demo').style.display='block'">
点击我！
</button>

## 参考资料

[Next官方文档](https://theme-next.org/docs/tag-plugins/)
