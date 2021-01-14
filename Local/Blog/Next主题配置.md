---
title: Next主题配置
date: 2019-06-20 12:42:30
tags:
- Next
- Hexo
categories:
- Blog
---

{% note info no-icon %}
Next主题相关配置
基于[Next 7.7+](https://github.com/theme-next/hexo-theme-next)
{% endnote %}

<!--more-->

---

## 我的配置文件

```yml
# Remove unnecessary files after hexo generate.
minify: true

# Define custom file paths.
# Create your custom files in site directory `source/_data` and uncomment needed files below.
custom_file_path:
  style: source/_data/styles.styl

footer:
  # Specify the date when the site was setup. If not defined, current year will be used.
  since: 2019

  # Icon between year and copyright info.
  icon:
    # Icon name in Font Awesome. See: https://fontawesome.com/v4.7.0/icons/
    # `heart` is recommended with animation in red (#ff0000).
    name: heart
    # If you want to animate the icon, set it to true.
    animated: true
    # Change the color of icon, using Hex Code.
    color: "#743481"

    # Powered by Hexo & NexT
    powered: false

# Schemes
# 主题方案
scheme: Gemini

# Dark Mode
darkmode: true

menu:
  home: / || home
  #about: /about/ || user
  tags: /tags/ || tags
  categories: /categories/ || th
  archives: /archives/ || archive

# Enable / Disable menu icons / item badges.
menu_settings:
  icons: true
  badges: true

avatar:
  # Replace the default image and set the url here.
  url:  http://img.whl123456.top/image/avatar.jpg

social:
  GitHub: https://github.com/lanqilu || github
  E-Mail: http://mail.qq.com/cgi-bin/qm_share?t=qm_mailme&email=zqKvoL_noruOqKG2o6_nouCtoaM || envelope

social_icons:
  enable: true
  icons_only: true

# Use icon instead of the symbol # to indicate the tag at the bottom of the post
tag_icon: true

# Related popular posts
# Dependencies: https://github.com/tea3/hexo-related-popular-posts
# 需要安装插件 npm install hexo-related-popular-posts --save
related_posts:
  enable: false
```

## 侧边栏设置

打开`\themes\next\_config.yml`搜索`muen`
我的配置如下

```yml
menu:
  home: / || home
  archives: /archives/ || list-ul
  tags: /tags/ || tags
  categories: /categories/ || th
  Python: /categories/Python/ || code
  English: /categories/English/ || check-square-o
  Myself: /categories/Myself/ || street-view
  Blog: /categories/Blog/ || chevron-right
```

NexT 使用的是 [Font Awesome](http://fontawesome.dashgame.com/) 提供的图标
可以到该网站寻找适合自己的图标

## 站内搜索

本地搜索
在你站点的根目录下
`$ npm install hexo-generator-searchdb --save`

打开 Hexo 站点的 _config.yml,添加配置

```yml
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
```

打开 themes/next/_config.yml ,搜索关键字 local_search ,设置为 true：

```shell
local_search:
  enable: true
```

## 加载条

[theme-next-pace](https://github.com/theme-next/theme-next-pace)

```shell
cd themes/next
git clone https://github.com/theme-next/theme-next-pace source/lib/pace
```

在主题设置yml文件修改pace

```yml
pace:
  enable: true
  # Themes list:
  # big-counter | bounce | barber-shop | center-atom | center-circle | center-radar | center-simple
  # corner-indicator | fill-left | flat-top | flash | loading-bar | mac-osx | material | minimal
  theme: minimal
```

可以到[此网站](https://github.hubspot.com/pace/docs/welcome/)copy样式，复制到自定义样式styl文件中

## 静态资源压缩

{% note warning %}
gulp在4.0之后，以下方法无法顺利运行
{% endnote %}

参考[文章](https://zhousiwei.gitee.io/2019/07/22/Hexo%E5%8D%9A%E5%AE%A2%E4%BD%BF%E7%94%A8gulp%E5%8E%8B%E7%BC%A9%E9%9D%99%E6%80%81%E8%B5%84%E6%BA%90/)

在站点目录下：
`$ npm install gulp -g`

安装gulp插件：

```shell
npm install gulp-minify-css --save
npm install gulp-uglify --save
npm install gulp-htmlmin --save
npm install gulp-htmlclean --save
npm install gulp-imagemin --save
```

在 Hexo 站点下添加 gulpfile.js文件，文件内容如下：

```js
var gulp = require('gulp');
var minifycss = require('gulp-minify-css');
var uglify = require('gulp-uglify');
var htmlmin = require('gulp-htmlmin');
var htmlclean = require('gulp-htmlclean');
var imagemin = require('gulp-imagemin');
// 压缩css文件
gulp.task('minify-css', function() {
  return gulp.src('./public/**/*.css')
  .pipe(minifycss())
  .pipe(gulp.dest('./public'));
});
// 压缩html文件
gulp.task('minify-html', function() {
  return gulp.src('./public/**/*.html')
  .pipe(htmlclean())
  .pipe(htmlmin({
    removeComments: true,
    minifyJS: true,
    minifyCSS: true,
    minifyURLs: true,
  }))
  .pipe(gulp.dest('./public'))
});
// 压缩js文件
gulp.task('minify-js', function() {
    return gulp.src(['./public/**/.js','!./public/js/**/*min.js'])
        .pipe(uglify())
        .pipe(gulp.dest('./public'));
});
// 压缩 public/demo 目录内图片
// gulp.task('minify-images', function() {
//     gulp.src('./public/demo/**/*.*')
//         .pipe(imagemin({
//            optimizationLevel: 5, //类型：Number  默认：3  取值范围：0-7（优化等级）
//            progressive: true, //类型：Boolean 默认：false 无损压缩jpg图片
//            interlaced: false, //类型：Boolean 默认：false 隔行扫描gif进行渲染
//            multipass: false, //类型：Boolean 默认：false 多次优化svg直到完全优化
//         }))
//         .pipe(gulp.dest('./public/uploads'));
// });
// 默认任务
gulp.task('default', [
  'minify-html','minify-css','minify-js','minify-images'
]);
```

只需要每次在执行 generate 命令后执行 gulp 就可以实现对静态资源的压缩，压缩完成后执行 deploy 命令同步到服务器：

```shell
hexo g
gulp
hexo d
```

## 参考资料

[Hexo+Next个人博客主题优化](https://www.jianshu.com/p/efbeddc5eb19)