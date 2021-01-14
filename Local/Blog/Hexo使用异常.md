---
title: Hexo使用异常
tags:
  - Hexo
  - 异常
categories:
  - Hexo
date: 2020-03-22 13:17:31

---

{% note info no-icon %}
用于收集Hexo博客使用的异常情况，以及解决方法
{% endnote %}

<!-- more -->

<!-- hexo clean && hexo g && hexo d -->

---

## Cannot read property 'replace' of null

在使用`hexo s`命令来预览时，可以进入主界面，但点击文章无响应
报错内容如下

```powershell
E:\Desktop\hexo>hexo s
INFO  Start processing
INFO  Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.
ERROR Render HTML failed: 2020/03/21/微生物学/微生物学_第1章_原核生物的形态、构造和功能/index.html
TypeError: Cannot read property 'replace' of null
    at Hexo.externalLinkFilter (E:\Desktop\hexo\node_modules\hexo\lib\plugins\filter\after_render\external_link.js:22:15)
    at Hexo.tryCatcher (E:\Desktop\hexo\node_modules\bluebird\js\release\util.js:16:23)
    at Hexo.<anonymous> (E:\Desktop\hexo\node_modules\bluebird\js\release\method.js:15:34)
    at Promise.each.filter (E:\Desktop\hexo\node_modules\hexo\lib\extend\filter.js:62:52)
    at tryCatcher (E:\Desktop\hexo\node_modules\bluebird\js\release\util.js:16:23)
    at Object.gotValue (E:\Desktop\hexo\node_modules\bluebird\js\release\reduce.js:166:18)
    at Object.gotAccum (E:\Desktop\hexo\node_modules\bluebird\js\release\reduce.js:155:25)
    at Object.tryCatcher (E:\Desktop\hexo\node_modules\bluebird\js\release\util.js:16:23)
    at Promise._settlePromiseFromHandler (E:\Desktop\hexo\node_modules\bluebird\js\release\promise.js:547:31)
    at Promise._settlePromise (E:\Desktop\hexo\node_modules\bluebird\js\release\promise.js:604:18)
    at Promise._settlePromiseCtx (E:\Desktop\hexo\node_modules\bluebird\js\release\promise.js:641:10)
    at _drainQueueStep (E:\Desktop\hexo\node_modules\bluebird\js\release\async.js:97:12)
    at _drainQueue (E:\Desktop\hexo\node_modules\bluebird\js\release\async.js:86:9)
    at Async._drainQueues (E:\Desktop\hexo\node_modules\bluebird\js\release\async.js:102:5)
    at Immediate.Async.drainQueues [as _onImmediate] (E:\Desktop\hexo\node_modules\bluebird\js\release\async.js:15:14)
    at runCallback (timers.js:705:18)
    at tryOnImmediate (timers.js:676:5)
    at processImmediate (timers.js:658:5)
Unhandled rejection TypeError: Cannot read property 'replace' of null
    at Hexo.externalLinkFilter (E:\Desktop\hexo\node_modules\hexo\lib\plugins\filter\after_render\external_link.js:22:15)
    at Hexo.tryCatcher (E:\Desktop\hexo\node_modules\bluebird\js\release\util.js:16:23)
    at Hexo.<anonymous> (E:\Desktop\hexo\node_modules\bluebird\js\release\method.js:15:34)
    at Promise.each.filter (E:\Desktop\hexo\node_modules\hexo\lib\extend\filter.js:62:52)
    at tryCatcher (E:\Desktop\hexo\node_modules\bluebird\js\release\util.js:16:23)
    at Object.gotValue (E:\Desktop\hexo\node_modules\bluebird\js\release\reduce.js:166:18)
    at Object.gotAccum (E:\Desktop\hexo\node_modules\bluebird\js\release\reduce.js:155:25)
    at Object.tryCatcher (E:\Desktop\hexo\node_modules\bluebird\js\release\util.js:16:23)
    at Promise._settlePromiseFromHandler (E:\Desktop\hexo\node_modules\bluebird\js\release\promise.js:547:31)
    at Promise._settlePromise (E:\Desktop\hexo\node_modules\bluebird\js\release\promise.js:604:18)
    at Promise._settlePromiseCtx (E:\Desktop\hexo\node_modules\bluebird\js\release\promise.js:641:10)
    at _drainQueueStep (E:\Desktop\hexo\node_modules\bluebird\js\release\async.js:97:12)
    at _drainQueue (E:\Desktop\hexo\node_modules\bluebird\js\release\async.js:86:9)
    at Async._drainQueues (E:\Desktop\hexo\node_modules\bluebird\js\release\async.js:102:5)
    at Immediate.Async.drainQueues [as _onImmediate] (E:\Desktop\hexo\node_modules\bluebird\js\release\async.js:15:14)
    at runCallback (timers.js:705:18)
    at tryOnImmediate (timers.js:676:5)
    at processImmediate (timers.js:658:5)
```

### 解决思路

通过搜索引擎寻找相关报错解决方案例如：[简书一方案](https://www.jianshu.com/p/449accb044b4)
但我从未修改Hexo站点中的该配置项目,多次尝试无果

```yml
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: http://yoursite.com
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:
pretty_urls:
  trailing_index: true # Set to false to remove trailing 'index.html' from permalinks
  trailing_html: true # Set to false to remove trailing '.html' from permalinks

```

于是我尝试将我修改的Next主题的配置文件替换成默认配置，发现不报错了
于是乎问题缩小到了Next主题配置文件中

### 解决方法

通过推测很有可能是插件出现了问题
于是在配置文件中将安装使用的插件部分关闭，再尝试运行`hexo s`

很快找到了问题插件

```yml
# Related popular posts
# Dependencies: https://github.com/tea3/hexo-related-popular-posts
related_posts:
  enable: false
  title: # Custom header, leave empty to use the default one
  display_in_home: false
  params:
    maxCount: 5
    #PPMixingRate: 0.0
    #isDate: false
    #isImage: false
    #isExcerpt: false
```

通过重新安装该插件，问题顺利解决。

### 原因分析

问题造成的原因很可能是我在使用npm来安装插件，因为安装插件失败，于是我就将安装的插件卸载，导致其他插件的依赖收到了影响。
