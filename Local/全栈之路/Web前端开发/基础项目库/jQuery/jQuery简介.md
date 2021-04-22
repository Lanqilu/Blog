# JQuery简介

参考视频：[尚硅谷最新版JavaWeb全套教程](https://www.bilibili.com/video/BV1Y7411K7zz?p=62)

## JQuery介绍

1. 什么是 jQuery ? 

   jQuery，顾名思义，也就是 JavaScript 和查询（Query），它就是辅助 JavaScript 开发的 js 类。

2. jQuery 核心思想是什么 ？

   它的核心思想是 write less,do more(写得更少,做得更多)，所以它实现了很多浏览器的兼容问题。

3. jQuery 流行程度

   jQuery 现在已经成为最流行的 JavaScript 库，在世界前 10000 个访问最多的网站中，有超过 55%在使用 jQuery。

4. jQuery 好处

   jQuery 是免费、开源的，jQuery 的语法设计可以使开发更加便捷，例如操作文档对象、选择 DOM 元素、制作动画效果、事件处理、使用 Ajax 以及其他

## jQuery 入门

```html
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <title>Insert title here</title>
    <script type="text/javascript">
        window.onload = function () {
            let btnObj = document.getElementById("btnId");
            btnObj.onclick = function () {
                alert("JavaScript原生单击事件")
            }
        }
    </script>
</head>
<body>

<button id="btnId">SayHello</button>

</body>
</html>
```

需求：使用 jQuery 给一个按钮绑定单击事件

```html
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <title>Insert title here</title>

    <script type="text/javascript" src="https://cdn.bootcdn.net/ajax/libs/jquery/3.6.0/jquery.js"></script>
    <script type="text/javascript">
        //使用$()代替window.onload
        $(function () {
            //使用选择器获取按钮对象，随后绑定单击响应函数
            $("#btnId").click(function () {
                //弹出Hello
                alert('Hello');
            });
        });

    </script>
</head>
<body>

<button id="btnId">SayHello</button>

</body>
</html>
```

